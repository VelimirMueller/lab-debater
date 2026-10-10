# lab-debater — reference

Deep reference for [the README](../README.md). Everything here is fact, kept from the v1
README: the example output, the full architecture, the trace-reading guide, the project
structure and the experiment list.

## Example output (excerpt)

```
TOPIC: Is transparent algorithmic curation preferable to editorial human curation...
[MODERATOR, round 0]
Should news platforms be required to replace editorial human curation with
transparent algorithmic curation, given that algorithms can be audited for bias
while human editors cannot be held to the same standard of systematic accountability?

[PROPOSER, round 1]
## The Case for Transparent Algorithmic Curation
The core claim is straightforward: transparent algorithmic curation is preferable...

[CRITIC, round 1]
## Counter-Claim: Auditability Is Not Accountability, and Transparency Is Not Neutrality
The proposer has constructed the strongest version of the algorithmic case...

[JUDGE, round 2]
VERDICT: The critic carried the debate on the motion as framed. The proposer's
case rested on the structural superiority of auditable, correctable algorithmic
failure, but the critic successfully narrowed this to a contingent empirical
claim about interpretability that has not been discharged for production systems
at operational scale...
```

Debate outcome, full transcript, and both trace URLs are printed on completion.

## The whole system

```text
  python main.py "<proposition>"
     │  load .env · check keys · build_graph() · build_run_config()
     ▼
  ┌─────────────────────────────┐  tool_calls   ┌────────────────────┐
  │  LangGraph StateGraph       │ ────────────▸ │  Tavily            │
  │  setup · moderator          │ ◂──────────── │  search · extract  │
  │  proposer · critic  (ReAct) │  ToolMessage  └────────────────────┘
  │  moderator · judge          │
  │  verdict set? ── no ─▸ loop │  invoke()     ┌────────────────────┐
  │               └─ yes ─▸ END │ ────────────▸ │  Claude Sonnet 4.6 │
  └──────────────┬──────────────┘  retry-on-429 └────────────────────┘
                 │  every node, LLM call and tool call is a span
         ┌───────┴─────────┐
         ▼                 ▼
  ┌──────────────┐  ┌──────────────┐
  │  LangSmith   │  │  Langfuse    │
  │  env vars    │  │  callback    │
  └──────────────┘  └──────────────┘

  stdout ◂── transcript · VERDICT: line · two trace URLs
```

## Architecture

The whole agent is a LangGraph `StateGraph` with six nodes and one conditional edge:

```
                  ┌─────────┐
  START ─▶ setup ─▶moderator│ (opens: frames question)
                  └────┬────┘
                       ▼
                 ┌─────────┐
           ┌────▶│proposer │
           │     └────┬────┘
           │          ▼
           │     ┌─────────┐
           │     │ critic  │
           │     └────┬────┘
           │          ▼
           │     ┌─────────┐
           │     │moderator│ (distils round, poses sharper follow-up)
           │     └────┬────┘
           │          ▼
           │     ┌─────────┐
           │     │ judge   │ (continue? or emit VERDICT)
           │     └────┬────┘
           │          │
           │    ┌─────┴─────┐
           └────│ continue? │───▶ END
                └───────────┘
```

Notable design details:

- **Single moderator function, two graph positions.** `moderator_node` is registered as both `moderator_open` and `moderator_distill`. It branches internally on `len(transcript) == 0`. This is the idiomatic LangGraph pattern for reusing a callable across flow positions.
- **Termination via state, not a counter.** `verdict: str | None` in state is the single source of truth for "are we done?" — the judge parses its own output to set it. A `max_rounds` cap is defence-in-depth.
- **Per-node trace labels.** Each LLM call inside a node is tagged with a `run_name` like `proposer-argument-r1`, so you can filter traces by role-and-round in the UI. Tool-using turns add `-iter0`, `-iter1`, … suffixes for each ReAct iteration.
- **ReAct loops inside debater nodes.** `proposer` and `critic` are single nodes in the graph, but internally each runs a tool-call loop (`LLM → tool_calls → ToolMessages → LLM → …`) until the LLM produces prose or `MAX_TOOL_CALLS=4` is hit. The moderator, judge, and setup stay pure-LLM — they arbitrate structure, not evidence.

