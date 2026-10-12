# Humanitarians AI Fellowship Evidence Audit & Artifact Map

> **Document Status: Retrospectively Reconstructed Record**  
> **Notice:** This document provides a retrospectively reconstructed, version-controlled evidence map linking reported fellowship research activities to physical repository files, Git commits, and local execution artifacts. In accordance with privacy and security standards, sensitive personal data (such as personal contact details, residential addresses, government identification numbers, and private conversation transcripts) are excluded from public version control. Where evidence exists solely on local developer hardware, it is cataloged as local evidence and available privately upon request.

---

## 1. Administrative & Institutional Framework

| Evidentiary Item | Location / Reference | Verification Date | Evidentiary Purpose & Status |
| :--- | :--- | :--- | :--- |
| **Base Volunteer Agreement** | Local evidence — available privately upon request | Sep 04, 2026 | Establishes fellowship start date and institutional framework: Humanitarians AI Inc. (Boston, MA). Retained privately. |
| **Executed Volunteer Agreement** | Local evidence — available privately upon request | Sep 09, 2026 | Documents fellow role (AI Engineer), effective duration (Sep 4 – Sep 30, 2026), project (Iacon), supervisor (Dev shah), expected commitment (20–40 hrs/wk), and primary duties. |
| **Work Authorization** | Local evidence — available privately upon request | Sep 04, 2026 | Confirms fellow work authorization period (08/04/26 – 07/05/27). Specific document identifiers excluded. |
| **Fellowship Renewal Request** | Emailed to `hr@humanitarians.ai` and `bear@humanitarians.ai`, cc'ing Dev shah | Oct 12, 2026 | Formal administrative request to extend fellowship through October 31, 2026; approval remains pending. Supporting technical documentation provided in [`renewal-report.md`](../renewal-report.md). |

