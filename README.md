<div align="center">

# muhammadshayan124

**AI Engineer** building LLM agents, evaluated ML systems and the tooling around them.

[![GitHub followers](https://img.shields.io/github/followers/muhammadshayan124?style=flat&logo=github)](https://github.com/muhammadshayan124)
[![Profile views](https://komarev.com/ghpvc/?username=muhammadshayan124&style=flat&color=blue)](https://github.com/muhammadshayan124)

</div>

I build AI systems and then try to break my own claims about them. Most of what is below
carries an evaluation section that says what the numbers do *not* show, and several
projects include a note on a bug that only real data or CI exposed.

## Selected work

### [agentgate](https://github.com/muhammadshayan124/agentgate) — evaluation harness for tool-using agents

Agents are stochastic, so "fail the PR if the score drops" fires on identical builds. agentgate
records trajectories, checks them with deterministic rules (including must-never safety checks) and
gates changes with a statistical test. Measured on 200 replications against a simulated agent: the
naive gate fires on 43% of unchanged builds, agentgate on 3%. A mean-based test caught a single broken
task 8% of the time, which is why it has a per-task rule (100%, no extra false alarms). The README
says plainly that no real LLM agent or judge has been evaluated with it yet.

### [manifest](https://github.com/muhammadshayan124/manifest) — EU AI Act Article 50 scanner

Static analysis that flags chat interfaces with no AI disclosure and generative-content
pipelines with no labeling step (SARIF output, GitHub Action, pre-commit hook). v0.1 passed
its own tests and flagged `asyncio.run` and minified bundles on real code. I labelled every
finding on 13 real codebases by hand and rebuilt the detectors on an AST: strict precision
went from 33% to 88%. The write-up includes the part that does not flatter it: on four apps
I had not looked at, a chat interface was found in only one of them at first contact.

### [typology](https://github.com/muhammadshayan124/typology) — AML transaction-monitoring detector

Graph-aware anomaly detection scored on precision/recall, not accuracy. The first version
computed features over the whole dataset, so each transaction saw its own future. Rebuilt as
point-in-time features with a chronological split; `typology leakage` measures the cost:
PR-AUC 0.988 → 0.876, with an amount-only baseline (0.726) to show what the graph adds.

### [flux](https://github.com/muhammadshayan124/flux) — German grid-load and renewables forecaster

Day-ahead forecasts from SMARD data, with an ablation on identical held-out rows. Naive
persistence barely beats a calendar model for wind; adding one weather-forecast feature set
cuts onshore-wind error from 64.8% to 24.0% NMAE. The README states why that is an upper
bound (archived forecasts are not true day-ahead issues).

### [Sentinel](https://github.com/muhammadshayan124/sentinel) — multimodal RAG agent

[![CI](https://github.com/muhammadshayan124/sentinel/actions/workflows/ci.yml/badge.svg)](https://github.com/muhammadshayan124/sentinel/actions/workflows/ci.yml)

Ingests PDFs, text and images (CLIP embeddings for hybrid retrieval), answers through a
ReAct-style tool loop, and ships with retrieval and answer evals, OpenTelemetry tracing and
CI/CD with Docker packaging.

### Agent systems with the safety logic under test

| Project | What it is | What the tests found |
|---|---|---|
| [scout](https://github.com/muhammadshayan124/scout) | Multi-agent lead-gen pipeline exposed as an MCP server, with a gated email sender, warmup ramp, kill switch and suppression list | A draft for a non-email channel could have been sent by SMTP; reply matching was case-sensitive |
| [voice-proxy](https://github.com/muhammadshayan124/voice-proxy) | Personal voice/chat assistant with a deterministic AI disclosure and a hard escalation boundary | Malformed or blocked model output crashed a live turn instead of escalating |

## Open-source contributions (under review, not merged)

| PR | Change |
|---|---|
| [agno-agi/agno#10175](https://github.com/agno-agi/agno/pull/10175) | `EurostatTools`: live EU statistics for agents |
| [crewAIInc/crewAI#7457](https://github.com/crewAIInc/crewAI/pull/7457) | `EurostatTool` for CrewAI agents |

## Also

| Project | Description |
|---|---|
| [github-crawler](https://github.com/muhammadshayan124/github-crawler) | GitHub GraphQL crawler ingesting star counts for 100k+ repos into PostgreSQL, scheduled via GitHub Actions. |
| [repolens](https://github.com/muhammadshayan124/repolens) | GitHub repo health auditor: docs, CI, tests, licensing and secret-leak checks. |
| [Histopathology-Image-Classification](https://github.com/muhammadshayan124/Histopathology-Image-Classification) | CNN-based histopathology image classifier (TensorFlow/Keras). |
| [Fake-News-Detection-Using-Machine-Learning](https://github.com/muhammadshayan124/Fake-News-Detection-Using-Machine-Learning) | NLP text-classification pipeline using classical ML. |

## Tech stack

![Python](https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/-PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![TensorFlow](https://img.shields.io/badge/-TensorFlow-FF6F00?style=flat-square&logo=tensorflow&logoColor=white)
![FastAPI](https://img.shields.io/badge/-FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Anthropic](https://img.shields.io/badge/-Claude%20API-D4A574?style=flat-square&logo=anthropic&logoColor=white)
![OpenAI](https://img.shields.io/badge/-OpenAI-412991?style=flat-square&logo=openai&logoColor=white)
![Docker](https://img.shields.io/badge/-Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/-GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)

---

<div align="center">
<sub>Open to collaborating on LLM/agent tooling and ML infra projects — open an issue to say hi.</sub>
</div>
