# 🏋️ Fitness Profiling Using K-Means Clustering

Segmenting gym members into data-driven fitness profiles using unsupervised machine learning — turning raw workout and health metrics into actionable member segments a fitness business can act on.

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-KMeans-F7931E?logo=scikitlearn&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Processing-150458?logo=pandas&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## 📌 Table of Contents
- [Business Problem](#-business-problem)
- [Results at a Glance](#-results-at-a-glance)
- [Dataset](#-dataset)
- [Methodology](#-methodology)
- [Visual Results](#-visual-results)
- [Key Insights](#-key-insights)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [How to Run](#-how-to-run)
- [Future Enhancements](#-future-enhancements)
- [About the Author](#-about-the-author)

---

## 🎯 Business Problem

A fitness center wants to move away from a one-size-fits-all approach and instead understand its members as distinct groups based on **how they train** (heart rate, session duration, calories burned) and **their physical health indicators** (body fat %, BMI). This project builds an unsupervised **K-Means Clustering** model that automatically discovers these groups directly from the data — no labels required — so the business can design targeted training programs, retention strategies, and equipment/class planning around real member segments.

## 📊 Results at a Glance

| Metric | Value |
|---|---|
| Members analyzed | 973 |
| Features used for clustering | 6 (Avg BPM, Resting BPM, Session Duration, Calories Burned, Fat %, BMI) |
| Missing values handled | 63 (35 in Calories Burned, 28 in Fat %) |
| Clustering method | K-Means (scikit-learn) |
| Evaluation method | Elbow Method (WCSS) + Silhouette Score, K = 2–9 |
| Final number of clusters (K) | 5 |
| Final Silhouette Score | 0.206 |

## 🗂 Dataset

The dataset contains **973 gym members**, each described by 6 numeric features:

| Feature | Description |
|---|---|
| `Avg_BPM` | Average heart rate during a workout session |
| `Resting_BPM` | Resting heart rate |
| `Session_Duration (hours)` | Length of a workout session |
| `Calories_Burned` | Calories burned per session |
| `Fat_Percentage` | Body fat percentage |
| `BMI` | Body Mass Index |

Two columns had missing values — `Calories_Burned` (35 rows) and `Fat_Percentage` (28 rows). Skewness was checked for each before imputation (0.28 and -0.63 respectively) to decide on an appropriate fill strategy.

## 🧠 Methodology

1. **Handle Missing Values** — Checked distribution skew per column, then imputed `Calories_Burned` and `Fat_Percentage` using mean imputation to produce a fully clean dataset.
2. **Scale Features** — Applied `StandardScaler` to standardize all 6 features to zero mean / unit variance, so that high-magnitude features (like `Calories_Burned`, in the hundreds) don't dominate distance-based clustering over low-magnitude features (like `Fat_Percentage`).
3. **Find Optimal K** — Computed **WCSS (inertia)** for K = 1–10 to apply the Elbow Method, and cross-validated with **Silhouette Scores** across K = 2–9 to sanity-check cluster cohesion and separation.
4. **Build Final Model** — Trained the final `KMeans` model with **K = 5** (chosen from the elbow point, where the reduction in WCSS clearly flattens) and assigned each member a cluster label.
5. **Generate Submission** — Stored final predicted cluster labels in `submission_df` with a single `cluster` column, ready for evaluation or downstream business use.

## 📈 Visual Results

**Elbow Method — choosing K:** WCSS drops sharply through K = 2–5 and then flattens, indicating diminishing returns from adding more clusters beyond K = 5.

![Elbow Method](assets/elbow_method.png)

**Silhouette Score across K = 2–9:** Used as a secondary validation signal alongside the elbow curve to confirm cluster quality was stable and well-behaved around the selected K.

![Silhouette Scores](assets/silhouette_scores.png)

**Initial 2-cluster split** (before optimal K was selected) visualized on Avg BPM vs. Resting BPM:

![Initial Clustering](assets/initial_clustering_scatter.png)

## 💡 Key Insights

- Gym members are **not a single homogeneous population** — 5 statistically distinct fitness profiles emerge from combining cardiovascular effort (BPM), training volume (session duration, calories burned), and body composition (Fat %, BMI).
- The elbow curve shows most of the "explanatory value" of clustering is captured by K = 5 — segmenting further into more groups yields rapidly diminishing separation.
- This segmentation gives a fitness center a practical, data-backed way to differentiate programs (e.g. high-intensity/lower body-fat members vs. beginner or recovery-focused members) instead of treating every member the same.

## 🛠 Tech Stack

- **Python** — core language
- **pandas / NumPy** — data loading, cleaning, and manipulation
- **scikit-learn** — `StandardScaler`, `KMeans`, `silhouette_score`
- **Matplotlib** — Elbow curve, Silhouette score plot, cluster scatter visualization
- **Jupyter Notebook** — development environment

## 📁 Project Structure

```
fitness-profiling-kmeans/
├── fitness_profile_using_kmeans.ipynb   # Full analysis: EDA → scaling → clustering → evaluation
├── README.md
└── assets/
    ├── elbow_method.png
    ├── silhouette_scores.png
    └── initial_clustering_scatter.png
```

## ▶️ How to Run

```bash
# 1. Clone the repository
git clone https://github.com/vishnusai2005/fitness-profiling-kmeans.git
cd fitness-profiling-kmeans

# 2. Install dependencies
pip install pandas numpy scikit-learn matplotlib jupyter

# 3. Launch the notebook
jupyter notebook fitness_profile_using_kmeans.ipynb
```

The dataset is loaded directly from a hosted CSV URL inside the notebook — no manual download needed.

## 🚀 Future Enhancements

- Select K by directly maximizing the silhouette score (or a weighted elbow + silhouette criterion) rather than relying on the elbow point alone.
- Revisit the imputation strategy for `Fat_Percentage` (skew ≈ -0.63) — median imputation may be more robust to its moderate skew than the mean.
- Add a PCA-based 2D/3D projection to visualize separation across all 6 dimensions, beyond a single 2-feature scatter plot.
- Profile each cluster's centroid values to assign human-readable personas (e.g. "Endurance-Focused", "Beginner/Recovery") for direct use by gym trainers.
- Wrap the trained model in a small Streamlit/Flask app so gym staff can look up a member's profile without touching the notebook.

## 👤 About the Author

**Vydhyam Vishnu Sai**

Exploring  machine learning , Deep Learning and Gen AI opportunities.

🔗 GitHub: [github.com/vishnusai2005](https://github.com/vishnusai2005)

---
