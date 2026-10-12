# Adaptive Agent Search & Repository Memory

**Humanitarians AI Research Project**  
**Investigator / Fellow:** Jaswanthi Banoth  
**Lead Agent Architecture:** Optimus (arkenOS runtime, `gemini-3.8-flash` Frontier driver)  
**Target Repository Under Study:** [`HandsOnLLM/Hands-On-Large-Language-Models`](https://github.com/HandsOnLLM/Hands-On-Large-Language-Models) (O'Reilly, 2024; 83 files, 169 Markdown headings, 528 cells/docs)  

---

## 1. Project Background & Research Objective

Autonomous software agents frequently fail when searching complex codebases and notebook repositories:
1. **Conversational vs. Code Vocabulary Mismatch:** Users ask questions in high-level prose (*"How do I adapt an embedding model to a new domain without labels?"*), but codebases implement them using concise section headings (*"Unsupervised Learning"*) and specific library symbols (*`TSDAE`*, *`DenoisingAutoEncoderLoss`*). Naive string greps fail on Turn 1 with 0 hits.
2. **Notebook JSON Noise & Spatial Distribution:** Jupyter notebooks (`.ipynb`) are stored as raw JSON. Naive text searches flood LLM context with base64 image strings and escape characters. Furthermore, notebook logic is chronologically distributed across cells: imports happen in Cell 2, prompt definitions in Cell 23, and model instantiations in Cell 37. Isolated snippet greps miss necessary context.

**Core Research Question:**  
*Can an autonomous agent record its own search failures, synthesize persistent structural indexes, and reuse them to eliminate repeated search failures on unseen repository tasks?*

---

## 2. What Was Built

```text
adaptive-agent-search/
├── README.md                                  # Repository documentation & experimental summary
├── .gitignore                                 # Clean exclusion rules for external repos & runtime data
├── adaptive_search_repl.ipynb                 # Interactive REPL environment with clean notebook cell parsing
└── memory-bin/                                # Persistent memory workspace
    ├── README.md                              # Architecture & query quickstart guide
    ├── indexes/                               # Reusable materialized indexes
    │   ├── heading_structure_index.json       # 169 true Markdown headings with cell offsets
    │   ├── regex_alias_index.json             # 8 concept-to-token alias mappings with compiled regexes
    │   ├── pragmatic_task_index.json          # 6 engineering workflows mapped to contiguous cell blocks
    │   └── lexical_vector_index.json          # Subword (char 3-gram + word) TF-IDF vector space model
    ├── lessons/                               # Codified failure lessons & taxonomies
    │   ├── failure_modes.json                 # Structured failure taxonomy (FM-01 through FM-04)
    │   └── search_lessons.md                  # Operational search principles
    ├── traces/                                # Raw execution trace logs
    │   ├── cumulative_search_trace.json       # Multi-session search attempts
    │   ├── dense_failure_trace.json           # Chapter 8 dense retrieval caveat investigation
    │   ├── connected_agent_trace.json         # Automated index-routed search trace
    │   └── telemetry_verification_trace.json  # Telemetry verification (times_used & index_reused)
    └── experiments/                           # Controlled benchmarks & ablation studies
        ├── benchmark_questions.json           # Original frozen 4-question benchmark
        ├── benchmark_questions_v2_verified.json # Audited, ground-truth verified benchmark
        ├── first_index_ab/                    # Pilot 1 initial A/B run traces
        ├── first_index_ab_with_local_expansion/ # Pilot 2 traces with local cell expansion
        └── benchmark_v2_ranked_anchors_ab_run/  # Pilot 3 final verified traces with ranked anchor selection
```

### Core Components:
1. **`adaptive_search_repl.ipynb`:** Parses `.ipynb` files into clean `markdown` and `code` cells, providing search primitives (`ripgrep_search`, `lexical_search`, `fuzzy_search`, `view_cell`) and execution logging (`SearchTraceLog`).
2. **`memory-bin/indexes/`:** Materializes persistent indexes born from failure diagnoses.
   * *Classification note:* `lexical_vector_index.json` is accurately classified as a **subword lexical vector-space index (TF-IDF / character n-gram cosine similarity)**, *not* a neural embedding model.
3. **Decoupled Evaluation Interface (`BenchmarkEvaluator`):** An independent evaluator that validates search outputs post-search against strict contracts (`required_all`, `required_any`, `expected_files`) without leaking criteria to the agent.
4. **Local Cell-Block Expansion & Ranked Anchor Selection:**
   * Generic boilerplate filter (`is_setup_or_install_cell`) deprioritizes line 1 `!pip install` cells.
   * Candidate anchor scoring ranks executable code cells with imports/API calls (score 30.0) above markdown section headings (score 10.0) and boilerplate (score -1.0).
   * Automatically expands a bounded $\pm 3$ cell window around the selected anchor.

---

## 3. Verified Experimental Results (Pilot 3)

The benchmark was executed across **4 frozen questions** stored in `memory-bin/experiments/benchmark_questions_v2_verified.json` under paired MEMORY OFF vs. MEMORY ON conditions:

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

### Aggregate Metrics:
* **Correct Evidence Found:** MEMORY ON: **1 / 4 (25%)** | MEMORY OFF: **0 / 4 (0%)**
* **Failed Searches:** MEMORY ON: **1** | MEMORY OFF: **2** (50% reduction)
* **Global Searches:** MEMORY ON: **5** | MEMORY OFF: **6** (-1 redundant search)
* **False-Positive Index Routing:** **0** (router correctly rejected negative controls `q3` and `q4`).

---

## 4. What the Findings Prove and Do Not Prove

### What is Supported:
1. **Elimination of Turn-1 Conversational Failures:** On `q2`, MEMORY ON converted a 16-word conversational query into an exact pre-scoped regex on Turn 1, whereas MEMORY OFF attempted a fuzzy heading search for `"local quantized runtime"`, found 0 hits, and had to fall back to an un-scoped search.
2. **Code-Specific Anchor Ranking is Crucial:** Ranking code imports (Cell 37) above section headings (Cell 22) was the decisive factor that allowed local expansion to capture the actual GGUF model load call (`Llama.from_pretrained`), converting `q2` from a failure into verified evidence.
3. **Thresholding Prevents False-Positive Hijacking:** On the two un-indexed negative control questions (`q3` and `q4`), the router correctly rejected candidate matches (confidence below 0.60) and executed identical fallback logic to MEMORY OFF.

### What is NOT Claimed:
1. **Does not prove general autonomous learning:** The agent did not modify model weights or learn new neural policies. It retrieved deterministic, pre-compiled regexes from persistent JSON files.
2. **Does not solve long-range cell spans:** On `q1`, both conditions reached the correct notebook but failed within a single $\pm 3$ cell window because the class definition was 10 cells away from the heading.
3. **Does not establish a statistical benchmark:** This was a controlled 4-question pilot. A general percentage improvement cannot be claimed without scaling to 15+ questions across all 12 chapters.

---

## 5. Reproduction Quickstart

```bash
# 1. Clone the testbed repository
git clone https://github.com/HandsOnLLM/Hands-On-Large-Language-Models.git

# 2. Open and run the interactive REPL notebook
jupyter notebook adaptive_search_repl.ipynb

# 3. Query the persistent index in Python
python3 -c "
import json
with open('memory-bin/indexes/regex_alias_index.json') as f:
    data = json.load(f)
print('Total persistent concepts:', len(data['aliases']))
"
```
