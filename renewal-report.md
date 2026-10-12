# Humanitarians AI Fellowship Renewal Report

**Fellow Name:** Jaswanthi Banoth  
**Project:** Adaptive Agent Search & Repository Memory (Optimus Fleet)  
**Degree Program:** Master of Science in Business Analytics  
**Project Supervisor:** Dev  
**Requested Renewal Period:** October 1 – October 31, 2026  
**Repository:** [https://github.com/jaswanthi03/adaptive-agent-search](https://github.com/jaswanthi03/adaptive-agent-search)  

---

## 1. Renewal Request Context & Explanation

I am submitting this renewal request for the period of **October 1 to October 31, 2026**.

**Explanation for Late Submission:**  
This renewal report is being submitted after October 1 because I mistakenly assumed that the fellowship agreement would renew automatically at the beginning of the month. I recognize that administrative renewal requires an explicit submission and evaluation. This document accurately reflects my ongoing technical contributions, without implying that HR or administration has pre-approved a backdated extension.

*Administrative Placeholders for Verification:*
* **Current Agreement Expiration Date:** `[PLACEHOLDER: Current Agreement Expiration Date, e.g., September 30, 2026]`
* **OPT Work Authorization Dates:** `[PLACEHOLDER: Current OPT Authorization Start Date to End Date, e.g., MM/DD/YYYY - MM/DD/YYYY]`
* **Weekly Hours Logged:** `[PLACEHOLDER: Weekly Hours Logged, e.g., XX hours/week]`
* **Time-Tracking / Frictional Logs:** `[PLACEHOLDER: Link to Dated Frictional / Time-tracking Sheet or Log]`
* **Weekly Video Update:** `[PLACEHOLDER: Link to 75-90s Video Progress Recording / Antigravity URL]`

---

## 2. Academic & Career Relevance (MS in Business Analytics)

This research directly operationalizes key competency areas of my Master of Science in Business Analytics curriculum:
1. **Information Retrieval & Predictive Query Routing:** Translating business and natural-language user queries into optimized, probabilistic retrieval strategies (e.g. evaluating confidence scores, precision-recall trade-offs, and setting $\ge 0.60$ acceptance thresholds to eliminate false-positive routing).
2. **Data Pipeline Architecture & Index Engineering:** Parsing complex semi-structured data (Jupyter notebook JSON) into clean structured indexes (hierarchical Markdown trees, regex alias tables, and subword vector space matrices).
3. **Controlled Empirical A/B Testing:** Designing paired ablation studies holding all underlying variables constant to isolate whether persistent memory produces statistically valid performance gains.
4. **Data Integrity & Ground-Truth Auditing:** Systematically auditing benchmark datasets against underlying production source code, diagnosing labeling errors (e.g., catching parameter value discrepancies in gold test sets).

---

## 3. Timeline: October Work vs. Prior Work

To ensure complete academic and administrative honesty, the work is chronologically partitioned as follows:

* **Work Predating October 1, 2026:**
  * Initial exploratory onboarding of the testbed repository (`HandsOnLLM/Hands-On-Large-Language-Models`).
  * Prototyping basic interactive Jupyter REPL concepts and preliminary grep heuristics.
* **Work Completed During Requested Renewal Period (October 1–12, 2026):**
  * Formalized the three-tier `memory-bin/` architecture ([`memory-bin/README.md`](memory-bin/README.md)).
  * Materialized the four persistent search indexes ([`heading_structure_index.json`](memory-bin/indexes/heading_structure_index.json), [`regex_alias_index.json`](memory-bin/indexes/regex_alias_index.json), [`pragmatic_task_index.json`](memory-bin/indexes/pragmatic_task_index.json), [`lexical_vector_index.json`](memory-bin/indexes/lexical_vector_index.json)).
  * Implemented `MemoryBinIndexManager` with false-positive protection (confidence $\ge 0.60$).
  * Built the decoupled `BenchmarkEvaluator` with strict post-search token verification.
  * Conducted the ground-truth repository code audit and authored [`benchmark_questions_v2_verified.json`](memory-bin/experiments/benchmark_questions_v2_verified.json).
  * Built generic local cell-block expansion with pip-install boilerplate filtering and code-over-heading anchor ranking.
  * Executed the paired A/B benchmarks across three iterative pilots (Pilot 1, Pilot 2, Pilot 3).

---

## 4. Key Technical Results & Direct Evidence Links

### Result 1: Elimination of Turn-1 Query Mismatches via Persistent Indexing
* **The Problem:** Natural language questions (*"What library runtime executes the local quantized GGUF model?"*) fail completely when submitted to raw string greps because codebases use technical symbols.
* **The Solution:** Materialized [`regex_alias_index.json`](memory-bin/indexes/regex_alias_index.json), linking conceptual intents to pre-compiled regex patterns.
* **Evidence:** In Pilot 3, MEMORY ON resolved `q2_local_quantized_runtime` on Turn 1 via `ripgrep_adapted_regex`, whereas MEMORY OFF failed on Turn 1 with 0 heading hits.
* **Evidence Link:** [`benchmark_v2_ranked_anchors_ab_run/experiment_summary.json`](memory-bin/experiments/benchmark_v2_ranked_anchors_ab_run/experiment_summary.json)

### Result 2: Code-Specificity Anchor Ranking for Notebook Cell Expansion
* **The Problem:** In multi-cell search hits, expanding around the first match naively anchored on high-level section headings (Cell 22), truncating code models defined further down (Cell 37).
* **The Solution:** Implemented `score_candidate_anchor`, scoring executable code cells with imports/API calls (score 30.0) above markdown section headings (score 10.0) and setup boilerplate (score -1.0).
* **Evidence:** In `q2`, this logic correctly selected Cell 37 (`from llama_cpp.llama import Llama`), expanding cells 34–40 and capturing the GGUF model load call (`Phi-3-mini-4k-instruct-q4.gguf`), converting the task to verified evidence (`Correct: True`).
* **Evidence Link:** [`adaptive_search_repl.ipynb`](adaptive_search_repl.ipynb) & [`benchmark_v2_ranked_anchors_ab_run/full_traces_paired.json`](memory-bin/experiments/benchmark_v2_ranked_anchors_ab_run/full_traces_paired.json)

### Result 3: Rigorous Ground-Truth Gold Evidence Audit
* **The Problem:** Automated benchmark evaluators can produce misleading metrics if gold answers are flawed or conjoin disjoint cells.
* **The Solution:** Independently audited all benchmark questions against AST/source code in Chapters 3, 5, 6, and 9. Caught a benchmark note typo claiming `diversity=0.8` (repository defines `diversity=0.5`), eliminated structural cell conflation on Q2, and replaced hallucinated token `past_key_values` with `use_cache=True` on Q4.
* **Evidence Link:** [`benchmark_questions_v2_verified.json`](memory-bin/experiments/benchmark_questions_v2_verified.json) (audit notes embedded per question)

---

## 5. Summary of Current Experimental Observations (Pilot 3)

The verified 4-question paired A/B benchmark yielded the following exact numbers:
* **Evidence Correctness:** MEMORY ON: **1 / 4 (25%)** | MEMORY OFF: **0 / 4 (0%)**
* **Failed Searches:** MEMORY ON: **1** | MEMORY OFF: **2** (on this 4-question run)
* **Global Searches:** MEMORY ON: **5** | MEMORY OFF: **6**
* **False-Positive Routing:** **0** (router correctly rejected negative controls `q3` and `q4`).

*Objective Finding:* Persistent structural indexing eliminates conversational query mismatches on indexed topics, while conservative thresholding prevents false-positive degradation on negative controls. It does not establish general autonomous learning.

---

## 6. Next Deliverable & Milestone Schedule

* **Next Research Deliverable:** Adaptive Heading-to-Heading Cell-Block Expansion & Scaled 15-Question Multi-Chapter Benchmark
* **Target Delivery Date:** **October 31, 2026**
* **Specific Tasks for Remainder of October:**
  1. Replace fixed $\pm 3$ cell expansion with dynamic heading-bounded expansion (expanding forward until the next `##` heading or max 10 cells) to resolve long-range cell spans like `q1`.
  2. Author and freeze a 15-question verified benchmark suite across all 12 chapters of the repository.
  3. Execute full statistical paired A/B evaluations and produce a final fellowship research paper draft.
