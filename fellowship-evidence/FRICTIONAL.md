# Humanitarians AI Fellowship Frictional Analysis & Bottleneck Log

> **Document Status: Retrospectively Reconstructed Record**  
> **Notice:** This document provides an honest, empirical accounting of technical failures, design bottlenecks, benchmark discrepancies, and administrative friction encountered during the fellowship period (September 4 – October 12, 2026).

---

## 1. Technical & Engineering Friction

### A. Video Artifact Rendering & Export Timeout (October 2–3, 2026)
* **Trigger:** Attempting to render a 1080p Remotion-style weekly work recap video directly from browser animations into an MP4 file using Playwright and an FFmpeg rawvideo pipe.
* **Underlying Mechanism:** 
  - Capturing continuous canvas/CSS animations in a headless Chromium instance on local Apple Silicon hardware generated high memory overhead.
  - Frame extraction dropped frames unpredictably, Playwright browser sessions stalled, and local write operations to `.arkenos/artifacts/weekly_recap_video.html` experienced timeouts.
  - As a result, no clean offline `.mp4` file was persisted locally into workspace artifacts.
* **Resolution & Strategic Pivot:**
  - Decoupled video production from the local agent orchestration environment.
  - Established a clean separation of concerns on October 9 by preparing structured research handoff briefs (summarizing goals, repository metrics, benchmark tables, and failure traces) so that progress videos could be rendered in dedicated external Antigravity nodes.
  - Available video records are compiled in the external [Research Progress Videos (Google Drive)](https://drive.google.com/drive/folders/10CnVBjdP3e29JFJIHTLEWQyQy-BqzOwr?usp=drive_link) folder for reviewer inspection.

---

### B. Dense Keyword Retrieval Failure on Code/Notebooks (October 5, 2026)
* **Trigger:** Querying complex architectural concepts (e.g., DSPy teleprompters, DPO fine-tuning, llama.cpp GGUF runtimes) in `Hands-On-Large-Language-Models`.
* **Underlying Mechanism:** 
  - Computational notebooks differ fundamentally from standard text documents. Code cells frequently use shorthand variable names, local imports across dozens of cells, or modular wrapper functions rather than verbatim library names.
  - Naive grep and ripgrep searches resulted in either zero hits (lexical mismatch) or overwhelming false positives (hundreds of lines of boilerplate imports), captured in [`dense_failure_trace.json`](../memory-bin/traces/dense_failure_trace.json).
* **Resolution & Architectural Evolution:**
  - Architected the persistent [`memory-bin/`](../memory-bin/) directory on October 7.
  - Implemented multi-tier indexes:
    1. [`regex_alias_index.json`](../memory-bin/indexes/regex_alias_index.json): Maps high-level concepts to exact code-level regex patterns (e.g., `Llama|llama_cpp|AutoModelForCausalLM|GGUF`).
    2. [`heading_structure_index.json`](../memory-bin/indexes/heading_structure_index.json): Provides hierarchical markdown structural anchors.
    3. [`lessons/failure_modes.json`](../memory-bin/lessons/failure_modes.json): Documents failure modes to redirect search strategies before execution.

---

### C. Snippet Window Starvation by Boilerplate Cells (October 7, 2026 — Pilot 1)
* **Trigger:** Executing the initial controlled MEMORY ON vs. OFF A/B evaluation ([`first_index_ab/experiment_traces_summary.json`](../memory-bin/experiments/first_index_ab/experiment_traces_summary.json)).
* **Underlying Mechanism:** 
  - Notebook files typically begin with package installation commands (e.g., `!pip install transformers datasets`).
  - Search hits occurring near the beginning of notebooks caused the fixed inspection window to be completely filled with boilerplate shell commands, starving the snippet window of substantive Python model definitions.
  - Consequently, both MEMORY OFF and MEMORY ON scored **0 / 4 (0%) correct evidence**.
* **Resolution & Code Correction:**
  - Implemented `is_setup_or_install_cell()` to systematically detect and filter out environment setup and package installation cells from evaluation snippet windows.

---

### D. Suboptimal First-Hit Anchor Selection Trap (October 8, 2026 — Initial Benchmark v2)
* **Trigger:** Evaluating Question 2 (`q2_local_quantized_runtime`) in [`benchmark_questions_v2_verified.json`](../memory-bin/experiments/benchmark_questions_v2_verified.json).
* **Underlying Mechanism:** 
  - In [`benchmark_v2_ab_run/experiment_summary.json`](../memory-bin/experiments/benchmark_v2_ab_run/experiment_summary.json), a search query returned multiple hits across a notebook. The agent naively anchored to the *first* non-boilerplate hit, which happened to be a high-level markdown section header (Cell 22) rather than the substantive code execution cell (Cell 37).
  - Expanding around the markdown header completely missed the downstream GGUF model load calls, resulting in **0 / 4 correct evidence** for both conditions.
* **Resolution & Code Correction:**
  - Engineered `score_candidate_anchor` in [`adaptive_search_repl.ipynb`](../adaptive_search_repl.ipynb).
  - The anchor selector scores executable code cells with imports and API calls (+30.0) above markdown section headings (+10.0), allowing the agent to anchor directly to substantive code.
  - In Pilot 3 ([`benchmark_v2_ranked_anchors_ab_run/experiment_summary.json`](../memory-bin/experiments/benchmark_v2_ranked_anchors_ab_run/experiment_summary.json)), this enabled MEMORY ON to correctly locate the GGUF runtime on Question 2.

---

### E. Fixed Cell-Span Truncation Ceiling (October 7–8, 2026 — Pilot 2 & Pilot 3)
* **Trigger:** Evaluating Question 1 (`q1_topic_diversity`) targeting Chapter 5.
* **Underlying Mechanism:** 
  - In `Chapter 5 - Text Clustering and Topic Modeling.ipynb`, the conceptual section heading is located at Cell 36.
  - However, the actual code implementation (`MaximalMarginalRelevance(diversity=0.5)`) is located at Cell 46—a span of 10 cells.
  - A fixed $\pm 3$ cell expansion window anchored at Cell 36 stops at Cell 39, falling 7 cells short of the ground-truth code definition.
  - Both MEMORY OFF and MEMORY ON failed Question 1 due to this structural span limitation.
* **Resolution & Documented Next Step:**
  - Formulated the design for dynamic heading-bounded expansion (reading forward until the next markdown `##` header or a maximum ceiling of 10 cells) as documented in [`RESEARCH_PROGRESS.md`](../RESEARCH_PROGRESS.md).
  - Accurately logged dynamic heading-bounded expansion as a **proposed future improvement** rather than claiming completed implementation.

---

## 2. Benchmark Discrepancy & Verification Analysis

### The Discrepancy
Earlier draft narratives referenced an unverified claim of 4 / 4 (100%) accuracy for MEMORY ON. A rigorous audit of the physical filesystem and experiment traces reveals that this claim was unsupported and conflated.

### Physical Evidence Audit Across Experiment Runs

| Experiment Run | Directory Path | Condition | Result | Root Cause / Evaluation Notes |
| :--- | :--- | :---: | :---: | :--- |
| **Pilot 1** | [`first_index_ab/experiment_traces_summary.json`](../memory-bin/experiments/first_index_ab/experiment_traces_summary.json) | MEMORY OFF<br>MEMORY ON | 0 / 4 (0%)<br>0 / 4 (0%) | Boilerplate setup cells (`!pip install`) starved snippet window on all 4 questions. |
| **Pilot 2** | [`first_index_ab_with_local_expansion/experiment_summary.json`](../memory-bin/experiments/first_index_ab_with_local_expansion/experiment_summary.json) | MEMORY OFF<br>MEMORY ON | 0 / 4 (0%)<br>**1 / 4 (25%)** | Added $\pm 3$ cell expansion. Passed `q2_local_reasoning_runtime`. |
| **Benchmark v2 Initial** | [`benchmark_v2_ab_run/experiment_summary.json`](../memory-bin/experiments/benchmark_v2_ab_run/experiment_summary.json) | MEMORY OFF<br>MEMORY ON | 0 / 4 (0%)<br>0 / 4 (0%) | First-hit anchor trap selected markdown header (Cell 22) instead of code (Cell 37). |
| **Pilot 3 (Ranked Anchors)** | [`benchmark_v2_ranked_anchors_ab_run/experiment_summary.json`](../memory-bin/experiments/benchmark_v2_ranked_anchors_ab_run/experiment_summary.json) | MEMORY OFF<br>MEMORY ON | 0 / 4 (0%)<br>**1 / 4 (25%)** | Ranked anchor selection enabled `q2` pass. `q1` failed due to 10-cell span. `q3` & `q4` correctly rejected by router. |

### Verification Verdict
1. **The True Empirical Score:** The verified empirical score for Pilot 3 with ranked anchor selection is **1 / 4 (25%) correct evidence** for MEMORY ON versus **0 / 4 (0%)** for MEMORY OFF.
2. **Negative Control Observations:** For negative control questions (`q3` and `q4`), zero false-positive index activations were observed; the router declined to activate the regex alias index (confidence below 0.60 threshold). This demonstrates safe rejection on these specific items, but does not establish general statistical router precision across unseen distributions.
3. **No 4/4 Artifact Exists:** No physical trace, summary JSON, or execution log on disk contains a 4/4 result. Conflating the targeted `q2` fix with a 100% benchmark score was an error in retrospective drafting. The official fellowship records have been strictly updated to reflect the true **1 / 4 (25%)** score.

---

## 3. Administrative & Process Friction

### A. Automatic Renewal Misconception & Pending Status (September 30 – October 12, 2026)
* **Friction Point:** The initial volunteer agreement officially expired on September 30, 2026. Technical research continued into October under the fellow's assumption that fellowship appointments renew automatically.
* **Impact:** Administrative renewal was formally submitted via email on October 12, 2026, to `hr@humanitarians.ai` and `bear@humanitarians.ai`, with project supervisor Dev shah copied.
* **Resolution:** Documented the administrative gap in Section 1 of [`renewal-report.md`](../renewal-report.md) (which serves as supporting research documentation) and noted that formal October renewal remains pending approval.

### B. Absence of Standardized Daily Time Tracking
* **Friction Point:** While the contractual agreement defined a high-level commitment of 20–40 hours per week, no automated daily time-tracking software or punch log was utilized during local execution.
* **Impact:** Specific daily hours cannot be algorithmically verified from session logs alone, as session files record active agent turns but do not capture offline reading, writing, or conceptual design.
* **Resolution:** In [`WORK_LOG.md`](WORK_LOG.md), the fellow formally confirmed the actual hours worked across each reporting period: Week 1 (20h), Week 2 (24h), Week 3 (28h), Week 4 (30h), and Week 5 (10h, partial 5-day week through October 12). These hours are designated as "self-reported, retrospectively reconstructed" to preserve reporting integrity.