* **Institutional Entity:** Humanitarians AI Inc. (327 Huntington Ave, Boston, MA 02115; EIN: 33-1984805; E-Verify ID: 2572255)
* **Fellow:** Sai Jaswanthi Banoth (GitHub: [`jaswanthi03`](https://github.com/jaswanthi03))
* **Project Supervisor:** Dev shah

---

## 2. Optimus Conversation Transcripts (Local Session Records)

Historical Optimus agent session transcripts are preserved locally on developer hardware and are available privately upon request. They are excluded from the public repository to prevent exposing private working logs.

| Session ID | Primary Date Span | Interaction Volume | Focus & Evidentiary Anchor | Availability |
| :--- | :--- | :--- | :--- | :--- |
| `chat_v4_1790617879368_1` | 2026-09-28 | 1 user prompt (16 total entries) | Ingested *Chemistry: Atoms First* PDF into local workspace. | Local evidence — available privately upon request |
| `chat_v4_1790617960434_4` | 2026-09-28 to 2026-09-30 | 20 user prompts (185 total entries) | Tested Strategies 1–4; drafted 1-page summary for Dev shah (Sep 29); tested concept graph search (Sep 30). | Local evidence — available privately upon request |
| `chat_v4_1790796081188_39`| 2026-09-30 | 1 user prompt (19 total entries) | Scoped cluster GPU hardware parameters (A6000 vs. H100, NVLink, Slurm parameters). | Local evidence — available privately upon request |
| `chat_v4_1790983198528_1` | 2026-10-02 to 2026-10-03 | 5 user prompts (67 total entries) | Prototyped weekly Remotion recap video; tested browser animation via Playwright; diagnosed export timeouts. | Local evidence — available privately upon request |
| `chat_v4_1791232222583_1` | 2026-10-05 to 2026-10-12 | 29 user prompts (411 total entries) | Ingested `HandsOnLLM` repo; built REPL; native ripgrep integration; Dev technical update; `memory-bin/` build; A/B benchmarking; renewal preparation; pushed to GitHub. | Local evidence — available privately upon request |
| `chat_v4_1791824632723_40`| 2026-10-12 (Live) | Active session | Full timeline reconstruction, discrepancy resolution, and evidence documentation. | Local evidence — available privately upon request |

---

## 3. Physical Code, Tooling, Indexes & Traces

### A. Chemistry Textbook Retrieval (September 28–30, 2026)
* **Target Document:** `Chemistry_Atoms_First_no_contents_or_preface.pdf` (47.2 MB; 1,345 pages; local evidence — available privately upon request; excluded from Git tracking via `.gitignore`).
* **Custom Extraction Tooling:** Local native Swift parser (`pdf_tool.swift` / `pdf_tool`) used to generate structural sections.
* **Extracted Section Indexes:** Local JSON maps (`chapter_sections.json`, `sections_map.json`) cataloging textbook chapters and section boundaries.

### B. Adaptive Agent Search REPL & Code Repository (October 5–12, 2026)
* **Target Repository Under Study:** [`HandsOnLLM/Hands-On-Large-Language-Models`](https://github.com/HandsOnLLM/Hands-On-Large-Language-Models) (O'Reilly, 2024; 83 files, 169 markdown headings, 528 notebook cells).
* **Interactive REPL Notebook:** [`adaptive_search_repl.ipynb`](../adaptive_search_repl.ipynb) (22 cells; interactive indexing, tool dispatch, and evaluation harness).
* **Native Search Tool Integration:** Ripgrep (`rg` v15.2.0 with PCRE2) integrated into [`adaptive_search_repl.ipynb`](../adaptive_search_repl.ipynb) (cells 6, 11).
* **Initial Execution Traces:** Local execution logs (`search_trace_log.json` and `experiment_dense_failure_trace.json`) capturing query timestamps and dense keyword failures on Chapter 8 (persisted canonically in `memory-bin/traces/`).

### C. Persistent Memory Architecture ([`memory-bin/`](../memory-bin/))
* **Structured Indexes ([`memory-bin/indexes/`](../memory-bin/indexes/)):**
  - [`heading_structure_index.json`](../memory-bin/indexes/heading_structure_index.json): 1,024 lines; hierarchical structural outline of all 12 chapters.
  - [`lexical_vector_index.json`](../memory-bin/indexes/lexical_vector_index.json): 59,034 lines; lexical TF-IDF vector-space representation using word and character n-grams with cosine similarity across parsed cells.
  - [`pragmatic_task_index.json`](../memory-bin/indexes/pragmatic_task_index.json): 287 lines; task-oriented semantic dispatch mapping.
  - [`regex_alias_index.json`](../memory-bin/indexes/regex_alias_index.json): 260 lines; concept-to-code regex mappings (e.g., GGUF, DPO, DSPy).
* **Failure Lessons ([`memory-bin/lessons/`](../memory-bin/lessons/)):**
  - [`failure_modes.json`](../memory-bin/lessons/failure_modes.json): Structured taxonomy of retrieval failure modes and corrective actions.
  - [`search_lessons.md`](../memory-bin/lessons/search_lessons.md): Diagnostic summary of cell-block locality and anchor-selection rules.
* **Persistent Execution Traces ([`memory-bin/traces/`](../memory-bin/traces/)):**
  - [`connected_agent_trace.json`](../memory-bin/traces/connected_agent_trace.json)
  - [`cumulative_search_trace.json`](../memory-bin/traces/cumulative_search_trace.json)
  - [`dense_failure_trace.json`](../memory-bin/traces/dense_failure_trace.json)
  - [`smoke_test_index_enabled.json`](../memory-bin/traces/smoke_test_index_enabled.json)
  - [`telemetry_verification_trace.json`](../memory-bin/traces/telemetry_verification_trace.json)

---

## 4. Benchmark Discrepancy & Verification Proof

### Background
During retrospective drafting, an unverified narrative claim of 4 / 4 (100%) accuracy for MEMORY ON appeared. To maintain research integrity, an exhaustive audit of all experiment files was conducted, verifying that the empirical score is **1 / 4 (25%)**.

### Benchmark Data Verification Table

| Evaluation Suite | Physical Summary File | MEMORY OFF Score | MEMORY ON Score | Evaluation Conditions & Physical Evidence |
| :--- | :--- | :---: | :---: | :--- |
| **Pilot 1** | [`first_index_ab/experiment_traces_summary.json`](../memory-bin/experiments/first_index_ab/experiment_traces_summary.json) | 0 / 4 (0%) | 0 / 4 (0%) | Fixed window without cell expansion evaluated on [`benchmark_questions.json`](../memory-bin/experiments/benchmark_questions.json); boilerplate `!pip install` consumed snippet window on all 4 questions. |
| **Pilot 2** | [`first_index_ab_with_local_expansion/experiment_summary.json`](../memory-bin/experiments/first_index_ab_with_local_expansion/experiment_summary.json) | 0 / 4 (0%) | **1 / 4 (25%)** | Added fixed $\pm 3$ cell expansion; `q2_local_reasoning_runtime` passed; `q1`, `q3`, `q4` failed. |
| **Benchmark v2 Initial** | [`benchmark_v2_ab_run/experiment_summary.json`](../memory-bin/experiments/benchmark_v2_ab_run/experiment_summary.json) | 0 / 4 (0%) | 0 / 4 (0%) | Tested on [`benchmark_questions_v2_verified.json`](../memory-bin/experiments/benchmark_questions_v2_verified.json); naive anchor selection chose markdown heading (Cell 22) instead of model code (Cell 37) on `q2`. |
| **Pilot 3 (Ranked Anchors)** | [`benchmark_v2_ranked_anchors_ab_run/experiment_summary.json`](../memory-bin/experiments/benchmark_v2_ranked_anchors_ab_run/experiment_summary.json) | 0 / 4 (0%) | **1 / 4 (25%)** | Implemented `score_candidate_anchor`; `q2_local_quantized_runtime` passed; `q1` failed due to 10-cell span; `q3` & `q4` negative controls correctly rejected by router. |

### Verification Findings
1. **The Verified Score is 1/4 (25%):** The highest verified performance achieved by MEMORY ON across all frozen benchmarks is **1 / 4 (25%) correct evidence** (passing `q2`), compared to **0 / 4 (0%)** for MEMORY OFF.
2. **Negative Control Observations:** For negative control questions (`q3` and `q4`), zero false-positive index activations were observed; the router declined to activate the regex alias index (confidence below 0.60 threshold). This demonstrates safe rejection on these specific items, but does not establish general statistical router precision across unseen distributions.
3. **`q1` Bottleneck Formally Logged:** In [`RESEARCH_PROGRESS.md`](../RESEARCH_PROGRESS.md) and [`README.md`](../README.md), the failure on `q1_topic_diversity` (caused by a 10-cell span between heading and code definition) is explicitly documented as a **proposed future next step** requiring dynamic heading-bounded expansion.
4. **Classification of Dynamic Heading-Bounded Expansion:** Dynamic heading-bounded expansion remains classified as **PROPOSED** in research planning; only fixed $\pm 3$ cell-block expansion with ranked anchor selection was tested in Pilot 3.
5. **No Claim of General Learning:** The benchmark demonstrates that persistent indexing and code-specific anchor ranking resolved a targeted retrieval failure on `q2`. It does not claim autonomous agent learning or general statistical superiority.

---

## 5. Git Version Control, Remote Lineage & Project Videos

* **Remote Repository URL:** [https://github.com/jaswanthi03/adaptive-agent-search](https://github.com/jaswanthi03/adaptive-agent-search)
* **Default Branch:** `main`

### Authenticated Git Commit Log:
1. **Commit [`df9aa2c`](https://github.com/jaswanthi03/adaptive-agent-search/commit/df9aa2c393bc6d8d7fc96e37049b8d9ad7b56952)** (2026-10-12 11:45:33 CDT):
   * *Message:* `Initial commit: Adaptive Agent Search research artifacts, persistent indexes, and benchmark traces`
   * *Changes:* 27 files changed, 64,230 insertions(+).
2. **Commit [`4fab590`](https://github.com/jaswanthi03/adaptive-agent-search/commit/4fab590658f466e6d66b61e1cdcecae81dc398a6)** (2026-10-12 11:55:23 CDT):
   * *Message:* `Add verified research progress report, renewal evidence, and updated README`
   * *Changes:* 3 files changed, 312 insertions(+), 64 deletions(-).
3. **Commit [`9e6db29`](https://github.com/jaswanthi03/adaptive-agent-search/commit/9e6db29)** (2026-10-12 12:30:14 CDT):
   * *Message:* `Add retrospective fellowship work log, frictional analysis, and evidence audit`
   * *Changes:* 4 files changed, 424 insertions(+).
4. **Commit [`e10edff`](https://github.com/jaswanthi03/adaptive-agent-search/commit/e10edff96a69ef4c64ad67b1028335a523ff5e45)** (2026-10-12 12:31:37 CDT):
   * *Message:* `Revise project supervisor name and update hours`
   * *Changes:* 1 file changed, 6 insertions(+), 6 deletions(-).
5. **Commit [`b7e9828`](https://github.com/jaswanthi03/adaptive-agent-search/commit/b7e9828)** (2026-10-12 12:44:48 CDT):
   * *Message:* `Renumber reporting periods to Week 1-5 and integrate confirmed hours`
   * *Changes:* 1 file changed, 28 insertions(+), 43 deletions(-).

### External Video Evidence Folder:
* **Google Drive Video Folder:** [Humanitarians AI Fellowship Research Progress Videos](https://drive.google.com/drive/folders/10CnVBjdP3e29JFJIHTLEWQyQy-BqzOwr?usp=drive_link)
* *Evidentiary Note:* External video repository containing research progress recordings and Brutalist video updates. Provided as a video evidence folder for review; individual video files represent work-in-progress research records rather than independently audited artifacts.
