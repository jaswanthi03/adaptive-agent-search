# Humanitarians AI Fellowship Work Log

> **Document Status: Retrospectively Reconstructed Record with Fellow Confirmation**  
> **Notice:** This document is a retrospective work log reconstructed from authenticated filesystem timestamps, Git commit logs, shell histories, and arkenOS agent session transcripts. The fellowship agreement establishes an expected volunteer commitment of **20–40 hours per week**. On October 12, 2026, the fellow formally confirmed the actual hours worked during each of the five active reporting periods (September 11 through October 12, 2026). The initial administrative onboarding window (September 4–10, 2026) is excluded from reporting as no research tasks were assigned during that period. In accordance with institutional reporting standards, confirmed hours are designated as **"self-reported, retrospectively reconstructed"** and are strictly distinguished from independently verified clock-in timesheets, which were not maintained.

---

## 1. Fellow & Fellowship Overview

* **Fellow Name:** Sai Jaswanthi Banoth
* **Institutional Affiliate:** Humanitarians AI Inc. (Boston, MA; EIN: 33-1984805; E-Verify: 2572255)
* **Project Name:** Iacon Project / Adaptive Agent Search & Repository Memory
* **Fellowship Role:** AI Engineer
* **Project Supervisor:** Dev shah
* **Academic Program:** Master of Science in Business Analytics
* **Agreement Period:** September 4, 2026 – September 30, 2026 (Agreement signed September 9, 2026)
* **Renewal Period:** October 1, 2026 – October 31, 2026 (Renewal request submitted October 12, 2026)
* **Public Repository:** [https://github.com/jaswanthi03/adaptive-agent-search](https://github.com/jaswanthi03/adaptive-agent-search)

---

## 2. Weekly Hours & Commitment Summary (Sep 11 – Oct 12, 2026)

> *Note on Initial Period (Sep 4 – Sep 10, 2026):* The initial volunteer agreement was entered into on September 9, 2026 (effective retroactive to September 4). Because no research tasks were assigned by supervisors during that initial administrative onboarding window, reporting begins with Week 1 on September 11, 2026.

| Reporting Week | Date Range | Contractual Status | Expected Commitment | Actual Hours Worked | Evidentiary Anchor & Primary Milestones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Week 1** | Sep 11 – Sep 17, 2026 | September Agreement | 20–40 hours/week | 20 hours — self-reported, retrospectively reconstructed | Gemma open-weights architectures, RLVR literature review, verifiable reward reasoning |
| **Week 2** | Sep 18 – Sep 24, 2026 | September Agreement | 20–40 hours/week | 24 hours — self-reported, retrospectively reconstructed | arkenOS runtime installation (DMGs v0.3.1 & v0.3.2), local CLI environment setup |
| **Week 3** | Sep 25 – Sep 30, 2026 | September Agreement | 20–40 hours/week | 28 hours — self-reported, retrospectively reconstructed | Chemistry textbook ingestion (1,345 pages), Strategies 1–4, Dev 1-page report, GPU cluster scoping |
| **Week 4** | Oct 01 – Oct 07, 2026 | October Renewal | 20–40 hours/week | 30 hours — self-reported, retrospectively reconstructed | Remotion video prototype, Hands-On LLM repo ingestion, REPL build, ripgrep integration, `memory-bin/` build |
| **Week 5** | Oct 08 – Oct 12, 2026 | October Renewal | 20–40 hours/week | 10 hours — self-reported, retrospectively reconstructed | Benchmark v2 suite, anchor-selection fix, Pilot 3 verified A/B run (1/4 ON vs 0/4 OFF), Brutalist handoff, GitHub push |

---

## 3. Detailed Weekly Activity Logs

### Week 1: September 11 – September 17, 2026
* **Contractual Status:** Within September Agreement
* **Expected Commitment:** 20–40 hours/week
* **Actual Hours Worked:** 20 hours — self-reported, retrospectively reconstructed
* **Primary Objective:** Open-Source LLM Architectures, Gemma Models & RLVR Literature Research

#### Activities Performed & Fellow Contributions:
1. **Foundation Model Research:** Researched open-weights foundation models (Gemma family) and methods for adapting compact open-weight models to agentic tool-use tasks.
2. **RLVR Evaluation:** Investigated Reinforcement Learning with Verifiable Rewards (RLVR) literature, focusing on deterministic verifiable feedback loops (e.g., exact code execution, test passes, ground-truth information retrieval) to guide agent search policies without human reward models.

#### Technical Classification:
* Exploratory Literature Review & Formulation.

#### Supporting Evidentiary Artifacts:
* Volunteer Agreement Section 1: Explicit contractual assignment for *"creating and training RL environments using Optimus Agent"* and *"conducting benchmarking and evaluations"*.
* Fellow research notes on open-weights model adaptation and verifiable reward environments.

---

### Week 2: September 18 – September 24, 2026
* **Contractual Status:** Within September Agreement
* **Expected Commitment:** 20–40 hours/week
* **Actual Hours Worked:** 24 hours — self-reported, retrospectively reconstructed
* **Primary Objective:** arkenOS Tooling Setup and Agent Runtime Infrastructure

#### Activities Performed & Fellow Contributions:
1. **Runtime Infrastructure Acquisition:** Downloaded, installed, and configured the arkenOS agent desktop environment on local Apple Silicon hardware.
2. **Local Environment Configuration:** Set up local Python, Node, and native shell execution runtimes to enable Optimus agent orchestration and tool execution.

#### Technical Classification:
* Implemented Development Infrastructure.

#### Supporting Evidentiary Artifacts:
* arkenOS v0.3.1 binary DMG: `/Users/jaswanthibanoth/Downloads/arkenOS-0.3.1-mac-arm64.dmg` (Downloaded: 2026-09-21 10:54:34 EDT)
* arkenOS v0.3.2 binary DMG: `/Users/jaswanthibanoth/Downloads/arkenOS-0.3.2-mac-arm64 (1).dmg` (Downloaded: 2026-09-22 10:54:45 EDT)

---

### Week 3: September 25 – September 30, 2026
* **Contractual Status:** Within September Agreement (Agreement expired September 30, 2026)
* **Expected Commitment:** 20–40 hours/week
* **Actual Hours Worked:** 28 hours — self-reported, retrospectively reconstructed
* **Primary Objective:** Chemistry Textbook Ingestion, Local-to-Global Search Experiments & GPU Scoping

#### Activities Performed & Fellow Contributions:
1. **Workspace Initialization:** Established local arkenOS research workspace (`/Users/jaswanthibanoth/Desktop/jev`).
2. **Textbook Ingestion & Tooling:** Acquired OpenStax *Chemistry: Atoms First* (1,345 pages, 47.2 MB) without front matter to test dynamic mapping and retrieval over large unstructured corpora. Built native extraction tools (`pdf_tool.swift`, `pdf_tool`) and generated structural section indexes (`chapter_sections.json`, `sections_map.json`).
3. **Comparative Strategy Evaluation:** Formulated and tested four distinct retrieval strategies across chemical physics and thermodynamics queries:
   * *Strategy 1 (Map-First Ranking):* Candidate section ranking before search.
   * *Strategy 2 (Local Search First + Pivot Rule):* Targeted search with a 2-failure pivot threshold.
   * *Strategy 3 (Evidence Completeness Verification):* Pausing answer generation to verify macroscopic vs. microscopic completeness.
   * *Strategy 4 (Vocabulary / Synonym Expansion):* Translating colloquial user questions into formal scientific domain phrasing.
4. **Executive Reporting for Dev Shah:** Authored a condensed 1-page technical summary on September 29 tailored specifically for project supervisor Dev Shah.
5. **Semantic Concept Graph Search:** Evaluated whether concept graphs could actively guide retrieval for complex chemical equilibrium and reaction spontaneity queries on September 30.
6. **Hardware Infrastructure Scoping:** Investigated GPU cluster specifications on September 30 (NVIDIA A6000 8×48GB vs. H100 4×80GB, NVLink interconnects, Slurm job scheduling) for distributed training and model hosting.

#### Technical Classification:
* Implemented Tooling & Tested Empirical Strategies.

#### Supporting Evidentiary Artifacts:
* Workspace config: `.arkenos/README.mdx`, `.arkenos/.arkenos.local.json` (Created: 2026-09-28 10:39 EDT)
* Textbook file: `Chemistry_Atoms_First_no_contents_or_preface.pdf` (Added: 2026-09-28 10:49 EDT)
* Custom extraction binaries: `.arkenos/bin/pdf_tool.swift`, `.arkenos/bin/pdf_tool`
* Structural indexes: `.arkenos/chapter_sections.json`, `.arkenos/sections_map.json`
* Session Log 1: `2026-09-28T17-52-07-794Z_chat_v4_1790617879368_1.jsonl` (Workspace file ingestion)
* Session Log 2: `2026-09-28T17-54-32-504Z_chat_v4_1790617960434_4.jsonl` (20 prompts; Strategies 1–4; Dev 1-page report; concept graph)
* Session Log 3: `2026-09-30T19-21-41-851Z_chat_v4_1790796081188_39.jsonl` (GPU cluster hardware inquiry)

---

### Week 4: October 1 – October 7, 2026
* **Contractual Status:** Post-Expiration / Covered under October Renewal Period
* **Expected Commitment:** 20–40 hours/week
* **Actual Hours Worked:** 30 hours — self-reported, retrospectively reconstructed
* **Primary Objective:** Video Automation Prototype, Codebase REPL, Native ripgrep Integration & `memory-bin/`

#### Activities Performed & Fellow Contributions:
1. **Video Production Prototyping:** Prototyped a weekly research recap video using browser Remotion styling and Playwright automation on October 2–3 (`.playwright-cli/`), identifying rendering bottlenecks on macOS.
2. **Repository Ingestion:** Pivoted from static PDF search to structured software repository search. Cloned `HandsOnLLM/Hands-On-Large-Language-Models` (83 files, 169 headings, 528 notebook cells).
3. **Adaptive Search REPL:** Constructed `adaptive_search_repl.ipynb` as an interactive execution REPL with JSON-based trace logging (`search_trace_log.json`).
4. **Native Search Tool Integration:** Evaluated `grep`, `ag`, `fzf`, and native `ripgrep` (`rg` v15.2.0). Integrated native ripgrep into agent tool dispatch for sub-millisecond regex matching across notebook code and markdown cells.
5. **Supervisor Technical Update:** Synthesized a sharp one-page technical update for Dev Shah on October 5 (<2,000 characters).
6. **Dense Failure Analysis:** Isolated dense keyword failure on Chapter 8 (DSPy, DPO, llama.cpp GGUF), generating `experiment_dense_failure_trace.json`.
7. **Persistent Memory-Bin Architecture:** Engineered `memory-bin/` on October 7, cleanly separating raw traces (`traces/`) from generalized lessons (`lessons/failure_modes.json`, `search_lessons.md`) and four structured indexes:
   - `heading_structure_index.json`
   - `lexical_vector_index.json`
   - `pragmatic_task_index.json`
   - `regex_alias_index.json`
8. **Pilot A/B Experimentation:** Executed a 4-question MEMORY ON vs. OFF pilot experiment, discovering a 10-cell expansion truncation bottleneck. Implemented dynamic heading-bounded local expansion.

#### Technical Classification:
* Implemented Architecture & Tested Controlled A/B Pilots.

#### Supporting Evidentiary Artifacts:
* Video automation logs: `.playwright-cli/`, `2026-10-02T23-55-44-138Z_chat_v4_1790983198528_1.jsonl`
* Cloned repository: `Hands-On-Large-Language-Models/` (Cloned: 2026-10-05 15:48 EDT)
* REPL Notebook: `adaptive_search_repl.ipynb` (Created: 2026-10-05; updated 2026-10-07)
* Execution traces: `search_trace_log.json`, `experiment_dense_failure_trace.json`
* Persistent memory directory: `memory-bin/` (indexes, lessons, traces)
* Pilot experiment results: `memory-bin/experiments/first_index_ab/`, `...with_local_expansion/`
* Session Log 5: `2026-10-05T20-31-37-404Z_chat_v4_1791232222583_1.jsonl` (Oct 5–7 exchanges)

---

### Week 5: October 8 – October 12, 2026
* **Contractual Status:** Post-Expiration / Covered under October Renewal Period
* **Expected Commitment:** 20–40 hours/week
* **Actual Hours Worked:** 10 hours — self-reported, retrospectively reconstructed
* **Primary Objective:** Verified Benchmark Suite, Anchor Ranking Fix, Pilot 3 Evaluation & Renewal Finalization

#### Activities Performed & Fellow Contributions:
1. **Verified Benchmark Suite Creation:** Authored and froze `benchmark_questions_v2_verified.json` on October 8 with verified cell spans across multiple repository chapters.
2. **Anchor-Selection Diagnosis & Fix:** Diagnosed failure on Question 2 (`q2_local_quantized_runtime`) caused by anchoring to generic boilerplate cells (`!pip install`). Implemented conceptual density-ranked anchor selection (`score_candidate_anchor`) in `adaptive_search_repl.ipynb`.
3. **Pilot 3 A/B Benchmark Evaluation:** Ran the verified A/B benchmark (`benchmark_v2_ranked_anchors_ab_run`):
   * **MEMORY ON:** Scored **1 / 4 (25%) correct evidence**, successfully retrieving the full GGUF quantized runtime implementation on `q2_local_quantized_runtime` via ranked anchor selection.
   * **MEMORY OFF:** Scored **0 / 4 (0%) correct evidence**, failing to locate the required code models.
   * **Router Precision:** Correctly produced 0 false-positive routings on negative control questions (`q3` and `q4`).
   * **Identified Expansion Ceiling:** Observed that `q1_topic_diversity` failed because target code (`diversity=0.5` at Cell 46) was 10 cells separated from the section heading (Cell 36), exceeding the fixed $\pm 3$ window and establishing the technical requirement for heading-bounded dynamic expansion.
4. **Brutalist Video Research Handoff:** Structured a verified research handoff brief on October 9 for Brutalist video rendering in an external Antigravity environment.
5. **Renewal Documentation & Publication:** Authored `RESEARCH_PROGRESS.md` and `renewal-report.md` on October 12. Committed and pushed repository updates to GitHub (`jaswanthi03/adaptive-agent-search`). Initiated fellowship timeline reconstruction.

#### Technical Classification:
* Implemented, Verified & Published Research.

#### Supporting Evidentiary Artifacts:
* Frozen benchmark suite: `memory-bin/experiments/benchmark_questions_v2_verified.json`
* Pilot 3 experiment summary: `memory-bin/experiments/benchmark_v2_ranked_anchors_ab_run/experiment_summary.json`
* Brutalist handoff brief: In Session Log 5 (`2026-10-09T16:13:15Z`)
* Progress & renewal reports: `RESEARCH_PROGRESS.md`, `renewal-report.md`, `README.md`
* Git Commits:
  - Commit `df9aa2c` (2026-10-12 11:45:33 CDT): Initial research artifacts & benchmark traces
  - Commit `4fab590` (2026-10-12 11:55:23 CDT): Verified progress report & renewal evidence
  - Commit `9e6db29` (2026-10-12 12:30:14 CDT): Add retrospective fellowship work log, frictional analysis, and evidence audit
  - Commit `e10edff` (2026-10-12 12:31:37 CDT): Revise project supervisor name and update hours
* Remote Repository: [https://github.com/jaswanthi03/adaptive-agent-search](https://github.com/jaswanthi03/adaptive-agent-search)
* Live Session Log: `2026-10-12T17-03-56-335Z_chat_v4_1791824632723_40.jsonl`

---

## 4. Verification & Confirmation Protocol

* **Fellow Confirmation Date:** October 12, 2026
* **Confirmation Status:** Confirmed by Sai Jaswanthi Banoth
* **Project Supervisor:** Dev shah
* **Accounting Methodology:**
  1. The fellow confirmed working the specific recorded hours across all five active reporting periods (September 11 – October 12, 2026): Week 1 (20h), Week 2 (24h), Week 3 (28h), Week 4 (30h), and Week 5 (10h, partial week through October 12).
  2. The initial administrative period (September 4–10, 2026) was removed as no research tasks were assigned during that onboarding window.
  3. Because automated timecards/timesheets were not maintained, hours are explicitly recorded as "self-reported, retrospectively reconstructed" to preserve reporting integrity.
  4. Supporting activities remain strictly anchored to verifiable artifacts, Git commit hashes, and persisted session logs.
