<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/banner/hero-v2-light.svg">
  <img alt="LAB-DEBATER. Agents argue. You read the trace. Experiment. A LangGraph debate agent: five roles on one Claude model, Tavily web research, two tracers." src="assets/banner/hero-v2-dark.svg" width="100%">
</picture>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/lead-v2-light.svg">
  <img alt="lab-debater runs a structured, Oxford-Union-style debate about any topic you give it. Five roles share one Claude model. Two of them search the live web for evidence. Every call lands in two trace UIs. It is a learning playground. The agents do not know that." src="assets/readme/lead-v2-dark.svg" width="100%">
</picture>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/stats-v2-light.svg">
  <img alt="6 GRAPH NODES. 4 TOOL CALLS PER TURN. 2 TRACERS PER RUN. 1 CLAUDE SONNET 4.6" src="assets/readme/stats-v2-dark.svg" width="100%">
</picture>
</p>

<br>

<p>
<img alt="01 WHAT IT DOES. FIVE ROLES. ONE VERDICT." src="assets/readme/divider-what-v2.svg" width="100%">
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/features-v2-light.svg">
  <img alt="DEBATE: Five roles on one Claude model. The moderator frames, proposer and critic argue, the judge rules. Max 3 rounds by default. RESEARCH: Proposer and critic search the live web with Tavily, up to 4 tool calls per turn. Sources appear as inline links. TRACE: Every call goes to LangSmith and Langfuse at the same time. Same run, two UIs. Tool errors stay visible" src="assets/readme/features-v2-dark.svg" width="100%">
</picture>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/what-v2-light.svg">
  <img alt="One model, five roles: Five roles share one Claude Sonnet 4.6 model. Each role has its own system prompt and temperature. Moderator: Opens with a sharp focus question, then distils each round into agreement, disagreement and a sharper follow-up. Proposer and critic: The proposer argues for, the critic argues against. Both engage the other side directly. Both have research tools. ReAct per debater turn: Up to MAX_TOOL_CALLS = 4 calls to tavily_search / tavily_extract, then prose. Sources appear inline as [claim](https://url) links. Judge: Ends each round: continue, or emit a verdict. An optional max_rounds cap (default 3) is defence in depth. Output: The full transcript, a VERDICT: line, and both trace URLs. An excerpt is in docs/REFERENCE.md" src="assets/readme/what-v2-dark.svg" width="100%">
</picture>
</p>

<br>

<p>
<img alt="02 QUICK START. FIVE KEYS. ONE COMMAND." src="assets/readme/divider-start-v2.svg" width="100%">
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/start-v2-light.svg">
  <img alt="Terminal: $ git clone https://github.com/VelimirMueller/lab-debater.git | $ cd lab-debater &amp;&amp; pip install -r requirements.txt | $ cp .env.example .env   # add 5 API keys | $ python main.py &quot;Should remote work be the default?&quot;" src="assets/readme/start-v2-dark.svg" width="100%">
</picture>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/keys-v2-light.svg">
  <img alt="Anthropic: console.anthropic.com (https://console.anthropic.com). One full debate costs about $0.10-0.20 on Sonnet 4.6 with tools, about $0.05 without. LangSmith: smith.langchain.com (https://smith.langchain.com). Free tier: 5k traces per month. Langfuse: cloud.langfuse.com (https://cloud.langfuse.com) (EU) or us.cloud.langfuse.com (https://us.cloud.langfuse.com) (US). Free tier: 50k observations per month. Tavily: tavily.com (https://tavily.com). Free tier: 1,000 credits per month, one credit per basic search or extract. One debate budgets at most 24 calls, so 40+ debates fit" src="assets/readme/keys-v2-dark.svg" width="100%">
</picture>
</p>

