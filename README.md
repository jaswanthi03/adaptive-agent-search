# Adaptive Agent Search & Repository Memory

**Humanitarians AI Fellowship Research Project**  
**Fellow:** Jaswanthi Banoth  
**Project Lead / Agent:** Optimus (arkenOS runtime, `gemini-3.8-flash` Frontier driver)  
**Project Supervisor:** Dev  
**Target Repository Under Study:** [`HandsOnLLM/Hands-On-Large-Language-Models`](https://github.com/HandsOnLLM/Hands-On-Large-Language-Models) (O'Reilly, 2024; 83 files, 169 Markdown headings, 528 cells/docs)  

---

## 1. Project Background & Research Objective

Autonomous software agents frequently struggle when retrieving information from code and notebook repositories:
1. **Conversational vs. Code Vocabulary Mismatch:** Users ask questions in high-level prose (*"How do I adapt an embedding model to a new domain without labels?"*), whereas codebases organize content by concise headings (*"Unsupervised Learning"*) and specific symbols (*`TSDAE`*, *`DenoisingAutoEncoderLoss`*). Naive string greps fail on Turn 1 with 0 hits.
2. **Notebook JSON Noise & Spatial Distribution:** Jupyter notebooks (`.ipynb`) are stored as raw JSON. Naive text searches flood LLM context with base64 image strings and escape characters. Furthermore, notebook implementations are chronologically distributed across cells: imports happen in Cell 2, prompt templates in Cell 23, and model instantiations in Cell 37. Isolated snippet greps miss necessary context.

**Core Research Question:**  
*Can an autonomous agent record its own search failures, synthesize persistent structural indexes, and reuse them to eliminate repeated search failures on unseen repository tasks?*

---

## 2. Repository Structure & Artifacts

All research code, persistent indexes, failure taxonomies, and experimental benchmarks are organized under:

```text
adaptive-agent-search/
├── README.md                                           # Repository overview & reproduction guide
├── RESEARCH_PROGRESS.md                                # Fellowship progress, contributions & supervision history
├── renewal-report.md                                   # Fellowship renewal evidence document
├── .gitignore                                          # Clean exclusions (external repos, runtime state)
├── adaptive_search_repl.ipynb                          # Interactive REPL notebook with clean cell parsing
└── memory-bin/                                         # Persistent memory workspace
    ├── README.md                                       # Architecture & query guide
    ├── indexes/                                        # Reusable materialized indexes
    │   ├── heading_structure_index.json                # 169 true Markdown headings with cell offsets
    │   ├── regex_alias_index.json                      # 8 concept-to-token alias mappings with compiled regexes
    │   ├── pragmatic_task_index.json                   # 6 engineering workflows mapped to contiguous cell blocks
    │   └── lexical_vector_index.json                   # Subword (char 3-gram + word) TF-IDF vector space model
    ├── lessons/                                        # Codified failure lessons & taxonomies
    │   ├── failure_modes.json                          # Structured failure taxonomy (FM-01 through FM-04)
    │   └── search_lessons.md                           # Operational search principles
    ├── traces/                                         # Raw execution trace logs
    │   ├── cumulative_search_trace.json                # Multi-session search attempts
    │   ├── dense_failure_trace.json                    # Chapter 8 dense retrieval caveat investigation
    │   ├── connected_agent_trace.json                  # Automated index-routed search trace
    │   └── telemetry_verification_trace.json           # Telemetry verification (times_used & index_reused)
    └── experiments/                                    # Controlled benchmarks & ablation studies
        ├── benchmark_questions.json                    # Original frozen 4-question benchmark
        ├── benchmark_questions_v2_verified.json          # Audited, ground-truth verified benchmark
        ├── first_index_ab/                             # Pilot 1 initial A/B run traces
        ├── first_index_ab_with_local_expansion/        # Pilot 2 traces with local cell expansion
        └── benchmark_v2_ranked_anchors_ab_run/         # Pilot 3 final verified traces with ranked anchor selection
```

### Direct Links to Key Artifacts:
* **Interactive Notebook:** [`adaptive_search_repl.ipynb`](adaptive_search_repl.ipynb)
* **Materialized Indexes:**
  * [`heading_structure_index.json`](memory-bin/indexes/heading_structure_index.json) (Structural hierarchy of 169 headings)
  * [`regex_alias_index.json`](memory-bin/indexes/regex_alias_index.json) (Precompiled regex patterns and target chapters)
  * [`pragmatic_task_index.json`](memory-bin/indexes/pragmatic_task_index.json) (Workflow pipeline mappings)
  * [`lexical_vector_index.json`](memory-bin/indexes/lexical_vector_index.json) (Subword TF-IDF model over 528 cells)
* **Failure Taxonomies & Lessons:**
  * [`failure_modes.json`](memory-bin/lessons/failure_modes.json) (Formal catalog of failure patterns FM-01 to FM-04)
  * [`search_lessons.md`](memory-bin/lessons/search_lessons.md) (Codified search heuristics)
* **Audited Benchmarks & Experiment Traces:**
  * [`benchmark_questions_v2_verified.json`](memory-bin/experiments/benchmark_questions_v2_verified.json) (Audited benchmark ground truth)
  * [`benchmark_questions.json`](memory-bin/experiments/benchmark_questions.json) (Original frozen benchmark)
  * Final Paired Summary: [`experiment_summary.json`](memory-bin/experiments/benchmark_v2_ranked_anchors_ab_run/experiment_summary.json)
  * Final Paired Traces: [`full_traces_paired.json`](memory-bin/experiments/benchmark_v2_ranked_anchors_ab_run/full_traces_paired.json)
  * Earlier Ablations: [`Pilot 1 Summary`](memory-bin/experiments/first_index_ab/experiment_traces_summary.json) | [`Pilot 2 Summary`](memory-bin/experiments/first_index_ab_with_local_expansion/experiment_summary.json)

---

## 3. Implementation Details

1. **Jupyter Cell Parser (`RepoIndex`):** Eliminates raw JSON metadata noise by parsing `.ipynb` files into discrete `markdown` and `code` cells, cataloging 169 Markdown headings and 528 cells/files.
2. **Index Loading & Routing (`MemoryBinIndexManager`):** Loads [`regex_alias_index.json`](memory-bin/indexes/regex_alias_index.json) and routes natural-language questions to pre-compiled regexes. Enforces a confidence threshold of $\ge 0.60$ with stop-word filtering to prevent false-positive index hijacking.
3. **Decoupled Evaluation Interface (`BenchmarkEvaluator`):** An independent evaluation contract that grades retrieved evidence post-search against `required_all`, `required_any`, and `expected_files` tokens. The agent has zero knowledge of gold criteria during retrieval.
4. **Ranked Anchor Selection & Local Cell Expansion:**
   * Generic boilerplate filter (`is_setup_or_install_cell`) deprioritizes line 1 `!pip install` cells.
   * Anchor scorer (`score_candidate_anchor`) ranks executable code cells with imports/API calls (score 30.0) above markdown section headings (score 10.0) and setup boilerplate (score -1.0).
   * Automatically expands a bounded $\pm 3$ cell window around the top-ranked anchor.
5. **Representation Note:** [`lexical_vector_index.json`](memory-bin/indexes/lexical_vector_index.json) is strictly a **subword lexical vector-space model (character 3-gram + word TF-IDF with cosine similarity)**. It does not use neural bi-encoders (e.g. `sentence-transformers`), which were excluded from this phase.

---

## 4. Controlled Benchmark Results (Pilot 3)

The benchmark was evaluated across **4 frozen questions** stored in [`benchmark_questions_v2_verified.json`](memory-bin/experiments/benchmark_questions_v2_verified.json) under paired MEMORY OFF vs. MEMORY ON conditions. Both conditions used the identical search tools, action budgets, and post-search evaluator:

| Question ID | Target Chapter | Condition | First Action Tool | Global Searches | Local Inspections | Failed Searches | Index Used | Correct Evidence |
| :--- | :--- | :--- | :--- | :---:| :---:| :---:| :--- | :---: |
| **`q1_topic_diversity`** | Ch 5 | MEMORY OFF | `fuzzy_headings` | 1 | 1 | 0 | None | False |
| **`q1_topic_diversity`** | Ch 5 | MEMORY ON | `ripgrep_adapted_regex` | 1 | 1 | 0 | `topic_modeling_diversity` | False |
| **`q2_local_quantized_runtime`** | Ch 6 | MEMORY OFF | `fuzzy_headings` | 2 | 0 | 1 | None | False |
| **`q2_local_quantized_runtime`** | Ch 6 | MEMORY ON | `ripgrep_adapted_regex` | 1 | 1 | 0 | `reasoning_local_models` | **True** |
| **`q3_multimodal_captioning`** | Ch 9 | MEMORY OFF | `fuzzy_headings` | 2 | 0 | 1 | None | False |
| **`q3_multimodal_captioning`** | Ch 9 | MEMORY ON | `fuzzy_headings` | 2 | 0 | 1 | None | False |
| **`q4_kv_cache_speedup`** | Ch 3 | MEMORY OFF | `fuzzy_headings` | 1 | 1 | 0 | None | False |
| **`q4_kv_cache_speedup`** | Ch 3 | MEMORY ON | `fuzzy_headings` | 1 | 1 | 0 | None | False |

### Verified Summary Metrics:
* **Correct Evidence Found:** MEMORY ON: **1 / 4 (25%)** | MEMORY OFF: **0 / 4 (0%)**
* **Failed Search Actions:** MEMORY ON: **1** | MEMORY OFF: **2** (on this specific 4-question run)
* **Global Searches:** MEMORY ON: **5** | MEMORY OFF: **6**
* **False-Positive Index Routing:** **0** (router correctly rejected negative controls `q3` and `q4`).

---

## 5. Observations and Limitations

### Empirical Observations from this Pilot:
1. **Targeted Turn-1 Routing:** On `q2`, MEMORY ON converted a 16-word conversational query into an exact pre-scoped regex on Turn 1 (`(Tree-of-Thought|...|llama_cpp|...)`). In contrast, MEMORY OFF attempted a fuzzy heading search for `"local quantized runtime"`, found 0 hits (a failed search), and had to execute an un-scoped fallback search.
2. **Code-Specific Anchor Ranking is Critical:** In `q2`, ranking code imports (Cell 37, score 30.0) above section headings (Cell 22, score 10.0) enabled local cell expansion to capture `from llama_cpp.llama import Llama`, `Llama.from_pretrained`, and `Phi-3-mini-4k-instruct-q4.gguf`, satisfying all gold evaluator criteria.
3. **Safe Rejection on Negative Controls:** On the two un-indexed negative control questions (`q3` and `q4`), the router correctly rejected candidate matches (confidence below 0.60) and executed identical fallback logic to MEMORY OFF with zero false-positive routing.

### Current Limitations & Failure Analysis:
1. **Fixed-Window Distance Failure (`q1`):** In Chapter 5, the topic representation heading is at Cell 38, but the `MaximalMarginalRelevance(diversity=0.5)` implementation is at Cell 46. A fixed $\pm 3$ cell window anchored at Cell 36 stops at Cell 39, missing the code definition by 7 cells. Both conditions failed because of this spatial span.
2. **Heuristic Keyword Mismatches (`q3` and `q4`):** On `q3`, the heuristic heading query `"vision-language architecture image"` did not match the concise chapter headings (`## BLIP-2` and `## CLIP`). On `q4`, the heading query `"attention optimization mechanism"` matched a README instead of Chapter 3.
3. **Pilot Scope:** The reduction in failed searches (1 vs. 2) and the single solved question (1 vs. 0) are observations from a small 4-question pilot. They do **not** establish a general performance improvement or prove autonomous learning.
4. **Lexical Representation Boundary:** When queries share zero token overlap with headings or aliases, the lexical TF-IDF index cannot compute continuous semantic proximity.

---

## 6. Reproduction Quickstart

The following instructions have been verified and execute cleanly:

### Step 1: Clone the Reference Repository
```bash
# Clone the repository under study into the workspace root
git clone https://github.com/HandsOnLLM/Hands-On-Large-Language-Models.git
```

### Step 2: Verify the Persistent Indexes
```bash
python3 -c "import json; data = json.load(open('memory-bin/indexes/regex_alias_index.json')); print('Cataloged concepts:', len(data['aliases']))"
```
*Expected output:* `Cataloged concepts: 8`

### Step 3: Inspect the Verified Benchmark Specification
```bash
python3 -c "import json; data = json.load(open('memory-bin/experiments/benchmark_questions_v2_verified.json')); print('Benchmark questions:', len(data))"
```
*Expected output:* `Benchmark questions: 4`

### Step 4: Inspect the Final Pilot 3 Experiment Summary
```bash
python3 -c "import json; data = json.load(open('memory-bin/experiments/benchmark_v2_ranked_anchors_ab_run/experiment_summary.json')); print('Paired test items:', len(data))"
```
*Expected output:* `Paired test items: 4`

### Step 5: Run the Interactive Notebook
```bash
jupyter notebook adaptive_search_repl.ipynb
```

---

## 7. Fellowship Documentation & Retrospective Evidence

As part of the **Humanitarians AI Fellowship**, the complete research history, retrospective work logs, technical bottlenecks, and artifact verification records are cataloged in the [`fellowship-evidence/`](fellowship-evidence/) directory:

* [**Fellowship Work Log (`fellowship-evidence/WORK_LOG.md`)**](fellowship-evidence/WORK_LOG.md): Reconstructed weekly activity log (September 4 – October 12, 2026), contractual commitment details (20–40 hours/week), technical classifications, and evidentiary anchors.
* [**Frictional Analysis & Bottleneck Log (`fellowship-evidence/FRICTIONAL.md`)**](fellowship-evidence/FRICTIONAL.md): In-depth examination of technical failure modes (video artifact rendering timeouts, code boilerplate starvation, cell-span truncation ceiling, anchor selection traps), benchmark verification analysis, and administrative friction.
* [**Evidence Audit & Artifact Map (`fellowship-evidence/EVIDENCE.md`)**](fellowship-evidence/EVIDENCE.md): Immutable audit trail linking reported milestones to physical files, Git commits, Optimus session transcripts, and benchmark evaluation proofs (with sensitive personal data redacted).

