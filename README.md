# Awesome Agent Evals [![Awesome](https://awesome.re/badge.svg)](https://awesome.re) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)

> A curated, **auto-updating** list of tools, frameworks, benchmarks, and ideas for **evaluating AI agents** — knowing whether your agent actually works, and whether it still works tomorrow.

Anyone can demo an agent in five minutes. The hard part is knowing it does the right thing on the inputs you didn't try, and catching the moment a prompt tweak, model bump, or new tool quietly makes it worse. Agents fail differently from plain LLM calls — they take multi-step **trajectories**, call **tools**, keep **state**, and a small early mistake compounds into a wrong final action. Evaluating them needs more than "eyeball the output."

**🔎 Prefer to browse and filter? → [agent-evals.agentpostmortem.com](https://agent-evals.agentpostmortem.com)**

---

## Start Here: How to Think About Agent Evals

A quick mental model so the tools below make sense.

**1. Outcome vs. trajectory.**
- *Outcome eval* asks: was the final answer/action correct? (Did it book the right flight?)
- *Trajectory eval* asks: were the intermediate steps right? (Did it call the tools in a sane order, not hallucinate an argument, not loop?)
- Agents need both. A right answer reached by luck through a broken path will break on the next input.

**2. Offline vs. online.**
- *Offline* — run a fixed test set in CI before you ship. Repeatable, gates regressions.
- *Online* — measure real traffic in production (traces, user signals, live guardrails). Catches what your test set didn't imagine.

**3. How you score.**
- *Deterministic checks* — exact match, regex, "did the tool get called", schema/JSON validity, task completed. Cheap and reliable; use them wherever you can.
- *LLM-as-judge* — a model grades the output against a rubric. Flexible for open-ended answers, but the judge itself needs validating (bias, position effects) or you're measuring noise.
- *Human review* — the ground truth you calibrate the others against. Expensive; spend it on a small gold set.

**4. The metrics people actually track.**
Task success rate, tool-call accuracy, faithfulness/groundedness (for RAG), safety/guardrail violations, cost per task, latency (p50/p95/p99), and **regression delta** — did this change make it worse than last week?

**5. The one rule.** A metric you can game in isolation is worthless. Recall means nothing without a leak/precision counterpart; a 0% failure rate means nothing if you never tested a hard case. Pair your metrics, and keep a gold set you trust.

---

<!-- LIST:START -->
**38 tools and benchmarks**, auto-refreshed weekly. Star counts updated **2026-08-31**. Browse the filterable version at **[agent-evals.agentpostmortem.com](https://agent-evals.agentpostmortem.com)**.

### At a glance: eval frameworks compared

| Tool | Stars | Host | Eval mode | LLM-judge | Language | RAG-strong |
| --- | --- | --- | --- | --- | --- | --- |
| [Langfuse](https://github.com/langfuse/langfuse) | 34k | OSS | offline + online | ✅ | Python/TS | ✅ |
| [promptfoo](https://github.com/promptfoo/promptfoo) | 24.7k | OSS | offline + online | ✅ | TS | ✅ |
| [Opik](https://github.com/comet-ml/opik) | 21.7k | OSS | offline + online | ✅ | Python/TS | ✅ |
| [OpenAI Evals](https://github.com/openai/evals) | 19.3k | OSS | offline | ✅ | Python | — |
| [DeepEval](https://github.com/confident-ai/deepeval) | 18k | OSS | offline | ✅ | Python | ✅ |
| [Ragas](https://github.com/explodinggradients/ragas) | 15.6k | OSS | offline | ✅ | Python | ✅ |
| [Phoenix](https://github.com/Arize-ai/phoenix) | 11.3k | OSS | offline + online | ✅ | Python | ✅ |
| [Evidently](https://github.com/evidentlyai/evidently) | 7.9k | OSS | offline + online | ✅ | Python | — |
| [Giskard](https://github.com/Giskard-AI/giskard) | 5.8k | OSS | offline | ✅ | Python | ✅ |
| [Deepchecks](https://github.com/deepchecks/deepchecks) | 4k | OSS | offline | ✅ | Python | — |
| [TruLens](https://github.com/truera/trulens) | 3.5k | OSS | offline + online | ✅ | Python | ✅ |
| [LangWatch](https://github.com/langwatch/langwatch) | 3.5k | OSS | offline + online | ✅ | Python/TS | ✅ |
| [Inspect](https://github.com/UKGovernmentBEIS/inspect_ai) | 2.7k | OSS | offline | ✅ | Python | — |
| [UpTrain](https://github.com/uptrain-ai/uptrain) | 2.4k | OSS | offline + online | ✅ | Python | ✅ |
| [Weave](https://github.com/wandb/weave) | 1.1k | OSS | offline + online | ✅ | Python/TS | — |


### Eval Frameworks & Platforms

- [Langfuse](https://github.com/langfuse/langfuse) `★ 34k` — Open-source LLM engineering platform: evals, observability, prompt management, datasets.
- [promptfoo](https://github.com/promptfoo/promptfoo) `★ 24.7k` — Test prompts, agents, and RAG with declarative configs; red-teaming and CI/CD built in. Used by OpenAI and Anthropic.
- [Opik](https://github.com/comet-ml/opik) `★ 21.7k` — Trace, evaluate, and monitor LLM/RAG/agentic apps with automated evals and dashboards.
- [OpenAI Evals](https://github.com/openai/evals) `★ 19.3k` — The original framework plus an open registry of benchmarks for evaluating LLMs and systems.
- [DeepEval](https://github.com/confident-ai/deepeval) `★ 18k` — "Pytest for LLMs" — a large library of metrics you assert on in unit tests.
- [Ragas](https://github.com/explodinggradients/ragas) `★ 15.6k` — Metrics focused on RAG and agent pipelines; strong on faithfulness and context quality.
- [Phoenix](https://github.com/Arize-ai/phoenix) `★ 11.3k` — AI observability and evaluation, OpenTelemetry-based (by Arize).
- [Evidently](https://github.com/evidentlyai/evidently) `★ 7.9k` — Open-source ML/LLM observability with 100+ metrics for evaluation and monitoring.
- [Giskard](https://github.com/Giskard-AI/giskard) `★ 5.8k` — Open-source testing for LLM agents; scans for vulnerabilities and quality issues.
- [Deepchecks](https://github.com/deepchecks/deepchecks) `★ 4k` — Continuous validation for models and data, extended to LLM apps.
- [TruLens](https://github.com/truera/trulens) `★ 3.5k` — Evaluation and tracking for LLM experiments and agents, built around feedback functions.
- [LangWatch](https://github.com/langwatch/langwatch) `★ 3.5k` — A platform for LLM evaluations and AI agent testing.
- [Inspect](https://github.com/UKGovernmentBEIS/inspect_ai) `★ 2.7k` — A rigorous evaluation framework from the UK AI Safety Institute; first-class agent and tool-use support.
- [UpTrain](https://github.com/uptrain-ai/uptrain) `★ 2.4k` — 20+ preconfigured checks plus root-cause analysis on failures.
- [Weave](https://github.com/wandb/weave) `★ 1.1k` — A lightweight toolkit for tracking and evaluating LLM apps (by Weights & Biases).

### Observability & Tracing

- [OpenLLMetry](https://github.com/traceloop/openllmetry) `★ 7.4k` — Open-source GenAI observability built on OpenTelemetry.
- [Helicone](https://github.com/Helicone/helicone) `★ 6.1k` — Open-source LLM observability; one line of code to monitor, evaluate, and experiment.
- [AgentOps](https://github.com/AgentOps-AI/agentops) `★ 5.8k` — Agent monitoring, cost tracking, and benchmarking; integrates with CrewAI, OpenAI Agents SDK, LangChain, AutoGen, and more.
- [Langtrace](https://github.com/Scale3-Labs/langtrace) `★ 1.2k` — OpenTelemetry-based end-to-end tracing, evals, and metrics for LLM apps and vector DBs.

### Guardrails & Runtime Checks

- [Guardrails AI](https://github.com/guardrails-ai/guardrails) `★ 7.3k` — Add input/output validators and structured guarantees to LLMs.
- [NeMo Guardrails](https://github.com/NVIDIA/NeMo-Guardrails) `★ 7k` — Programmable guardrails for LLM conversational systems (by NVIDIA).
- [LLM Guard](https://github.com/protectai/llm-guard) `★ 3.2k` — A security toolkit for LLM interactions (PII, toxicity, injection).
- [Rebuff](https://github.com/protectai/rebuff) `★ 1.5k` — A self-hardening prompt-injection detector.

### Agent Benchmarks & Task Suites

- [SWE-bench](https://github.com/princeton-nlp/SWE-bench) `★ 5.8k` — Can agents resolve real GitHub issues? The de facto coding-agent benchmark.
- [ToolBench](https://github.com/OpenBMB/ToolBench) `★ 5.7k` — Training, serving, and evaluating LLMs for tool use (ICLR'24 spotlight).
- [AgentBench](https://github.com/THUDM/AgentBench) `★ 3.7k` — A comprehensive benchmark evaluating LLMs as agents across 8 environments (ICLR'24).
- [OSWorld](https://github.com/xlang-ai/OSWorld) `★ 3.1k` — Benchmarking multimodal agents on open-ended tasks in real computer environments (NeurIPS 2024).
- [MLE-bench](https://github.com/openai/mle-bench) `★ 1.7k` — How well do agents perform at machine-learning engineering? (by OpenAI).
- [WebArena](https://github.com/web-arena-x/webarena) `★ 1.6k` — A realistic, self-hostable web environment for autonomous agents.
- [τ-bench (tau-bench)](https://github.com/sierra-research/tau-bench) `★ 1.4k` — Tool-agent-user interaction in realistic customer-service settings (by Sierra).
- [BrowserGym](https://github.com/ServiceNow/BrowserGym) `★ 1.3k` — A Gym environment for web-automation agents (by ServiceNow).
- [AndroidWorld](https://github.com/google-research/android_world) `★ 869` — An environment and benchmark for autonomous mobile agents (by Google Research).
- [VisualWebArena](https://github.com/web-arena-x/visualwebarena) `★ 485` — WebArena extended to multimodal, visually-grounded web tasks.

### Specialized & CI Eval Tools

- [EvalGate](https://github.com/royalpinto007/evalgate) `★ 0` — Prompt and agent regression CI as a GitHub Action; the build fails when your prompt gets dumber, with PR delta comments.
- [VoiceEval](https://github.com/royalpinto007/Voiceeval) `★ 0` — Evaluation for voice agents; catches what text evals miss: mis-hearing, missing confirmation, latency, barge-in.
- [Agentrace](https://github.com/royalpinto007/Agentrace) `★ 0` — Observability for Claude Code subagents; reads session transcripts and flags results you shouldn't trust.
- [AnswerProof](https://github.com/royalpinto007/answerproof) `★ 0` — Verifiable, tamper-evident receipts for RAG answers (Merkle inclusion proofs + Ed25519 signatures).

### Datasets

- [LLM Datasets](https://github.com/mlabonne/llm-datasets) `★ 4.8k` — A curated list of datasets and tools for post-training and evaluation.

<!-- LIST:END -->

---

## Related

- [awesome-llm-guardrails](https://github.com/royalpinto007/awesome-llm-guardrails) — the guardrails counterpart: stop your agent from leaking data, getting jailbroken, or emitting malformed output.

## Contributing

Contributions are very welcome — this list is only as good as the community keeps it. Edit [`data/tools.json`](data/tools.json) and open a PR (the README table and the site are generated from it — don't edit them by hand). Please:

- One object per tool: `name`, `url`, `category`, a one-honest-sentence `desc`, and `repo` (`owner/name`) if it's on GitHub so stars auto-populate.
- Categories: `framework`, `observability`, `guardrails`, `benchmark`, `specialized`, `dataset`.
- No dead links, no pure marketing, no paid placements. Prefer things you've actually used.

New to open source? Adding one good entry here is a perfectly good first PR.

> Star counts refresh automatically every week via GitHub Actions — no manual upkeep.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related rights to this work.
