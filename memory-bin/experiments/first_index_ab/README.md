# First Controlled MEMORY/INDEX ON vs OFF Experiment

## Objective
Evaluate whether persistent regex/alias indexes improve search efficiency on 4 unseen benchmark questions while holding tools, repository representations, and evaluators constant.

## Benchmark Questions
1. `q1_topic_diversity`: In topic modeling representations, how does the pipeline control keyword diversity to prevent redundant topic terms, and what class is used? (Target: Ch 5, Index expected: YES)
2. `q2_local_reasoning_runtime`: When performing Chain of Thought or Tree of Thought reasoning locally, what library runtime executes the quantized model? (Target: Ch 6, Index expected: YES)
3. `q3_multimodal_captioning`: What vision-language architecture is used for image captioning and visual question answering in the multimodal chapter? (Target: Ch 9, Index expected: NO)
4. `q4_kv_cache_speedup`: What attention optimization mechanism is implemented to speed up autoregressive token generation by storing past projection tensors? (Target: Ch 3, Index expected: NO)

## Paired Results

| Question | Condition | First Action | Actions | Failed Searches | Index Used | Correct Evidence |
| :--- | :--- | :--- | :---:| :---:| :--- | :---: |
| `q1_topic_diversity` | MEMORY OFF | `fuzzy_headings` | 2 | 0 | None | False |
| `q1_topic_diversity` | MEMORY ON | `ripgrep_adapted_regex` | 2 | 0 | `topic_modeling_diversity` | False |
| `q2_local_reasoning_runtime` | MEMORY OFF | `fuzzy_headings` | 2 | 0 | None | False |
| `q2_local_reasoning_runtime` | MEMORY ON | `ripgrep_adapted_regex` | 2 | 0 | `reasoning_local_models` | False |
| `q3_multimodal_captioning` | MEMORY OFF | `fuzzy_headings` | 2 | 1 | None | False |
| `q3_multimodal_captioning` | MEMORY ON | `fuzzy_headings` | 2 | 1 | None | False |
| `q4_kv_cache_speedup` | MEMORY OFF | `fuzzy_headings` | 2 | 0 | None | False |
| `q4_kv_cache_speedup` | MEMORY ON | `fuzzy_headings` | 2 | 0 | None | False |

## Key Findings
1. Index routing occurred exclusively on the 2 questions where an index was expected to help (`q1` and `q2`).
2. Zero false-positive routing was observed on the 2 un-indexed negative controls (`q3` and `q4`), correctly falling back to baseline heuristics.
3. In this pilot, neither condition surfaced complete gold evidence within the 2-action retrieval budget due to top-of-file installation boilerplate consuming snippet limits.
