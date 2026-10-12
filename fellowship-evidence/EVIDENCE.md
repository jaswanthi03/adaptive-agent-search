# Humanitarians AI Fellowship Evidence Audit & Artifact Map

> **Document Status: Retrospectively Reconstructed Record**  
> **Notice:** This document establishes an immutable audit trail linking all reported fellowship claims to physical files, Git commits, and persisted session logs. In accordance with privacy and security standards, sensitive personal data (such as phone numbers, personal home addresses, and government identification numbers) have been strictly excluded or redacted.

---

## 1. Administrative & Institutional Framework

| Evidentiary Anchor | Physical Path / Document | Timestamp (Local / UTC) | Verified Fact / Contractual Term |
| :--- | :--- | :--- | :--- |
| **Base Volunteer Agreement** | `/Users/jaswanthibanoth/Desktop/Humanitarians_AI_Inc_Base_Volunteer_Agreement.pdf` | 2026-09-04 14:00:31 EDT | Establishes fellowship start date and institutional identity: Humanitarians AI Inc. (Boston, MA). |
| **Executed Volunteer Agreement** | `/Users/jaswanthibanoth/Downloads/Sai_Banoth_HaiInc_Agreement_Filled.docx` | 2026-09-09 20:17:25 EDT | Establishes role (AI Engineer), effective duration (Sep 4 – Sep 30, 2026), project (Iacon), supervisor (Dev), expected commitment (20–40 hrs/wk), and primary duties. |
| **Work Authorization** | `/Users/jaswanthibanoth/Downloads/EAD.pdf` | 2026-09-04 12:43:17 EDT | Confirms fellow work authorization period (08/04/26 – 07/05/27). Specific document IDs redacted. |
| **Renewal Report** | `renewal-report.md` | 2026-10-12 11:52:29 CDT | Formally requests administrative renewal for October 1 – October 31, 2026, documenting research continuation and milestones. |

* **Institutional Entity:** Humanitarians AI Inc. (327 Huntington Ave, Boston, MA 02115; EIN: 33-1984805; E-Verify ID: 2572255)
* **Fellow:** Sai Jaswanthi Banoth (GitHub: `jaswanthi03`)
* **Project Supervisor:** Dev

---

## 2. Optimus Conversation Transcripts (Pi JSONL Logs)

Optimus chat transcripts are stored locally at:  
`/Users/jaswanthibanoth/Library/Application Support/arkenOS/agent/sessions/Users-jaswanthibanoth-Desktop-jev/`

| Session ID | File Name | Timestamp Span | Message / Prompt Count | Primary Focus & Milestones |
| :--- | :--- | :--- | :--- | :--- |
| `chat_v4_1790617879368_1` | `2026-09-28T17-52-07-794Z_...` | 2026-09-28 17:52 UTC | 1 user prompt (16 total entries) | Ingested *Chemistry: Atoms First* PDF into workspace `jev`. |
| `chat_v4_1790617960434_4` | `2026-09-28T17-54-32-504Z_...` | 2026-09-28 to 2026-09-30 | 20 user prompts (185 total entries) | Formulated and tested Strategies 1–4; generated 1-page summary for Dev (Sep 29); tested concept graph search (Sep 30). |
| `chat_v4_1790796081188_39`| `2026-09-30T19-21-41-851Z_...` | 2026-09-30 19:21 UTC | 1 user prompt (19 total entries) | Scoped cluster GPU hardware parameters (A6000 vs. H100, NVLink, Slurm limits). |
| `chat_v4_1790983198528_1` | `2026-10-02T23-55-44-138Z_...` | 2026-10-02 to 2026-10-03 | 5 user prompts (67 total entries) | Prototyped weekly Remotion recap video; tested browser animation via Playwright; diagnosed export timeouts. |
| `chat_v4_1791232222583_1` | `2026-10-05T20-31-37-404Z_...` | 2026-10-05 to 2026-10-12 | 29 user prompts (411 total entries) | Ingested `HandsOnLLM` repo; built REPL; native ripgrep integration; Dev technical update; `memory-bin/` build; A/B benchmarking; renewal preparation; pushed to GitHub. |
| `chat_v4_1791824632723_40`| `2026-10-12T17-03-56-335Z_...` | 2026-10-12 (Live) | Active turn | Full timeline reconstruction, discrepancy resolution, and evidence documentation. |

---

## 3. Physical Code, Tooling, Indexes & Traces

### A. Chemistry Textbook Retrieval (September 28–30, 2026)
* **Target Document:** `./Chemistry_Atoms_First_no_contents_or_preface.pdf` (47,249,491 bytes; 1,345 pages)
* **Custom Extraction Binary:** `./.arkenos/bin/pdf_tool` (Compiled native Swift binary `./.arkenos/bin/pdf_tool.swift`)
* **Extracted Section Indexes:** 
  - `./.arkenos/chapter_sections.json` (Structured chapter and section boundaries)
  - `./.arkenos/sections_map.json` (Hierarchical mapping of chemical topics)

### B. Adaptive Agent Search REPL & Code Repository (October 5–12, 2026)
* **Testbed Repository:** `./Hands-On-Large-Language-Models/` (Cloned October 5, 2026; 83 files, 169 markdown headings, 528 cells)
* **Interactive REPL:** `./adaptive_search_repl.ipynb` (53,628 bytes; 22 cells; interactive indexing, tool dispatch, and evaluation harness)
* **Initial Execution Traces:**
  - `./search_trace_log.json` (7 query entries capturing tool dispatches and timestamps)
  - `./experiment_dense_failure_trace.json` (4 queries capturing dense keyword retrieval failures on Chapter 8)
