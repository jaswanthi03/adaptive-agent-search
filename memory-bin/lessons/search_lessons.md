# Reusable Search Lessons & Adaptive Strategies

## Core Principle: Index Evolution from Search Failures
A textual lesson tells an agent what *not* to do. An **adaptive index** gives the agent an immediate structural path to succeed next time. Whenever a search failure reveals an impedance mismatch between user queries and repository structure, we materialize that understanding into an index in `memory-bin/indexes/`.

---

## 1. The Two-Stage Funnel (Heading Anchor → Spatial Cell Block)
* **Observed Failure:** In Chapter 8, searching for `"dense retrieval failure"` yielded 0 hits. In Chapter 7, `"Conversation Summary Memory"` yielded 0 hits.
* **Mechanism:** In Jupyter notebook codebases, narrative titles are declared in Markdown headers, while code cells execute technical mechanics.
* **Generalization:** Always query the **Heading Structure Index** (`heading_structure_index.json`) to anchor the section, then inspect the sequential neighborhood with `view_cell`.

## 2. Bridging Colloquial Intent to Implementation Tokens
* **Observed Failure:** Users ask for *"unsupervised embedding adaptation"*, but the code implements `TSDAE`, `losses.DenoisingAutoEncoderLoss`, and `del_ratio`.
* **Mechanism:** Concept-to-code gap. 
* **Generalization:** Maintain the **Regex & Alias Index** (`regex_alias_index.json`) mapping high-level concepts to precompiled regex patterns and library classes.

## 3. Spatial Locality in Notebook Pipelines
* **Observed Failure:** Grepping for `bm25` in Chapter 8 returned 5 disconnected snippets, missing the narrative comparison.
* **Mechanism:** Notebook cells are written chronologically (Imports → Data Prep → Model Config → Execution → Output).
* **Generalization:** Maintain the **Pragmatic Task Index** (`pragmatic_task_index.json`) mapping engineering tasks to contiguous cell blocks.

## 4. Semantic Subword Matching Over Exact Tokens
* **Observed Failure:** Minor typos (e.g. `"Tranformer-based"` in Chapter 10) or vocabulary divergence cause exact matches to fail silently.
* **Mechanism:** Exact token matching is brittle.
* **Generalization:** Maintain the **Semantic Embedding Index** (`semantic_embedding_index.json`) using continuous subword character 3-grams and TF-IDF cosine similarity.
