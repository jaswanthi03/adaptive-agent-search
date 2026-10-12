# Humanitarians AI Fellowship Renewal Report

**Fellow Name:** Jaswanthi Banoth  
**Project:** Adaptive Agent Search & Repository Memory (Optimus Fleet)  
**Degree Program:** Master of Science in Business Analytics  
**Project Supervisor:** Dev shah  
**Requested Renewal Period:** October 1 – October 31, 2026  
**Repository:** [https://github.com/jaswanthi03/adaptive-agent-search](https://github.com/jaswanthi03/adaptive-agent-search)  
**Video Evidence Folder:** [Research Progress Videos (Google Drive Folder)](https://drive.google.com/drive/folders/10CnVBjdP3e29JFJIHTLEWQyQy-BqzOwr?usp=drive_link)  

---

## 1. Renewal Request Context & Explanation

I am submitting this renewal request for the period of **October 1 to October 31, 2026**.

**Submission Context & Status:**  
The fellowship renewal request was submitted via email on October 12, 2026, to `hr@humanitarians.ai` and `bear@humanitarians.ai`, with project supervisor Dev Shah (`Dev shah`) copied. This request is being submitted after October 1 because I mistakenly assumed that the fellowship agreement would renew automatically at the beginning of the month. I recognize that administrative renewal requires explicit submission, review, and approval. This document serves as supporting research documentation detailing October research continuation; approval remains pending.

*Administrative Verification Records:*
* **Current Agreement Expiration Date:** September 30, 2026 (Initial volunteer agreement covered September 4 – September 30, 2026).
* **Work Authorization Dates:** August 4, 2026 – July 5, 2027 (08/04/2026 – 07/05/2027; local evidence available privately upon request).
* **Weekly Hours Logged:** 20–30 hours/week across full weeks; 10 hours for partial 5-day week ending October 12, 2026 (self-reported, retrospectively reconstructed).
* **Time-Tracking & Frictional Logs:** Cataloged in [`fellowship-evidence/WORK_LOG.md`](fellowship-evidence/WORK_LOG.md) and [`fellowship-evidence/FRICTIONAL.md`](fellowship-evidence/FRICTIONAL.md).
* **Weekly Video Updates:** [Research Progress Videos (Google Drive Folder)](https://drive.google.com/drive/folders/10CnVBjdP3e29JFJIHTLEWQyQy-BqzOwr?usp=drive_link).

---

## 2. Academic & Career Relevance (MS in Business Analytics)

This research directly operationalizes key competency areas of my Master of Science in Business Analytics curriculum:
1. **Information Retrieval & Predictive Query Routing:** Translating business and natural-language user queries into optimized, probabilistic retrieval strategies (e.g. evaluating confidence scores, precision-recall trade-offs, and setting $\ge 0.60$ acceptance thresholds to eliminate false-positive routing).
2. **Data Pipeline Architecture & Index Engineering:** Parsing complex semi-structured data (Jupyter notebook JSON) into clean structured indexes (hierarchical Markdown trees, regex alias tables, and subword vector space matrices).
3. **Controlled Empirical A/B Testing:** Designing paired ablation studies holding all underlying variables constant to isolate whether persistent memory produces statistically valid performance gains.
4. **Data Integrity & Ground-Truth Auditing:** Systematically auditing benchmark datasets against underlying production source code, diagnosing labeling errors (e.g., catching parameter value discrepancies in gold test sets).

---

## 3. Timeline: October Work vs. Prior Work

To ensure complete academic and administrative transparency, fellowship work is chronologically partitioned as follows:

* **Work Predating October 1, 2026 (September 4 – September 30, 2026):**
  * Finalized initial volunteer agreement onboarding (effective September 4, signed September 9).
  * Explored open-weights foundation models (Gemma family) and literature on Reinforcement Learning with Verifiable Rewards (RLVR).
  * Installed and configured arkenOS runtime environment on local Apple Silicon hardware (DMGs v0.3.1 and v0.3.2).
  * Acquired *Chemistry: Atoms First* (1,345 pages) and tested local-to-global PDF retrieval strategies (Strategies 1–4), concept graphs, and scoped GPU cluster requirements.
  * Cloned and onboarded the testbed repository ([`HandsOnLLM/Hands-On-Large-Language-Models`](https://github.com/HandsOnLLM/Hands-On-Large-Language-Models)).
  * Prototyped basic interactive Jupyter REPL concepts and preliminary grep heuristics.
* **Work Completed During Requested Renewal Period (October 1–12, 2026):**
  * Formalized the three-tier `memory-bin/` architecture ([`memory-bin/README.md`](memory-bin/README.md)).
  * Materialized the four persistent search indexes ([`heading_structure_index.json`](memory-bin/indexes/heading_structure_index.json), [`regex_alias_index.json`](memory-bin/indexes/regex_alias_index.json), [`pragmatic_task_index.json`](memory-bin/indexes/pragmatic_task_index.json), [`lexical_vector_index.json`](memory-bin/indexes/lexical_vector_index.json)).
  * Implemented `MemoryBinIndexManager` with false-positive protection (confidence $\ge 0.60$).
  * Built the decoupled `BenchmarkEvaluator` with strict post-search token verification.
  * Conducted the ground-truth repository code audit and authored [`benchmark_questions_v2_verified.json`](memory-bin/experiments/benchmark_questions_v2_verified.json).
  * Built local cell-block expansion with pip-install boilerplate filtering and code-over-heading anchor ranking (`score_candidate_anchor`).
  * Executed paired A/B benchmarks across three iterative pilots (Pilot 1, Pilot 2, Pilot 3).
  * Reconstructed complete retrospective evidence records in [`fellowship-evidence/`](fellowship-evidence/).

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
* **False-Positive Routing on Negative Controls:** **0** (zero false-positive index activations were observed on the two negative-control questions `q3` and `q4`).

*Objective Finding:* Persistent structural indexing eliminates conversational query mismatches on indexed topics, while conservative thresholding prevents false-positive degradation on negative controls. It does not establish general autonomous learning.

---

## 6. Next Deliverable & Milestone Schedule

* **Next Research Deliverable:** Proposed Adaptive Heading-to-Heading Cell-Block Expansion & Scaled 15-Question Multi-Chapter Benchmark
* **Target Delivery Date:** **October 31, 2026**
* **Specific Tasks for Remainder of October:**
  1. **Dynamic Heading-Bounded Expansion (Proposed):** Replace fixed $\pm 3$ cell expansion with dynamic heading-bounded expansion (expanding forward until the next `##` heading or max 10 cells) to resolve long-range cell spans like `q1`.
  2. **Benchmark Scaling:** Author and freeze a 15-question verified benchmark suite across all 12 chapters of the repository.
  3. **Statistical Evaluation:** Execute full statistical paired A/B evaluations and produce a final fellowship research paper draft.
