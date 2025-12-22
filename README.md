# Product Matcher v4

Match competitor products to your catalog using neural networks.

## Algorithm

```
Competitor Products              Your Catalog
       ↓                              ↓
   [BGE-M3]                       [BGE-M3]
   Encoder                         Encoder
       ↓                              ↓
   Embeddings ──── FAISS ──── Embeddings
                    ↓
              Top-K candidates
                    ↓
            [BGE-Reranker-v2-m3]
                    ↓
               Best match
```

**Stage 1 — Retrieval:** BGE-M3 creates embeddings, FAISS finds Top-K similar products.

**Stage 2 — Reranking:** Cross-encoder reranks candidates and selects the best match.

## Quick Start

```python
from product_matcher import quick_match

results = quick_match(
    competitor_csv='competitor.csv',
    our_csv='our_products.csv',
    output_csv='matches.csv'
)
```

## Notebook Usage

### 1. Configure settings (cell 5)

```python
COMPETITOR_FILE = "competitor_products.csv"
OUR_FILE = "our_products.csv"
COMPETITOR_COL_NAME = 'name'  # product name column
OUR_COL_NAME = 'name'
```

### 2. Run cells in order

```
1️⃣ Imports       — load libraries
2️⃣ GPU Check     — verify GPU availability
3️⃣ Core Class    — define matcher class
5️⃣ Config        — your settings
6️⃣ Load Data     — load CSV files
7️⃣ Initialize    — create matcher
8️⃣ Run Matching  — execute matching
9️⃣ Results       — analyze and save
```

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `threshold` | 0.5 | Confidence threshold (0-1) |
| `top_k` | 5 | Candidates for reranking |
| `encoder_batch` | 8 | Encoder batch size (4-8 for 4GB VRAM) |
| `reranker_batch` | 16 | Reranker batch size |
| `use_fp16` | True | FP16 precision to save memory |

## Output Format

| Column | Description |
|--------|-------------|
| `competitor_product` | Competitor product name |
| `our_product` | Matched product |
| `rerank_score` | Confidence score (0-1) |
| `match_status` | `matched` / `low_confidence` |

## Requirements

- Python 3.10+
- PyTorch with CUDA
- sentence-transformers
- faiss-cpu (or faiss-gpu)
- pandas, numpy, tqdm

```bash
pip install -r requirements.txt
```

## GPU

Optimized for GPUs with 4GB VRAM (RTX 3050 and similar). Works on CPU too, but slower.