* **Native Search Tool Integration:** Ripgrep (`rg` v15.2.0 with PCRE2 at `/Users/jaswanthibanoth/Library/Application Support/arkenOS/agent/bin/rg`) integrated into `adaptive_search_repl.ipynb` (cells 6, 11).

### C. Persistent Memory Architecture (`memory-bin/`)
* **Structured Indexes (`memory-bin/indexes/`):**
  - `heading_structure_index.json`: 1,024 lines; hierarchical structural outline of all 12 chapters.
  - `lexical_vector_index.json`: 59,034 lines; inverted token index across all code and markdown cells.
  - `pragmatic_task_index.json`: 287 lines; task-oriented semantic dispatch mapping.
  - `regex_alias_index.json`: 260 lines; concept-to-code regex mappings (e.g., GGUF, DPO, DSPy).
* **Failure Lessons (`memory-bin/lessons/`):**
  - `failure_modes.json`: Structured taxonomy of retrieval failure modes and corrective actions.
  - `search_lessons.md`: Diagnostic summary of cell-block locality and anchor-selection rules.
* **Persistent Execution Traces (`memory-bin/traces/`):**
  - `connected_agent_trace.json`, `cumulative_search_trace.json`, `dense_failure_trace.json`, `smoke_test_index_enabled.json`, `telemetry_verification_trace.json`.

---

## 4. Benchmark Discrepancy & Verification Proof

### Background
During initial retrospective drafting, an unverified narrative claim of **4 / 4 (100%) accuracy** for MEMORY ON appeared. To maintain research integrity, an exhaustive audit of all experiment files was conducted.

### Benchmark Data Verification Table

| Evaluation Suite | Physical Summary File | MEMORY OFF Score | MEMORY ON Score | Evaluation Conditions & Physical Evidence |
| :--- | :--- | :--- | :--- | :--- |
| **Pilot 1** | `memory-bin/experiments/first_index_ab/experiment_traces_summary.json` | 0 / 4 (0%) | 0 / 4 (0%) | Fixed window without cell expansion; boilerplate `!pip install` consumed snippet window on all 4 questions. |
| **Pilot 2** | `memory-bin/experiments/first_index_ab_with_local_expansion/experiment_summary.json` | 0 / 4 (0%) | **1 / 4 (25%)** | Added $\pm 3$ cell expansion; `q2_local_reasoning_runtime` passed; `q1`, `q3`, `q4` failed. |
| **Benchmark v2 Initial** | `memory-bin/experiments/benchmark_v2_ab_run/experiment_summary.json` | 0 / 4 (0%) | 0 / 4 (0%) | Tested on `benchmark_questions_v2_verified.json`; naive anchor selection chose markdown heading (Cell 22) instead of model code (Cell 37) on `q2`. |
| **Pilot 3 (Ranked Anchors)** | `memory-bin/experiments/benchmark_v2_ranked_anchors_ab_run/experiment_summary.json` | 0 / 4 (0%) | **1 / 4 (25%)** | Implemented `score_candidate_anchor`; `q2_local_quantized_runtime` passed; `q1` failed due to 10-cell span; `q3` & `q4` correctly rejected by router. |

### Verification Findings
1. **The Claim of 4/4 is Erroneous:** There is no artifact, trace, or JSON file in the repository or git history representing a 4/4 benchmark run.
2. **The Verified Score is 1/4 (25%):** The highest verified performance achieved by MEMORY ON across all frozen benchmarks is **1 / 4 (25%) correct evidence** (passing `q2`), compared to **0 / 4 (0%)** for MEMORY OFF.
3. **Router Precision on Negative Controls:** For negative control questions (`q3` and `q4`), the router correctly produced **0 false positives**, accurately declining to activate the regex alias index.
4. **q1 Bottleneck Formally Logged:** In `RESEARCH_PROGRESS.md` and `README.md`, the failure on `q1_topic_diversity` (caused by a 10-cell span between heading and code definition) is explicitly documented as an *uncompleted future next step* requiring dynamic heading-bounded expansion.
5. **Record Correction:** All retrospective fellowship documentation (`WORK_LOG.md`, `FRICTIONAL.md`, and `EVIDENCE.md`) strictly reports the true verified result of **1 / 4 (25%)**.

---

## 5. Git Version Control & Public Repository Lineage

* **Local Repository Root:** `/Users/jaswanthibanoth/Desktop/jev/.git`
* **Remote Repository URL:** [https://github.com/jaswanthi03/adaptive-agent-search](https://github.com/jaswanthi03/adaptive-agent-search)
* **Default Branch:** `main`

### Authenticated Git Commit Log:
1. **Commit `df9aa2c393bc6d8d7fc96e37049b8d9ad7b56952`**
   * *Timestamp:* 2026-10-12 11:45:33 -0500
   * *Author:* Jaswanthi Banoth <jaswanthi.banoth@gmail.com>
   * *Commit Message:* `Initial commit: Adaptive Agent Search research artifacts, persistent indexes, and benchmark traces`
   * *Changes:* 27 files changed, 64,230 insertions(+).
2. **Commit `4fab590658f466e6d66b61e1cdcecae81dc398a6`**
   * *Timestamp:* 2026-10-12 11:55:23 -0500
   * *Author:* Jaswanthi Banoth <jaswanthi.banoth@gmail.com>
   * *Commit Message:* `Add verified research progress report, renewal evidence, and updated README`
   * *Changes:* 3 files changed, 312 insertions(+), 64 deletions(-).
