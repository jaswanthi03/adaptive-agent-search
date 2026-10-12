# Research Progress & Technical Report: Adaptive Agent Search

**Fellow:** Jaswanthi Banoth  
**Project Lead / Agent:** Optimus (arkenOS runtime, `gemini-3.8-flash` Frontier driver)  
**Project Supervisor:** Dev  
**Target Repository:** `HandsOnLLM/Hands-On-Large-Language-Models` (O'Reilly, 2024; 83 files, 169 true Markdown headings, 528 cells/docs)  
**Date:** October 2026  

---

## 1. Research Objective & Problem Addressed

Large language model (LLM) agents operating over technical repositories typically struggle with information retrieval because of two structural disconnects:
1. **The Vocabulary Mismatch:** Users formulate queries using conversational, goal-oriented natural language (*"How do I adapt an embedding model to a new domain without labels?"*), but codebases organize concepts using formal Markdown headings (*"Unsupervised Learning"*) and specific library symbols (*`TSDAE`*, *`DenoisingAutoEncoderLoss`*). Naive string greps fail on Turn 1 with zero hits.
2. **Notebook JSON Noise and Spatial Dispersion:** Jupyter notebooks (`.ipynb`) are stored as raw JSON. Naive regex searches flood context windows with base64 image strings and escape characters. Furthermore, code pipelines in notebooks are chronologically distributed across multiple cells: package installation occurs in Cell 1, imports in Cell 2, prompt templates in Cell 23, and model execution in Cell 37.

This research investigates whether an autonomous agent can maintain an explicit, persistent memory of search failures to iteratively synthesize reusable search indexes, adapt search queries across tasks, and eliminate repeated retrieval errors on code repositories.

---

## 2. Technical Deliverables & Fellow Contributions

During this research cycle, the following technical deliverables were designed, implemented, and verified in `/Users/jaswanthibanoth/Desktop/jev`:

### A. Core Architecture Deliverables
1. **Interactive Search REPL ([`adaptive_search_repl.ipynb`](adaptive_search_repl.ipynb)):**
   * Built a discrete cell parser (`RepoIndex`) that strips JSON metadata noise from `.ipynb` files.
   * Integrated five search tools: `ripgrep_search` (CLI `rg`), `lexical_search` (scoped regex), `fuzzy_search` (sequence matcher over headings), `inspect_structure`, and `view_cell`.
   * Implemented persistent execution logging (`SearchTraceLog`) to capture queries, hits, and qualitative lessons.
2. **Persistent Memory Workspace ([`memory-bin/`](memory-bin/README.md)):**
   * Designed a three-tier architecture separating raw telemetry (`traces/`), codified heuristics (`lessons/`), and materialized search indexes (`indexes/`).
   * Materialized four persistent indexes:
     * [`heading_structure_index.json`](memory-bin/indexes/heading_structure_index.json): 169 true Markdown headings mapped to files and cell offsets.
     * [`regex_alias_index.json`](memory-bin/indexes/regex_alias_index.json): 8 conceptual intents mapped to precompiled regex patterns and target chapters, featuring active usage telemetry (`times_used`, `successful_uses`, `tested`).
     * [`pragmatic_task_index.json`](memory-bin/indexes/pragmatic_task_index.json): 6 engineering workflows mapped to contiguous cell blocks.
     * [`lexical_vector_index.json`](memory-bin/indexes/lexical_vector_index.json): Subword (char 3-gram + word) TF-IDF vector space model with cosine similarity over 528 cells (*classified accurately as lexical vector-space, not neural embeddings*).

### B. Retrieval & Evaluation Pipeline Deliverables
1. **Index Routing & Anti-Hijacking Protection (`MemoryBinIndexManager`):**
   * Implemented dynamic index loading and candidate matching.
   * Built false-positive protection: filtered conversational stop words, penalized single-token coincidental overlap on multi-word aliases, and enforced an acceptance threshold of $\ge 0.60$.
   * *Verified Behavior:* Rejected an unrelated query (*"How can I search for dense layers in PyTorch model architectures?"*) at confidence 0.500 ($< 0.60$), preventing index hijacking.
2. **Decoupled Evaluation Interface (`BenchmarkEvaluator`):**
   * Created an independent post-search evaluation contract validating `required_all`, `required_any`, and `expected_files` tokens.
   * Ensured complete separation of search from grading: the agent selects tools and queries without awareness of evaluation criteria.
3. **Local Cell-Block Expansion & Ranked Anchor Selection:**
   * Built generic setup boilerplate filtering (`is_setup_or_install_cell`) to deprioritize line 1 `!pip install` cells.
   * Implemented candidate anchor ranking (`score_candidate_anchor`): prioritizes executable code cells with imports/API calls (score 30.0) over markdown section headings (score 10.0) and setup boilerplate (score -1.0).
   * Built automatic cell-block expansion (`expand_cell_block`) expanding a bounded $\pm 3$ cell window around the selected anchor.

---

## 3. Search Failures Diagnosed & Addressed

The research followed an iterative failure-driven methodology, formally cataloged in [`memory-bin/lessons/failure_modes.json`](memory-bin/lessons/failure_modes.json):

| Failure ID | Observed Failure Mode | Trigger / Symptom | Engineering Solution Implemented | Target Index / Mechanism |
| :--- | :--- | :--- | :--- | :--- |
| **`FM-01`** | **Conversational Prose Mismatch** | Querying `"dense retrieval failure"` yielded 0 hits because the chapter used `"Caveats of Dense Retrieval"`. | Pre-compiled regex patterns mapping human intents to technical symbols. | [`regex_alias_index.json`](memory-bin/indexes/regex_alias_index.json) |
| **`FM-02`** | **Disconnected Token Grep** | Grepping for `"bm25"` returned 5 isolated snippets without explanatory narrative or outputs. | Mapped engineering workflows to contiguous cell blocks and added local cell expansion. | [`pragmatic_task_index.json`](memory-bin/indexes/pragmatic_task_index.json) & `expand_cell_block` |
| **`FM-03`** | **Typo & Keyword Rigidity** | Literal grep for `"Transformer-based"` missed Chapter 10's heading due to an author typo (`"Tranformer-based"`). | Sequence similarity and subword character 3-gram indexing. | `fuzzy_heading_search` & [`lexical_vector_index.json`](memory-bin/indexes/lexical_vector_index.json) |
| **`FM-04`** | **Notebook JSON Schema Flooding** | Raw grep on `.ipynb` files flooded context with JSON keys, escape characters, and base64 strings. | Parsed notebooks into discrete, clean `markdown` and `code` cells. | `RepoIndex` parser in [`adaptive_search_repl.ipynb`](adaptive_search_repl.ipynb) |
| **`FM-05`** | **Boilerplate Snippet Starvation** | Initial ripgrep hits matched line 1 `!pip install` commands, consuming the narrow 6-snippet window before code could be seen. | Deprioritized pip-install cells and ranked executable code cells above headings. | `is_setup_or_install_cell` & `select_ranked_anchor` |

---

## 4. Controlled Benchmarks & Observed Outcomes

The system was evaluated across three controlled A/B pilots on four frozen benchmark questions (two index-assisted, two un-indexed negative controls):

### Pilot Progression Summary:
1. **Pilot 1 (Initial A/B Run; [`first_index_ab`](memory-bin/experiments/first_index_ab/)):** Both conditions scored **0 / 4 correct evidence** due to setup boilerplate (`!pip install`) starving the snippet window.
2. **Pilot 2 (Local Cell Expansion; [`first_index_ab_with_local_expansion`](memory-bin/experiments/first_index_ab_with_local_expansion/)):** Local cell expansion ($\pm 3$ cells) was added to both conditions. However, the anchor selector picked the first non-boilerplate hit (markdown heading Cell 22) instead of the model import (Cell 37), missing the GGUF model load call.
3. **Pilot 3 (Verified Benchmark with Ranked Anchor Selection; [`benchmark_v2_ranked_anchors_ab_run`](memory-bin/experiments/benchmark_v2_ranked_anchors_ab_run/)):**
   * MEMORY ON scored **1 / 4 correct evidence** (`q2_local_quantized_runtime` passed).
   * MEMORY OFF scored **0 / 4 correct evidence**.
   * Failed searches: MEMORY ON had **1** vs. MEMORY OFF had **2** (50% reduction).
   * Global searches: MEMORY ON had **5** vs. MEMORY OFF had **6**.
   * False-positive routing on negative controls: **0** (router correctly rejected `q3` and `q4`).

### Ground-Truth Benchmark Auditing:
Before finalizing Pilot 3, an audit of the benchmark specifications against the actual repository code exposed three errors in our initial evaluation contracts:
* *`q1_topic_diversity`:* Discovered a benchmark note typo claiming `diversity=0.8`. Repository line 2081 defines `MaximalMarginalRelevance(diversity=0.5)`. `0.8` does not exist in the codebase.
* *`q2_local_quantized_runtime`:* Discovered a structural conflation: CoT/ToT reasoning (Cells 23–29) runs on standard Hugging Face pipelines, while the quantized GGUF runtime (`llama_cpp`) is instantiated in Cell 37 (14 cells apart).
* *`q4_kv_cache_speedup`:* Removed hallucinated token `past_key_values`. Replaced with actual repository code parameter: `use_cache=True`.
* *Audit Result:* Preserved the original benchmark and created [`benchmark_questions_v2_verified.json`](memory-bin/experiments/benchmark_questions_v2_verified.json).

---

## 5. Project Supervision & Documented Feedback

**Project Supervisor:** Dev

During this research cycle, supervisory technical feedback directly steered key architecture and communication decisions:
1. **Executive Conciseness & High Signal:** Directed the condensation of verbose narrative traces into an executive-level, under-2,000-character technical update, enforcing a strict focus on empirical facts over speculative prose.
2. **Native Tooling Integration:** Directed the integration of the native CLI `ripgrep (rg)` binary into the search agent’s toolbox rather than relying solely on pure Python regex.
3. **Index Persistence Architecture:** Directed the project away from ephemeral text summaries toward creating the dedicated `memory-bin/` workspace for materialized, reusable regex indexes and structured failure taxonomies.

*(Note: Feedback is documented from technical directions and commit history; no formal HR approvals or backdated administrative sign-offs are implied).*

---

## 6. Remaining Limitations & Next Milestone

### Current Limitations:
1. **Fixed-Window Distance Gap (`q1`):** In Chapter 5, the topic representation heading is at Cell 38, but the `MaximalMarginalRelevance(diversity=0.5)` implementation is at Cell 46. A fixed $\pm 3$ cell window anchored at Cell 36 stops at Cell 39, missing the code definition by 7 cells.
2. **Lexical Representation Boundary:** When queries share zero token overlap with headings or aliases (e.g. `q3`), the lexical TF-IDF index cannot compute continuous semantic proximity.

### Next Research Milestone (Target Date: October 31, 2026):
1. **Adaptive Heading-to-Heading Block Expansion:** Implement dynamic cell expansion that reads forward from the anchor cell until the next major heading (`##`) or a maximum of 10 cells, resolving long-range cell spans like `q1`.
2. **Benchmark Scaling:** Expand the verified benchmark from 4 to 15+ questions across all 12 chapters to evaluate statistically significant recall differences.
