# LEGO Set Clustering Analysis

Unsupervised clustering analysis of 18,600+ LEGO sets to uncover patterns in how LEGO's product lineup has evolved by release year and part count.

## Dataset

Data sourced from [Rebrickable](https://rebrickable.com/downloads/), covering LEGO sets released between 1949 and 2021 (2022 excluded — incomplete year at time of data pull). Two files are used:

- `sets.csv` — set number, name, year, theme, part count
- `themes.csv` — theme names and hierarchy

## Approach

1. **Data cleaning** — merged sets and themes datasets, dropped redundant/unused columns, checked for null values, and removed the incomplete 2022 year.
2. **Feature scaling** — applied `StandardScaler` to `year` and `num_parts` so both features contribute equally to distance calculations (raw `num_parts` ranges into the thousands while `year` spans only ~70 years, which would otherwise dominate clustering).
3. **Choosing k** — used the elbow method, plotting inertia (within-cluster sum of squares) across k = 1 to 10, to justify **k = 6**.
4. **Clustering** — fit K-Means (k=6) on the scaled features, then inverse-transformed the resulting cluster centers back into real years and part counts for interpretation.
5. **Visualization** — scatter plot of all sets colored by cluster assignment, with cluster centers overlaid.

## Key Finding

The six clusters don't primarily split by era, despite year being one of the two input features. Instead, they mostly separate by **set scale**: small sets, mid-size sets, large sets, and mega/collector sets (5,000+ parts) all cluster around the *same* recent years (2012–2017). This suggests LEGO's growth has come from expanding across a broader range of product tiers released concurrently, rather than sets simply growing larger over time.

One older cluster (~1977) stands out as small-parts-only — though it's worth noting ~13% of the dataset predates 1990, spanning 40+ years of history, so this cluster likely blends distinct earlier eras that the model can't separate with limited data density.

## Tech Stack

- Python
- pandas
- scikit-learn (`StandardScaler`, `KMeans`)
- matplotlib

## Repo Structure

```
├── data/
│   ├── sets.csv
│   └── themes.csv
├── Lego_Analysis.ipynb
├── Lego_Analysis_Presentation.pptx
└── README.md
```

## Running Locally

```bash
pip install pandas scikit-learn matplotlib
jupyter notebook Lego_Analysis.ipynb
```
