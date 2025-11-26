# 🚀 Product Matcher GPU

A high-performance AI tool for semantic product matching (mapping competitor products to your internal catalog). Optimized for consumer GPUs (e.g., RTX 3050 4GB) but fully compatible with CPU.

## ⚡ Key Features

* **Two-Stage Architecture:**
    1.  **Retrieval:** Fast candidate search using vector embeddings (`BAAI/bge-m3` + FAISS).
    2.  **Reranking:** Precision ranking using a Cross-Encoder (`BAAI/bge-reranker-v2-m3`).
* **Optimized:** Uses FP16 precision to minimize VRAM usage.
* **Smart Caching:** Indexes your product catalog once to speed up future runs.

## 🛠 Installation

1.  **Prerequisites:** Python 3.8+.
2.  **Install Dependencies:**

    ```bash
    pip install requirements.txt
    ```

    *(Optional: Install `faiss-gpu` for faster indexing if you have a compatible environment, otherwise `faiss-cpu` works fine).*

## 📂 Data Preparation

Place two CSV files in the project root directory:

1.  `competitor_products.csv` — List of competitor products.
2.  `our_products.csv` — List of your internal products.

**Format:** Standard CSV files. Ensure both files have a column containing the product names (e.g., `name` or `title`).

## ⚙️ Configuration & Usage

1.  Open `main.py` and locate the `if __name__ == "__main__":` block at the bottom.
2.  Update the column names to match your CSV files:

    ```python
    COMPETITOR_COL_NAME = 'name'  # Column header in competitor_products.csv
    OUR_COL_NAME = 'name'         # Column header in our_products.csv
    ```

3.  Run the script:

    ```bash
    python main.py
    ```

## 📊 Output

The script generates `results_gpu_matched.csv` containing:

* `competitor_product`: Name of competitor product.
* `our_product`: Name of our product.
* `rerank_score`: Confidence score (0.0 to 1.0). Higher is better.
* `match_status`: **matched** (high confidence) or **low_confidence**.

---

### 💡 Note
On the first run, the script will download the necessary AI models (~3GB). This may take a few minutes. Subsequent runs will be much faster.