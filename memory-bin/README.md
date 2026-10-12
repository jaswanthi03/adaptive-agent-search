# memory-bin: Persistent Workspace for Adaptive Search

`memory-bin/` is the dedicated workspace for recording search traces, codifying reusable lessons, and persisting structural indexes that evolve from search failures.

---

## Directory Architecture

```text
memory-bin/
├── README.md               # Architecture documentation & query quickstart
├── traces/                 # Raw, immutable search execution logs
│   ├── cumulative_search_trace.json   # Full session traces (Runs 1, 2, etc.)
│   └── dense_failure_trace.json       # Chapter 8 failure & adaptation trace
├── lessons/                # Codified diagnostic lessons and failure patterns
│   ├── search_lessons.md              # High-level synthesized heuristics
│   └── failure_modes.json             # Structured failure indicators & mitigations
└── indexes/                # Materialized, reusable indexes built from failure diagnoses
    ├── heading_structure_index.json   # Structural hierarchy of all 12 chapters & headings
    ├── regex_alias_index.json         # Colloquial concept -> Code token & regex mappings
    ├── pragmatic_task_index.json      # Engineering tasks -> Contiguous notebook cell blocks
    └── semantic_embedding_index.json  # Continuous vector space model for semantic retrieval
```

---

## The Four Reusable Indexes

Every index in `memory-bin/indexes/` records **why it was created** and **which search failures motivated it**:

| Index | Primary Purpose | Failure Mode That Motivated It |
| :--- | :--- | :--- |
| **`heading_structure_index.json`** | Hierarchical map of all 169 markdown headings across chapters. | FM-01: Broad prose greps yielding 0 hits because topics are announced in headings. |
| **`regex_alias_index.json`** | Maps colloquial concepts to exact library classes and regexes. | FM-01: Natural language query mismatch (e.g. "unsupervised adaptation" vs `TSDAE`). |
| **`pragmatic_task_index.json`** | Maps engineering workflows to contiguous cell spans. | FM-02: Grepping code tokens returns isolated snippets without narrative/outputs. |
| **`semantic_embedding_index.json`** | Subword vector space model for cosine similarity retrieval. | FM-03: Keyword rigidity and typos (e.g. `Tranformer-based` in Chapter 10). |

---

## How to Query the Indexes in Python

```python
import json
from pathlib import Path

# 1. Query Heading Structure Index
with open("memory-bin/indexes/heading_structure_index.json") as f:
    heading_idx = json.load(f)
hits = [h for h in heading_idx["headings"] if "unsupervised" in h["heading"].lower()]

# 2. Query Regex / Alias Index
with open("memory-bin/indexes/regex_alias_index.json") as f:
    alias_idx = json.load(f)
alias_entry = next(a for a in alias_idx["aliases"] if a["concept"] == "dense_retrieval_caveats")
print("Target regex:", alias_entry["regex"])

# 3. Query Pragmatic Task Index
with open("memory-bin/indexes/pragmatic_task_index.json") as f:
    task_idx = json.load(f)
task = next(t for t in task_idx["tasks"] if t["task_id"] == "unsupervised_domain_adaptation_embeddings")
print("Inspect cell range:", task["contiguous_cell_range"])

# 4. Query Semantic Embedding Index
with open("memory-bin/indexes/semantic_embedding_index.json") as f:
    emb_idx = json.load(f)
# Compute cosine similarity between query subwords and emb_idx["vectors"]
```