For the full design and decision history, see [`docs/superpowers/specs/`](superpowers/specs/) and [`docs/superpowers/plans/`](superpowers/plans/).

## Web research in detail

Only **proposer and critic** have tools. Moderator and judge stay pure-LLM — they arbitrate the debate; they don't gather evidence.

Each debater turn runs a hand-rolled ReAct loop inside `_run_with_tools()` in `debate/nodes.py`:

1. Invoke the LLM on `[SystemMessage, HumanMessage, …]` with tools bound via `.bind_tools([search, extract])`.
2. If the response has no `tool_calls`, return its prose — that's the final argument.
3. Otherwise append the `AIMessage` (tool_calls intact), execute each call, append results as `ToolMessage`s, loop.
4. Hard cap at `MAX_TOOL_CALLS = 4`. Overrun → force one more invocation with the tools still bound but `tool_choice={"type": "none"}` ("use only the evidence above") to produce a clean close. The tools must stay bound: Anthropic rejects a history with `tool_use` blocks when no tools are defined.

The two tools, both from [`langchain-tavily`](https://pypi.org/project/langchain-tavily/):

- **`tavily_search`** — 4 results with `title`, `url`, ~500-char snippet. First call of a turn; snippets are often enough for citation.
- **`tavily_extract`** — full page as clean markdown. Only when a snippet is too thin for a proper quote.

Sources appear in the transcript as inline markdown links `[claim](https://url)`. Citation discipline is enforced via prompt: fabricated URLs are explicitly forbidden.

Tool errors don't crash the debate — they're caught at the loop level, surfaced as `ToolMessage(content="Tool error: ...")`, and the prompt instructs the LLM to argue from principle without apologising. Errors stay visible in LangSmith/Langfuse — the rate-limit day isn't hidden, you'll see the 429 span.

See [`debate/tools.py`](../debate/tools.py) for retrieval knobs (`max_results=4`, `search_depth="basic"`, `include_answer=False`) — tune in one place.

## Stack

- **LangGraph** — state machine for agent orchestration. Modern replacement for `AgentExecutor`. Gives inspectable per-node trace spans.
- **langchain-anthropic** — Claude integration. Currently points at `claude-sonnet-4-6`.
- **langchain-tavily** — `TavilySearch` + `TavilyExtract` as LangChain `Tool` instances. Bound to debater models via `.bind_tools()`; auto-trace as their own spans.
- **Tavily** — web search + page-extract backend. LLM-friendly API: pre-trimmed snippets for search, clean markdown for extract.
- **LangSmith** — hosted tracing. Auto-attaches via env vars — zero code. Best in class for LangChain-native observability.
- **Langfuse** — alternative hosted tracing, attached via an explicit callback handler. Open-source, self-hostable.
- **python-dotenv** — loads `.env` before LangChain imports so LangSmith's auto-attach actually sees the env vars.

Both tracers are wired simultaneously. This is *on purpose* — it demonstrates that tracing in LangChain is callback-based and vendor-neutral; you can A/B compare the UIs from the same run.

## Reading the traces

You'll see the same run from two angles.

**LangSmith** — run list shows `debate:<first-40-chars-of-topic>`. Open a run; the trace tree nests: `LangGraph` root → `setup` → `moderator_open` → `proposer` → `critic` → ... Each LLM call is a child span labeled with `role-action-rN` (e.g., `proposer-argument-r1`). Click any span for inputs, outputs, token counts, cost, latency. Filter by tags (`debate`, `experiment:v1`) or metadata (`prompts_version`, `max_rounds`).

**Langfuse** — same shape, different UI. The Observations table gives you a flat per-span view (every node and LLM call). The Trace view gives you the tree.

Things worth noticing in both UIs:

- `judge_router` outputs "continue" or "end" — the routing decision is a captured span of its own.
- `langgraph_checkpoint_ns` in metadata — LangGraph adds it for free; shows exact graph position.
- `ls_temperature` varies by role: 0.7 for proposer/critic (creative), 0.3 for moderator/judge (consistent).
- `tavily_search` / `tavily_extract` — every tool call is its own child span under the debater node, with the query/URL as input and the raw JSON response as output. Latency and cost attached.
- `proposer-argument-r1-iter0`, `-iter1`, … — the per-iteration run names inside a ReAct loop. `iterN` tells you how many rounds the LLM took before producing prose.
- `-forced-close` — only appears when `MAX_TOOL_CALLS` was exhausted. Rare; flag as pathological and tune prompts.
- Tags now include `tools:v1`; metadata has `max_tool_calls`, `search_provider`, and `prompts_version: v2`. Filter on these to A/B v1 (pre-tools) vs v2 (tools) runs on the same topic.

## Project structure

```
.
├── .env.example            # env template
├── .gitignore
├── main.py                 # entry point: env + CLI + graph invocation
├── requirements.txt
├── debate/
│   ├── __init__.py
│   ├── state.py            # DebateState + Turn TypedDicts
│   ├── prompts.py          # 5 system prompts — main iteration surface
│   ├── tools.py            # Tavily search + extract as LangChain Tool instances
│   ├── rate_limit.py       # retry-on-429 guardrail around ChatAnthropic.invoke()
│   ├── nodes.py            # node functions + model factory + ReAct loop helper
│   └── graph.py            # build_graph() — topology
└── docs/
    └── superpowers/
        ├── specs/          # design doc with trade-offs and rationale
        └── plans/          # task-by-task implementation plan
```

Each module has one responsibility:

- `state.py` — types only, no logic.
- `prompts.py` — constants only, no imports beyond docstring. **This is where you iterate most** when tuning voice.
- `tools.py` — Tavily configuration, isolated from orchestration so retrieval knobs (`max_results`, `search_depth`) don't bleed into `nodes.py`.
- `rate_limit.py` — reactive retry-on-429 guardrail wrapping every `ChatAnthropic.invoke()` call. Sleep duration honours the `retry-after` header when present, exponential backoff otherwise. Knobs via `.env` (`RATE_LIMIT_*`). It stacks on the Anthropic SDK's own retries (default 2 per call, not visible as spans), so a long 429 storm costs up to 3 × (`RATE_LIMIT_MAX_RETRIES` + 1) requests.
- `nodes.py` — node functions that read state + config, call the LLM, return state deltas. Also houses `_run_with_tools()` (the ReAct loop) and `MAX_TOOL_CALLS`. The model factory is here too.
- `graph.py` — short but architecturally important. Registers nodes, wires edges, compiles.
- `main.py` — thin entry point: load env, validate keys, build graph, invoke, print.

## What to experiment with

Iterations are cheap. The feedback loop is: edit → run → inspect trace → repeat.

- **Rewrite a prompt** — `debate/prompts.py`. How much voice/quality depends on prompt wording. Bump `prompts_version` in `main.py` metadata so you can compare versions in traces.
- **Soften the "always search" line** — `PROPOSER_SYSTEM` / `CRITIC_SYSTEM`. The current prompt says *"Open your turn with one tavily_search query"* — pushes search even on ethics topics. Soften to *"search if you need evidence"* and compare tool-call counts in traces.
- **Change termination rule** — `judge_node` in `debate/nodes.py`. Early-stop vs always-run-max-rounds: does forcing more rounds produce richer debates? Measure avg-rounds-to-verdict in traces.
- **Try a new topic** — `python main.py "..."`. How the moderator's opening question shapes the debate's entire trajectory.
- **Tweak `max_rounds`** — `main.py` (hardcoded to 3). Does a 5-round debate converge or just repeat?
- **Tweak `MAX_TOOL_CALLS`** — `debate/nodes.py`. 2 forces laser-focused queries; 8 invites loops. Does more research mean better arguments, or just more cost?
- **Toggle `search_depth="advanced"`** — `debate/tools.py`. 2× Tavily credit cost per search. Does it meaningfully improve citation quality or just burn credits?
- **Swap model per role** — `_ROLE_TEMPERATURES` + `_model_for`. Try Haiku for moderator (cheap/fast) + Opus for judge (deep reasoning). Cost vs quality trade-off in traces.
- **Add a new role** — `debate/nodes.py` + `debate/graph.py`. E.g., a "devil's advocate" that introduces a fresh angle mid-round. Tests your understanding of node registration + conditional edges.
- **Extend tools to the judge (Approach 2)** — `debate/nodes.py` + prompts. Judge becomes a fact-checker with receipts. The full migration is mapped in [`docs/superpowers/specs/2026-04-21-tavily-tool-use-design.md`](superpowers/specs/2026-04-21-tavily-tool-use-design.md).
