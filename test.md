# PROPOSAL TITLE

AI Agent Flow — Understand Customer Service Calls Data (VOX)

**Proposal submitted to Cere Foundation on** 2025-10-03 **by** Khachatur Gharibyan

**Project Slug** `vox-ai-agent-flow`

If your application is using an existing RFP provided by Cere, please add the link here: *N/A*

If you are proposing a new RFP that leverages the Cere stack, create a new file in the RFPs folder with a brief description of your proposal, and then add the link to it here: `request_for_proposals/vox-ai-agent-flow.md`

---

## Abstract

Build a production‑ready **multi‑agent pipeline** that ingests raw customer‑service audio at scale and converts it into structured insights end‑to‑end: **time‑aligned transcripts**, **semantic context trees**, **time‑series sentiment**, and **trigger flags** connected to searchable records and dashboards.

The system runs from **audio intake → queued processing → agent orchestration → persistence → APIs/UI**, reusing the Cere stack: **A6 E2E test harness**, **A8 NLP modules**, and **ROB primitives (RAFTs, Cubbies, Object Store)**. Acceptance is defined by unit/e2e tests, quantitative quality targets (WER, F1/AUROC), and operational SLAs. Public docs that inform the approach: [A6 • NLP Processor Lab](https://cere.notion.site/NLP-Processor-Lab-27ed800083d680aa9553e6fa83c6024b), [A8 • NLP Use‑Case Resources](https://cere.notion.site/NLP-Use-Case-resources-27fd800083d680f7a383c7d029e9f4c0), and [ROB Dev Onboarding](https://cere.notion.site/ROB-Platform-Developer-Onboarding-Guide-eaa8764057c547f7a950e003469bb4be).

---

## Team 🧑‍🤝‍🧑

> **Important:** The data below is for administrative and informational purposes only.

**Team members**

* **Khachatur Gharibyan** — author & implementer
  GitHub: [https://github.com/gharibyan](https://github.com/gharibyan)
  LinkedIn: [https://www.linkedin.com/in/xgharibyan/](https://www.linkedin.com/in/xgharibyan/)
  Email: [khachatur.gharibyan@gmail.com](mailto:khachatur.gharibyan@gmail.com)
  Repo (work‑in‑progress): [https://github.com/gharibyan/project-vox](https://github.com/gharibyan/project-vox)

**Name of team leader**
Khachatur Gharibyan

**Names of team members**
Khachatur Gharibyan (solo founder/contractor for initial scope)

**Contact**

* **Contact Name:** Khachatur Gharibyan
* **Contact Email:** [khachatur.gharibyan@gmail.com](mailto:khachatur.gharibyan@gmail.com)
* **Website:** —

**Team's experience**

* Background across data engineering and applied NLP. Example contributions in repo above; additional portfolio on request. Familiar with queue‑based backends, Redis/PG, and LLM/ASR integration.

**If anyone on your team has applied for a grant at the Web3 Foundation previously**
N/A

**Team Code Repos**

* [https://github.com/gharibyan/project-vox](https://github.com/gharibyan/project-vox) (VOX monorepo)

**Member GitHubs**

* [https://github.com/gharibyan](https://github.com/gharibyan)

**Team LinkedIn Profiles (if available)**

* [https://www.linkedin.com/in/xgharibyan/](https://www.linkedin.com/in/xgharibyan/)

---

## Development Roadmap 🔩

This roadmap is milestone‑driven with **clear deliverables and tests**. It aligns to Cere’s stack and Fred’s feedback to “not cut corners,” i.e., each milestone must ship with acceptance tests, dashboards, and reviewer demos.

**Overview**

* **Total Estimated Duration:** 8–10 weeks
* **Full‑Time Equivalent (FTE):** 1.5 (primary dev + part‑time support/review)

### Milestone 1 — Infrastructure & Hello‑World E2E

**Estimated duration:** 2 weeks · **FTE:** 1.5

| Number | Deliverable         | Specification                                                                                                                                                 |
| ------ | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | Monorepo + Docker   | Builds locally & CI; one‑command bootstrap spins up API, Redis, Postgres, and worker RAFTs.                                                                   |
| 2      | A6 E2E harness      | Add A6‑style test runner and fixtures; CI runs smoke test on PR. Docs: [A6 Lab](https://cere.notion.site/NLP-Processor-Lab-27ed800083d680aa9553e6fa83c6024b). |
| 3      | Object store + RBAC | Pluggable object storage (local/S3) with basic authN/Z.                                                                                                       |
| 4      | “Hello world” flow  | Upload short WAV → queued → transcribe stub → persisted → GET returns result. Demonstrated via Postman and short Loom.                                        |

### Milestone 2 — Orchestrator, RAFTs & Cubbies

**Estimated duration:** 3 weeks · **FTE:** 1.5

| # | Deliverable       | Specification                                                                                                                                                                                                                                                                              |
| - | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1 | AudioBatcher RAFT | Accepts file(s), writes to object store, enqueues processing item.                                                                                                                                                                                                                         |
| 2 | Orchestrator      | Deterministic state machine: `transcribe → context → sentiment → complete` with idempotency keys, max retry 3, exponential backoff. Shared state stored in **Cubbies** per [ROB guide](https://cere.notion.site/ROB-Platform-Developer-Onboarding-Guide-eaa8764057c547f7a950e003469bb4be). |
| 3 | Persistence (PG)  | Tables: engagements, transcripts, contexts, sentiments; JSONB payloads; migrations included.                                                                                                                                                                                               |
| 4 | Observability     | Metrics, logs, traces; operator dashboard page with run status, error surfaces, retry action.                                                                                                                                                                                              |

### Milestone 3 — Agents & Quality Targets (A8)

**Estimated duration:** 2–3 weeks · **FTE:** 1.5

| # | Deliverable          | Specification                                                                                                                     |
| - | -------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| 1 | **Transcribe Agent** | Input: audio file url; Output: `Array<{ text: string; timestamp: [number, number] }>`; WER target ≤ **x%** on eval set.           |
| 2 | **Context Agent**    | Builds conversation flow/topic graph; extracts entities/issues; F1 ≥ **y** on synthetic/seed set.                                 |
| 3 | **Sentiment Agent**  | Time‑series sentiment per segment; AUROC ≥ **z** on labeled set.                                                                  |
| 4 | **A8 Integration**   | Model registry/config guided by [A8 resources](https://cere.notion.site/NLP-Use-Case-resources-27fd800083d680f7a383c7d029e9f4c0). |

### Milestone 4 — APIs, UI & Evals

**Estimated duration:** 1.5–2 weeks · **FTE:** 1.5

| # | Deliverable            | Specification                                                                                                                      |
| - | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| 1 | REST API               | Upload (multipart or presigned), List engagements, Single engagement detail; OpenAPI spec + Postman collection.                    |
| 2 | Frontend               | TS + Vite + React + Tailwind. Upload with progress; detail view shows transcript, context tree, sentiment timeline, trigger flags. |
| 3 | Evals & Synthetic Data | Scripted call scenarios → TTS (e.g., 11Labs/async.ai) → optional noise injection → golden labels; CI gate on quality targets.      |
| 4 | Demo & Handover        | Recorded demo, runbooks, README, infra‑as‑code, and reviewer checklist completed.                                                  |

> All milestones include: unit tests, e2e test cases (A6 style), and ops dashboards. PRs reference A6/A8/ROB docs. “Don’t cut corners” means no milestone is accepted without tests and observable metrics.

---

## Future Plans

* **Sustainability:** convert to managed service/extension marketplace; continue eval data curation; add near‑real‑time streaming and multilingual models.
* **Adoption:** publish sample datasets, tutorials, and a hosted demo; explore partner deployments within the Cere ecosystem.
* **Roadmap (post‑grant):** multi‑tenant auth, cost dashboards, improved diarization, real‑time agent loop for live triggers, and CSV/BI exports.

---

## Preferred method of funds delivery

* **USDC on Eth address:** `0x0000000000000000000000000000000000000000` (placeholder)
  *or*
* **USD/EUR Fiat**

**Link to Logo image 1:1 (in GitHub)**
`https://github.com/gharibyan/project-vox` (add `/assets/logo.png` when available)

**Other Bio and details or thoughts, etc.**

* Primary focus is a robust pipeline with measurable quality. Evaluation plan emphasizes synthetic audio generation with controlled noise to stress ASR and sentiment stages; results tracked in the dashboard and compared against gold labels.

---

### Reviewer Checklist (internal)

* [ ] A6/A8/ROB links present in README and milestone docs.
* [ ] E2E tests reproducible locally and in CI; fixtures checked in.
* [ ] Redis (Cubbies) & Postgres schemas documented/migrated.
* [ ] API OpenAPI + Postman collection included.
* [ ] Frontend demo covers upload → results; operator console shows retries/errors.
