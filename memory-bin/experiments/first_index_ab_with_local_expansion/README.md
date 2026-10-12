# Experiment: Controlled A/B Benchmark with Automatic Local Cell Expansion

## Purpose
Test whether adding automatic local cell-block expansion (±3 cells around promising hits) and generic boilerplate deprioritization resolves the retrieval bottleneck identified in the initial pilot, while holding benchmark questions and tools constant.

## Benchmark Questions (Frozen)
1. `q1_topic_diversity`: Topic modeling keyword diversity and MMR class (Target: Chapter 5).
2. `q2_local_reasoning_runtime`: Local CoT/ToT reasoning runtime (`llama_cpp`) (Target: Chapter 6).
3. `q3_multimodal_captioning`: Vision-language architecture for captioning/VQA (Target: Chapter 9, un-indexed negative control).
4. `q4_kv_cache_speedup`: Attention optimization caching past keys and values (Target: Chapter 3, un-indexed negative control).

## Comparative Results

| Question | Condition | First Action | Global Searches | Local Inspections | Failed Searches | Index Used | Correct Evidence |
| :--- | :--- | :--- | :---:| :---:| :---:| :--- | :---: |
| `q1_topic_diversity` | MEMORY OFF | `fuzzy_headings` | 1 | 1 | 0 | None | False |
| `q1_topic_diversity` | MEMORY ON | `ripgrep_adapted_regex` | 1 | 1 | 0 | `topic_modeling_diversity` | False |
| `q2_local_reasoning_runtime` | MEMORY OFF | `fuzzy_headings` | 1 | 1 | 0 | None | False |
| `q2_local_reasoning_runtime` | MEMORY ON | `ripgrep_adapted_regex` | 1 | 1 | 0 | `reasoning_local_models` | **True** |
| `q3_multimodal_captioning` | MEMORY OFF | `fuzzy_headings` | 2 | 0 | 1 | None | False |
| `q3_multimodal_captioning` | MEMORY ON | `fuzzy_headings` | 2 | 0 | 1 | None | False |
| `q4_kv_cache_speedup` | MEMORY OFF | `fuzzy_headings` | 1 | 1 | 0 | None | False |
| `q4_kv_cache_speedup` | MEMORY ON | `fuzzy_headings` | 1 | 1 | 0 | None | False |

## Observed Outcomes
1. **Conversion to Verified Evidence:** Local cell-block expansion converted `q2_local_reasoning_runtime` from a failure into verified evidence under MEMORY ON (`Correct: True`), while MEMORY OFF remained `False`.
2. **Mechanistic Cause of Q2 Difference:** 
   - Under MEMORY ON, the persistent alias index supplied the pre-compiled regex `(Tree-of-Thought|...|llama_cpp|...)`. This matched `llama_cpp` in Cell 2 and `Tree-of-Thought` in Cell 22. Combining regex line hits with localized expansion around Cell 22 retrieved both required gold tokens.
   - Under MEMORY OFF, heading search anchored on Cell 26 (`## Tree-of-Thought`) and expanded cells 23–29, but missed the import `llama_cpp` (located in Cell 2).
3. **Boilerplate Deprioritization:** Boilerplate filtering correctly bypassed line 1 installation cells (`!pip install llama-cpp-python`, `!pip install bertopic`) to select substantive code and heading cells as anchors.
4. **Negative Controls:** Both conditions correctly rejected index routing on `q3` and `q4` with 0 false-positive routing, executing identical fallback paths.
