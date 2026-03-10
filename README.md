<h1 align="center">🍷 Dimensionality Reduction with PCA and Wine Clustering using K-Means</h1>

> With 13 chemical features describing each wine, visualizing clusters directly is impossible. This project explores what happens when you cluster on the full original data vs. clustering on PCA-reduced data — and the results are more nuanced than expected.

---

# 📌 Problem

The wine dataset has 13 chemical properties per sample and 3 known wine types. The goal is to use **unsupervised clustering** to group wines by similarity — without telling the model the actual types — and then evaluate how well it recovers those natural groups.

The core experiment: does reducing 13 features to 2 principal components help or hurt clustering? This project runs both and compares the results head-to-head.

---

# 📂 Dataset

| Property | Details |
|----------|---------|
| File | `wine.csv` |
| Records | 178 wine samples |
| Features | 13 chemical properties |
| Target | `Type` (1, 2, 3) — used only for evaluation, not training |
| Missing Values | None |

**Features:**

| Feature | Description |
|---------|-------------|
| `Alcohol` | Alcohol content |
| `Malic` | Malic acid |
| `Ash` | Ash content |
| `Alcalinity` | Alkalinity of ash |
| `Magnesium` | Magnesium level |
| `Phenols` | Total phenols |
| `Flavanoids` | Flavanoid content |
| `Nonflavanoids` | Non-flavanoid phenols |
| `Proanthocyanins` | Proanthocyanins |
| `Color` | Color intensity |
| `Hue` | Hue |
| `Dilution` | OD280/OD315 of diluted wines |
| `Proline` | Proline level |

**Type Distribution:**

| Type | Count |
|------|-------|
| 2 | 71 |
| 1 | 59 |
| 3 | 48 |

---

# 🔍 Approach

**Preprocessing:**
1. Dropped `Type` before feature analysis and clustering (used later for evaluation only)
2. **StandardScaler** applied before PCA — critical because PCA is variance-based and larger-scale features like `Proline` (hundreds) would dominate over `Ash` (single digits) without scaling
3. Note: `StandardScaler` chosen over `MinMaxScaler` intentionally — MinMaxScaler compresses variance range, which would cause PCA to lose information

**PCA Analysis:**
4. Ran PCA on all 13 features to get Explained Variance Ratio (EVR) per component
5. Plotted both individual EVR and cumulative EVR to determine optimal components
6. PC1 captures **36% variance**, PC2 captures **19%** — 2 components = ~55% total

**Two Clustering Experiments:**

| Task | Data Used | Purpose |
|------|-----------|---------|
| Task 3 | All 13 original scaled features | Full information clustering |
| Task 4 | 2 PCA components | Reduced dimension clustering |

- K-Means with `n_clusters=3` (matches known wine types) applied in both tasks
- Evaluated using **Silhouette Score** (no labels needed) and **Adjusted Rand Score** (requires true labels)

---

# 🤖 Model / Algorithm

| Component | Config |
|-----------|--------|
| PCA | `n_components=2` for clustering; full PCA first for EVR analysis |
| K-Means | `n_clusters=3`, `random_state=42` |
| Scaler | `StandardScaler` — mean=0, std=1 |

**Evaluation Metrics:**

| Metric | What it measures | Needs true labels? |
|--------|-----------------|-------------------|
| Silhouette Score | Cluster shape — dense and separated vs overlapping | No |
| Adjusted Rand Score | Accuracy of cluster assignments vs actual types | Yes |

---

# 📊 Results

| Metric | Task 3 (13 features) | Task 4 (2 PCA components) |
|--------|---------------------|--------------------------|
| Adjusted Rand Score | **0.897** ✅ Excellent | 0.352 ❌ Poor |
| Silhouette Score | 0.194 (modest) | **0.560** ✅ Good |

This is the interesting part — the two metrics tell opposite stories.

**Adjusted Rand Score** (accuracy of assignments) is far better on the original 13 features — the model correctly recovered wine types 90% of the time with full data. PCA dropped this to 35% because dimensionality reduction lost information that was critical for distinguishing wine types.

**Silhouette Score** (cluster shape quality) is better on PCA data — not because PCA clustering is actually better, but because reducing to 2 dimensions forces clusters to look more compact and separated geometrically. It's a bit misleading here.

**Key takeaway:** When the goal is accuracy, cluster on the full data. PCA is valuable for visualization and speed, but it comes at a real cost to clustering accuracy on this dataset.

---

# ▶️ How to Run

```bash
git clone https://github.com/Chaithanya449/Dimensionality-reduction-with-PCA-and-clustering-wines-using-K-Means..git
cd Dimensionality-reduction-with-PCA-and-clustering-wines-using-K-Means.
pip install -r requirements.txt
python dimensionality_reduction_with_pca_and_clustering_wines_using_k_means_.py
```

> Ensure `wine.csv` is in the same directory as the script.

---

# 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-lightgrey?logo=pandas)
![NumPy](https://img.shields.io/badge/NumPy-Numerical-blue?logo=numpy)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-PCA%20%7C%20KMeans%20%7C%20Metrics-orange?logo=scikit-learn)
![Seaborn](https://img.shields.io/badge/Seaborn-Visualization-9cf)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Plots-yellow)

---

# 📁 Project Structure

```
Dimensionality-reduction-with-PCA-and-clustering-wines-using-K-Means./
├── dimensionality_reduction_with_pca_and_clustering_wines_using_k_means_.py
├── wine.csv          # Dataset (178 samples, 14 columns)
└── README.md
```

---

# 🔮 Next Steps

The Silhouette Score vs Adjusted Rand Score contradiction is worth digging deeper into. Want to try clustering at different PCA component counts — 3, 4, 5 components — and track how both scores change. There's probably a sweet spot somewhere between 2 and 13 where accuracy doesn't drop as steeply.

Also want to try the elbow curve here instead of hardcoding `k=3`. Even though we know there are 3 wine types, it would be interesting to see if the elbow naturally lands at 3 or suggests something different.

Finally, feature importance from PCA loadings is something I skipped — want to go back and check which of the 13 original features contribute most to PC1 and PC2. That would tell us which chemical properties are most responsible for differentiating wine types.

---

# 👤 Author

**Chaithanya Krishna** · [LinkedIn](https://www.linkedin.com/in/chaitanyakrishna-profile) · [GitHub](https://github.com/Chaithanya449)
