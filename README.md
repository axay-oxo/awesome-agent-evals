# Awesome Agent Evals [![Awesome](https://awesome.re/badge.svg)](https://awesome.re) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)

> A curated list of tools, frameworks, benchmarks, and ideas for **evaluating AI agents** — knowing whether your agent actually works, and whether it still works tomorrow.

Anyone can demo an agent in five minutes. The hard part is knowing it does the right thing on the inputs you didn't try, and catching the moment a prompt tweak, model bump, or new tool quietly makes it worse. Agents fail differently from plain LLM calls — they take multi-step **trajectories**, call **tools**, keep **state**, and a small early mistake compounds into a wrong final action. Evaluating them needs more than "eyeball the output."

This list collects what the field actually uses.

---

## Contents

- [Start Here: How to Think About Agent Evals](#start-here-how-to-think-about-agent-evals)
- [Eval Frameworks & Platforms](#eval-frameworks--platforms)
- [Observability & Tracing](#observability--tracing)
- [Guardrails & Runtime Checks](#guardrails--runtime-checks)
- [Agent Benchmarks & Task Suites](#agent-benchmarks--task-suites)
- [Specialized & CI Eval Tools](#specialized--ci-eval-tools)
- [Datasets](#datasets)
- [Contributing](#contributing)

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

## Eval Frameworks & Platforms

General-purpose toolkits for defining test cases, scoring outputs, and comparing versions.

- [promptfoo](https://github.com/promptfoo/promptfoo) — Test prompts, agents, and RAG with declarative configs; red-teaming and CI/CD built in. Used by OpenAI and Anthropic.
- [DeepEval](https://github.com/confident-ai/deepeval) — "Pytest for LLMs." A large library of metrics (faithfulness, relevancy, task completion) you assert on in unit tests.
- [Ragas](https://github.com/explodinggradients/ragas) — Metrics focused on RAG and agent pipelines; strong on faithfulness and context quality.
- [OpenAI Evals](https://github.com/openai/evals) — The original framework + an open registry of benchmarks for evaluating LLMs and systems.
- [Inspect](https://github.com/UKGovernmentBEIS/inspect_ai) — A rigorous evaluation framework from the UK AI Safety Institute; first-class support for agents and tool use.
- [Giskard](https://github.com/Giskard-AI/giskard) — Open-source testing for LLM agents; scans for vulnerabilities and quality issues.
- [Opik](https://github.com/comet-ml/opik) — Trace, evaluate, and monitor LLM/RAG/agentic apps with automated evals and dashboards (by Comet).
- [Langfuse](https://github.com/langfuse/langfuse) — Open-source LLM engineering platform: evals, observability, prompt management, datasets.
- [Phoenix](https://github.com/Arize-ai/phoenix) — AI observability and evaluation, OpenTelemetry-based (by Arize).
- [TruLens](https://github.com/truera/trulens) — Evaluation and tracking for LLM experiments and agents, built around "feedback functions."
- [LangWatch](https://github.com/langwatch/langwatch) — A platform for LLM evaluations and AI agent testing.
- [UpTrain](https://github.com/uptrain-ai/uptrain) — 20+ preconfigured checks plus root-cause analysis on failures.
- [Deepchecks](https://github.com/deepchecks/deepchecks) — Continuous validation for models and data, extended to LLM apps.
- [Evidently](https://github.com/evidentlyai/evidently) — Open-source ML/LLM observability with 100+ metrics for evaluation and monitoring.
- [Weave](https://github.com/wandb/weave) — A lightweight toolkit for tracking and evaluating LLM apps (by Weights & Biases).

*Commercial platforms in this space include LangSmith (LangChain) and Braintrust — worth knowing even though they're not open source.*

## Observability & Tracing

You can't evaluate what you can't see. These capture the full agent trajectory (prompts, tool calls, latency, cost) in production.

- [Helicone](https://github.com/Helicone/helicone) — Open-source LLM observability; one line of code to monitor, evaluate, and experiment.
- [OpenLLMetry](https://github.com/traceloop/openllmetry) — Open-source GenAI observability built on OpenTelemetry.
- [Langtrace](https://github.com/Scale3-Labs/langtrace) — OpenTelemetry-based end-to-end tracing, evals, and metrics for LLM apps and vector DBs.
- [AgentOps](https://github.com/AgentOps-AI/agentops) — Agent monitoring, cost tracking, and benchmarking; integrates with CrewAI, OpenAI Agents SDK, LangChain, AutoGen, and more.

## Guardrails & Runtime Checks

Online evaluation: catch bad inputs/outputs *while the agent runs*.

- [NeMo Guardrails](https://github.com/NVIDIA/NeMo-Guardrails) — Programmable guardrails for LLM conversational systems (by NVIDIA).
- [Guardrails AI](https://github.com/guardrails-ai/guardrails) — Add input/output validators and structured guarantees to LLMs.
- [LLM Guard](https://github.com/protectai/llm-guard) — A security toolkit for LLM interactions (PII, toxicity, injection).
- [Rebuff](https://github.com/protectai/rebuff) — A self-hardening prompt-injection detector.

## Agent Benchmarks & Task Suites

Standardized environments to measure agent capability on realistic tasks.

- [SWE-bench](https://github.com/princeton-nlp/SWE-bench) — Can agents resolve real GitHub issues? The de facto coding-agent benchmark.
- [τ-bench (tau-bench)](https://github.com/sierra-research/tau-bench) — Tool-agent-user interaction in realistic customer-service settings (by Sierra).
- [AgentBench](https://github.com/THUDM/AgentBench) — A comprehensive benchmark evaluating LLMs as agents across 8 environments (ICLR'24).
- [WebArena](https://github.com/web-arena-x/webarena) — A realistic, self-hostable web environment for autonomous agents.
- [VisualWebArena](https://github.com/web-arena-x/visualwebarena) — WebArena extended to multimodal, visually-grounded web tasks.
- [BrowserGym](https://github.com/ServiceNow/BrowserGym) — A Gym environment for web-automation agents (by ServiceNow).
- [OSWorld](https://github.com/xlang-ai/OSWorld) — Benchmarking multimodal agents on open-ended tasks in real computer environments (NeurIPS 2024).
- [AndroidWorld](https://github.com/google-research/android_world) — An environment and benchmark for autonomous mobile agents (by Google Research).
- [MLE-bench](https://github.com/openai/mle-bench) — How well do agents perform at machine-learning engineering? (by OpenAI).
- [ToolBench](https://github.com/OpenBMB/ToolBench) — Training, serving, and evaluating LLMs for tool use (ICLR'24 spotlight).

## Specialized & CI Eval Tools

Narrower tools for specific agent surfaces — regression gates, voice, subagents, provenance.

- [EvalGate](https://github.com/royalpinto007/evalgate) — Prompt and agent regression CI as a GitHub Action; the build fails when your prompt gets dumber, with PR delta comments.
- [VoiceEval](https://github.com/royalpinto007/Voiceeval) — Evaluation for voice agents; catches what text evals miss: mis-hearing, missing confirmation, latency, barge-in.
- [Agentrace](https://github.com/royalpinto007/Agentrace) — Observability for Claude Code subagents; reads session transcripts and flags results you shouldn't trust.
- [AnswerProof](https://github.com/royalpinto007/answerproof) — Verifiable, tamper-evident receipts for RAG answers (Merkle inclusion proofs + Ed25519 signatures).

## Datasets

- [LLM Datasets](https://github.com/mlabonne/llm-datasets) — A curated list of datasets and tools for post-training and evaluation.

---

## Contributing

Contributions are very welcome — this list is only as good as the community keeps it. Open a PR to add a tool, benchmark, or resource. Please:

- Keep one entry per line: `[Name](link) — one honest sentence on what it does and who it's for.`
- Put it in the section it best fits; add a new section if none fits.
- No dead links, no pure marketing, no paid placements. Prefer things you've actually used.

New to open source? Adding one good entry here is a perfectly good first PR.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related rights to this work.
