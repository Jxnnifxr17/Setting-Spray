# Ulta Setting Spray: Pricing, Value & Market Segmentation

An analysis of 156 Ulta setting spray listings (59 brands, 2026 snapshot) that asks where the value is and what shoppers are paying for. It uses Python for the statistics and clustering, and Tableau for the dashboard.


## Key findings
- Small bottles carry a real premium. Median Small is about $14.41/oz versus about $8.81/oz for Large (roughly 1.6x), significant at p < 0.001 (Mann-Whitney). Small costs more per ounce than Large at 20 of the 22 brands that sell both.  
- Finish does not drive price. Dewy, Matte, Natural, Hydrate, and Radiant show no significant difference (Kruskal-Wallis p = 0.70), including after controlling for size tier.  
- Brand drives price. Budget brands (Essence, L.A. Girl, Revolution, ULTA Collection, e.l.f.) are roughly 3-4x cheaper per ounce than prestige brands (MAC, Charlotte Tilbury, Anastasia Beverly Hills, Tarte, Milk Makeup).  
- Four market segments (k-means, k = 4):  

| Segment | Listings | Median price | Median $/oz |
| :--- | :---: | :---: | :---: |
| **Value** | 41 | ~$10 | ~$3.85 |
| **Everyday** | 34 | ~$18 | ~$5.92 |
| **Premium** | 48 | ~$36 | ~$10.61 |
| **Small / mini sizes** | 33 | ~$18 | ~$17.78 |


The Small / mini cluster is defined by bottle size (median ~1.15 oz), not brand tier, so it reflects the small-size premium.

## Data
One row per product-size listing, collected from Ulta in 2026.

| Column | Description |
| :--- | :--- |
| **Brand, Name** | Brand and product name |
| **Price, Size** | Price in USD and size in ounces |
| **\$/OZ** | Price per ounce |
| **Finish Type** | Dewy, Matte, Natural, Hydrate, or Radiant |
| **Size type** | Small, Medium, or Large (see rule below) |
| **Link** | Product URL |


Size tier rule: one size available is Medium; three sizes are Small, Medium, and Large; two sizes are Small and Large; a mini is Small.  

Cleaning: removed 4 exact duplicate rows (160 to 156 listings), trimmed whitespace, standardized category labels, and confirmed price ÷ size matches $/oz on every row with no missing values.  

## Methods
- EDA: distributions of price, size, and $/oz; Spearman correlations.  
- Tests: Kruskal-Wallis across size tiers and finishes; one-sided Mann-Whitney for Small vs. Large. Non-parametric because $/oz is right-skewed.  
- Regression: OLS on log $/oz with finish and size tier.  
- Brand tiers: brands with 3+ listings split into terciles of median $/oz (cutoffs ~$4.07 and ~$8.50).  
- Clustering: k-means on standardized log(price) and log($/oz), k = 2 to 7. Price, size, and $/oz are mathematically linked, so using all three would double-count. Silhouette peaks at k = 2 (0.52), but k = 4 (0.48) is far more interpretable. Hierarchical clustering agrees with k-means (ARI 0.87).  

### Repository contents
| File / Asset | Description |
| :--- | :--- |
| **Ulta_Setting_Spray_2026_-_Sheet1__1_.csv** | Raw dataset |
| **Ulta_Setting_Spray_Analysis.ipynb** | Full analysis notebook with outputs |
| **setting_spray_clean.csv** | Cleaned, de-duplicated data |
| **setting_spray_clustered.csv** | Cleaned data plus cluster and segment labels |
| **setting_spray_tableau_ready.csv** | Clustered data plus precomputed fields for Tableau (Small Tax Ratio, Brand Median PPO, Brand Listings, Brand Tier) |


## Dashboard

The Tableau dashboard has six views: small-size tax by brand, segment scatter (price vs. size, log scales), brand price tiers, price per ounce by size tier, price per ounce by finish, and size mix by segment.

## Limitations. 
One retailer and one snapshot in 2026; promotions and prices change.
Small sample (156 listings, 59 brands); many brands have only 1-3 listings.
Size tiers were assigned by rule, and Medium mixes single-size products with mid-size ladder entries, so Small vs. Large is the cleanest comparison.
No sales, rating, or review data, so "value" means price per ounce, not quality or popularity.

## Next steps
Pair this shelf-level view with the GMI Insights setting spray market report.
Collect a second snapshot to measure price and promotion changes over time.
Add ratings and review counts to test whether higher price buys higher satisfaction.  

## Tools:  
Python (pandas, NumPy, SciPy, statsmodels, scikit-learn, seaborn, matplotlib), Jupyter, Tableau Public.
