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
pipelines with no labeling step (SARIF output, GitHub Action, pre-commit hook), plus a `manifest act`
toolkit: risk-tier classifier (prohibited / high-risk / transparency / GPAI), checklist and technical-documentation generator. v0.1 passed
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

### [fxcast](https://github.com/muhammadshayan124/fxcast) — can you forecast euro exchange rates? An evaluation harness

Walk-forward backtests on ECB data with training labels purged at the forecast origin, Diebold-Mariano tests with the
overlap correction, and Holm adjustment across all 72 comparisons. Result: for the rate itself, **no model beats the random walk**
(0 of 72 significantly better; the 10 significant results are the boosted trees being *worse*). Volatility is different: GARCH, EWMA and HAR beat
constant variance in 45 of 54 corrected comparisons, and the one exception (CHF) is a single event, the 2015 removal of the Swiss floor. A leakage test caught
my own purge guard checking the wrong thing.

### [recsys](https://github.com/muhammadshayan124/recsys) — recommender benchmark with a FastAPI service

MovieLens-1M under a global time split and full-ranking metrics (no sampled negatives), tuned on validation and scored once on test. ALS, EASE and item-kNN are
statistically indistinguishable (NDCG@10 0.196-0.198 against 0.174 for popularity); the two-tower model is **significantly worse** (-0.013, adjusted p = 0.003) but
covers 14-16% of the catalogue against 2-7%. Building the Docker image caught a runtime dependency I had listed as dev-only.

### [eurag](https://github.com/muhammadshayan124/eurag) — retrieval benchmark over six EU digital-law acts

67 hand-written questions whose gold articles are mechanically verified against the legal text. Dense retrieval beats BM25 by about 0.2 NDCG@10 on paraphrased
questions; equal-weight hybrid fusion did not help; many misses are cross-act confusion (an oracle act filter lifts hit@5 from 0.85 to 0.93). pgvector and Qdrant are
indistinguishable at the real corpus size and diverge only on a synthetic 100k scale-up. Retrieval only: no generation or faithfulness evaluation.

### [volops](https://github.com/muhammadshayan124/volops) — MLOps around a model that works (FX volatility)

MLflow tracking, a promotion gate, a monitored FastAPI service, Prometheus alerts, a Grafana dashboard, Kubernetes manifests (schema-validated) and CI/CD, verified end to end
under Docker Compose. My first design retrained on input drift; replaying a model trained 14 months earlier showed 79% of its inputs "drifting" while it still beat the benchmark
on all 12 series, so drift is now advisory and only measured out-of-sample accuracy triggers a retrain. A unit test also caught a label leak in my own holdout fit. Not deployed to a cloud cluster.

### [evalgate](https://github.com/muhammadshayan124/evalgate) — regression gate for LLM applications

Fails a PR unless the candidate is shown non-inferior within a stated margin (paired bootstrap), because "not significantly worse" is not evidence of no regression: in the
bundled example 12 regressions against 4 improvements gives McNemar p = 0.077, which a significance gate would pass, and the gate blocks it. The gate's error rates are checked by
simulation, and it reports how large a regression the eval set could have detected at all.

### [euagents](https://github.com/muhammadshayan124/euagents) — multi-agent research assistant over EU digital law (LangGraph + MCP)

A planner / researcher / drafter / verifier graph over two MCP tool servers, with a human approval gate before the only side-effecting tool, retries with fault injection and JSONL
tracing, run on a 1.7B local model on CPU. Every failure on a 30-task benchmark is assigned one cause. Findings: the graph succeeds on 57% against 3% for the same model as a free agent
(it made no tool call in any of 30 runs) and 20% with a directive prompt; 11 of the graph's 13 failures are search misses; and the verifier never fired, which the README states instead of claiming credit.

### [docextract](https://github.com/muhammadshayan124/docextract) — scanned report to verified JSON, with routing to human review

Synthetic scans at four quality levels, OCR, rule and small-LLM extraction, and constraint-based validation (bid <= ask, change = bid - previous close, ...) that decides which fields a person must check.
The first run exposed my parser and an OCR angle classifier that flipped short numbers upside down; the parser was then frozen in a commit before a held-out corpus was scored. Held-out: 100% field
accuracy at severities 0-2, 61.8% on bad fax-quality pages; the 1.7B model alone is far worse than rules; the hybrid keeps accuracy but cuts the review needed for a 1% error target from 25% to 14% of fields.
Synthetic documents only, and no real handwriting.

### [fxnews-qlora](https://github.com/muhammadshayan124/fxnews-qlora) — does QLoRA fine-tuning beat the simple baselines?

A 1.5B model fine-tuned with 4-bit QLoRA on a T4 for financial-news sentiment, compared on one held-out test set against a majority class, keyword rules, TF-IDF + logistic regression and the same
model prompted zero- and few-shot (bootstrap intervals, paired McNemar tests, 73 train/test duplicate headlines removed). Fine-tuned: **0.918 accuracy / 0.895 macro-F1 against TF-IDF's 0.822 / 0.751**
(right where TF-IDF is wrong on 297 headlines, the reverse on 70), at about 200x the per-example inference cost and a GPU. Zero-shot scores 0.381, below always-Neutral, because the untuned model says
"Bullish" 78% of the time; the README says that is a readout bias, not a verdict on LLMs. One run, one seed.

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