<p align="center">
<a href="https://console.anthropic.com"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-1-v2-light.svg"><img alt="Anthropic (https://console.anthropic.com)" src="assets/readme/links-1-v2-dark.svg" width="32%"></picture></a>
<a href="https://smith.langchain.com"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-2-v2-light.svg"><img alt="LangSmith (https://smith.langchain.com)" src="assets/readme/links-2-v2-dark.svg" width="32%"></picture></a>
<a href="https://cloud.langfuse.com"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-3-v2-light.svg"><img alt="Langfuse EU (https://cloud.langfuse.com)" src="assets/readme/links-3-v2-dark.svg" width="32%"></picture></a>
</p>

<p align="center">
<a href="https://us.cloud.langfuse.com"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-4-v2-light.svg"><img alt="Langfuse US (https://us.cloud.langfuse.com)" src="assets/readme/links-4-v2-dark.svg" width="32%"></picture></a>
<a href="https://tavily.com"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-5-v2-light.svg"><img alt="Tavily (https://tavily.com)" src="assets/readme/links-5-v2-dark.svg" width="32%"></picture></a>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/setup-v2-light.svg">
  <img alt="Terminal: # Python 3.13+ (ideally via uv, pyenv, or a PyCharm venv) | $ python -m venv .venv | $ source .venv/bin/activate   # or .venv\Scripts\activate on Windows | $ pip install -r requirements.txt | $ cp .env.example .env | # edit .env and paste in ANTHROPIC_API_KEY, LANGSMITH_API_KEY, | # LANGFUSE_PUBLIC_KEY, LANGFUSE_SECRET_KEY, TAVILY_API_KEY. | # Leave the other fields as-is. main.py fails fast if a required key is missing. | $ python main.py &quot;Should remote work be the default for knowledge workers?&quot;" src="assets/readme/setup-v2-dark.svg" width="100%">
</picture>
</p>

<details>
<summary><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-14-v2-light.svg"><img alt="Plain text" src="assets/readme/links-14-v2-dark.svg" width="32%"></picture></summary>

```bash
# clone, enter
git clone https://github.com/VelimirMueller/lab-debater.git
cd lab-debater

# Python 3.13+ (ideally via uv, pyenv, or a PyCharm venv)
python -m venv .venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows
pip install -r requirements.txt

# keys
cp .env.example .env
# edit .env and paste in ANTHROPIC_API_KEY, LANGSMITH_API_KEY,
# LANGFUSE_PUBLIC_KEY, LANGFUSE_SECRET_KEY, TAVILY_API_KEY.
# Leave the other fields as-is. main.py will fail fast if any required key is missing.

# run
python main.py "Should remote work be the default for knowledge workers?"
```

</details>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/privacy-v2-light.svg">
  <img alt="Tracing is on by default: LANGSMITH_TRACING=true in .env.example, plus the Langfuse callback in main.py (build_run_config()). Everything goes to both tracers: Every run sends the topic, all prompts, every model reply and the raw Tavily results, including full page text from tavily_extract, to LangSmith and Langfuse. Do not debate anything you would not paste into those dashboards. Run without LangSmith: Set LANGSMITH_TRACING=false. Indirect prompt injection: Scraped pages go back into the model as tool results, so a page can carry instructions aimed at the LLM. Acceptable for a local toy with read-only tools. Keep it in mind before giving the agent tools that act" src="assets/readme/privacy-v2-dark.svg" width="100%">
</picture>
</p>

<br>

<p>
<img alt="03 HOW IT WORKS. ONE GRAPH. ONE LOOP." src="assets/readme/divider-how-v2.svg" width="100%">
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/flow-v2-light.svg">
  <img alt="MODERATOR -&gt; PROPOSER -&gt; CRITIC -&gt; JUDGE. The judge loops the graph until a verdict. The max_rounds cap (3) is defence in depth." src="assets/readme/flow-v2-dark.svg" width="100%">
</picture>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/graph-v2-light.svg">
  <img alt="MODERATOR -&gt; PROPOSER -&gt; CRITIC -&gt; MODERATOR -&gt; JUDGE. Judge says continue: loop back to the proposer. Judge gives a verdict: END." src="assets/readme/graph-v2-dark.svg" width="100%">
</picture>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/how-v2-light.svg">
  <img alt="One StateGraph: One LangGraph StateGraph, six nodes, one conditional edge. The moderator is one function registered at two positions (moderator_open, moderator_distill); it branches on len(transcript) == 0. Termination lives in state: Not in a counter: verdict: str | None is the single source of truth. The judge parses its own output to set it. max_rounds is defence in depth. ReAct only in debater nodes: _run_with_tools() in debate/nodes.py. Moderator, judge and setup stay pure-LLM: they arbitrate structure, not evidence. Run names: Each LLM call carries a run_name like proposer-argument-r1. ReAct iterations add -iter0, -iter1, .... Design history: docs/superpowers/specs/ and docs/superpowers/plans/. Full reference: docs/REFERENCE.md" src="assets/readme/how-v2-dark.svg" width="100%">
</picture>
</p>

<p align="center">
<a href="docs/REFERENCE.md"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-6-v2-light.svg"><img alt="Reference (docs/REFERENCE.md)" src="assets/readme/links-6-v2-dark.svg" width="32%"></picture></a>
<a href="docs/superpowers/specs/"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-7-v2-light.svg"><img alt="Specs (docs/superpowers/specs/)" src="assets/readme/links-7-v2-dark.svg" width="32%"></picture></a>
<a href="docs/superpowers/plans/"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-8-v2-light.svg"><img alt="Plans (docs/superpowers/plans/)" src="assets/readme/links-8-v2-dark.svg" width="32%"></picture></a>
</p>

<br>

<p>
<img alt="04 USAGE. THE KNOBS, IN ONE PLACE." src="assets/readme/divider-usage-v2.svg" width="100%">
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/tools-v2-light.svg">
  <img alt="tavily_search: 4 results with title, url and a ~500-char snippet. Usually the first call of a turn. tavily_extract: The full page as clean markdown. For when a snippet is too thin for a proper quote. One place for the knobs: Both come from langchain-tavily (https://pypi.org/project/langchain-tavily/). Retrieval knobs live in debate/tools.py: max_results=4, search_depth=&quot;basic&quot;, include_answer=False. Clean close at the cap: The loop forces one clean close with the tools still bound and tool_choice={&quot;type&quot;: &quot;none&quot;}. Anthropic rejects a history with tool_use blocks when no tools are defined. Tool errors do not crash the debate: They surface as ToolMessage(content=&quot;Tool error: ...&quot;) and stay visible in both tracers. The rate-limit day is not hidden" src="assets/readme/tools-v2-dark.svg" width="100%">
</picture>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/env-v2-light.svg">
  <img alt="ANTHROPIC_API_KEY TAVILY_API_KEY: The model and the search. LANGSMITH_TRACING LANGSMITH_API_KEY LANGSMITH_PROJECT LANGSMITH_ENDPOINT: LangSmith. EU accounts must set the EU endpoint; a wrong region means 403 on /runs/multipart uploads. LANGFUSE_PUBLIC_KEY LANGFUSE_SECRET_KEY LANGFUSE_HOST: Langfuse. RATE_LIMIT_MAX_RETRIES RATE_LIMIT_MAX_SLEEP_SECONDS RATE_LIMIT_BASE_SLEEP_SECONDS: The 429 guardrail in debate/rate_limit.py. It honours the retry-after header, backs off exponentially otherwise, and stacks on the Anthropic SDK&#x27;s own retries (default 2 per call): a long 429 storm costs up to 3 x (RATE_LIMIT_MAX_RETRIES + 1) requests" src="assets/readme/env-v2-dark.svg" width="100%">
</picture>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/structure-v2-light.svg">
  <img alt="debate/prompts.py: Five system prompts. This is where you iterate most when tuning voice. debate/nodes.py: Node functions, the model factory, the ReAct loop, MAX_TOOL_CALLS. debate/graph.py: Topology. debate/state.py: Types only, no logic. main.py: Thin entry point: env, CLI, graph invocation, printing" src="assets/readme/structure-v2-dark.svg" width="100%">
</picture>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/scope-v2-light.svg">
  <img alt="Iterations are cheap: Edit, run, inspect trace, repeat. Ten documented tweaks (prompts, termination rule, max_rounds, MAX_TOOL_CALLS, search_depth=&quot;advanced&quot;, model per role, a new role, tools for the judge) are listed in docs/REFERENCE.md. Validation is mostly manual, via traces: tests/ holds offline regression tests for the forced-close and judge-cap edge paths and the rate-limit guardrail. Not in scope: No persistence, no UI, no API (CLI only), no streaming, no tool caching, no cross-turn source dedup, no multi-user sessions. Tools for proposer and critic only (Approach 1): Approach 2, the judge as fact-checker with receipts, is mapped in the Tavily design spec, not wired in. Spec: docs/superpowers/specs/2026-04-21-tavily-tool-use-design.md. Fork freely" src="assets/readme/scope-v2-dark.svg" width="100%">
</picture>
</p>

<p align="center">
<a href="docs/superpowers/specs/2026-04-21-tavily-tool-use-design.md"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-9-v2-light.svg"><img alt="Tavily design spec (docs/superpowers/specs/2026-04-21-tavily-tool-use-design.md)" src="assets/readme/links-9-v2-dark.svg" width="32%"></picture></a>
<a href="https://pypi.org/project/langchain-tavily/"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-10-v2-light.svg"><img alt="langchain-tavily (https://pypi.org/project/langchain-tavily/)" src="assets/readme/links-10-v2-dark.svg" width="32%"></picture></a>
<a href="debate/tools.py"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-11-v2-light.svg"><img alt="Retrieval knobs (debate/tools.py)" src="assets/readme/links-11-v2-dark.svg" width="32%"></picture></a>
</p>

<br>

<p>
<img alt="05 STATUS. WHAT WORKS. WHAT DOES NOT." src="assets/readme/divider-status-v2.svg" width="100%">
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/status-v2-light.svg">
  <img alt="debate graph, six nodes: working. tavily search + extract: working, 4 calls per turn. dual tracing: LangSmith + Langfuse. rate-limit guardrail: working. offline tests: forced-close, judge cap, rate limit. streaming, UI, persistence: out of scope" src="assets/readme/status-v2-dark.svg" width="100%">
</picture>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/tests-v2-light.svg">
  <img alt="Terminal: $ pip install -r requirements-dev.txt &amp;&amp; python -m pytest" src="assets/readme/tests-v2-dark.svg" width="100%">
</picture>
</p>

<details>
<summary><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-14-v2-light.svg"><img alt="Plain text" src="assets/readme/links-14-v2-dark.svg" width="32%"></picture></summary>

```bash
pip install -r requirements-dev.txt && python -m pytest
```

</details>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/ci-v2-light.svg">
  <img alt="CI: .github/workflows/tests.yml: the same pytest on every push and PR. LICENSE: MIT. Do whatever you want with it" src="assets/readme/ci-v2-dark.svg" width="100%">
</picture>
</p>

<p align="center">
<a href=".github/workflows/tests.yml"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-12-v2-light.svg"><img alt="CI (.github/workflows/tests.yml)" src="assets/readme/links-12-v2-dark.svg" width="32%"></picture></a>
<a href="LICENSE"><picture><source media="(prefers-color-scheme: light)" srcset="assets/readme/links-13-v2-light.svg"><img alt="License (LICENSE)" src="assets/readme/links-13-v2-dark.svg" width="32%"></picture></a>
</p>

<p>
<picture>
  <source media="(prefers-color-scheme: light)" srcset="assets/readme/footer-v2-light.svg">
  <img alt="VM. studio / lab · open source · look per vm-brand playbook · MIT © 2026 Velimir Mueller · -- EOF -- the judge rests --" src="assets/readme/footer-v2-dark.svg" width="100%">
</picture>
</p>
