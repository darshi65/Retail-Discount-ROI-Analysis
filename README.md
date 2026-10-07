# Retail Discount ROI & Cannibalization Analysis

Analysis of whether discounting is growing profitable volume or simply eroding margin on sales that would have happened anyway — using the Superstore retail dataset, Python for cleaning/EDA, and Power BI for an interactive dashboard.

---

## Business Problem

> Our discount strategy has been applied inconsistently across categories and regions. Leadership wants to know: is discounting actually growing profitable volume, or is it just eroding margin on sales that would have happened anyway? Which categories/sub-categories should we discount more, less, or not at all?

---

## Dataset

- **Source:** [Superstore Sales Dataset (Kaggle)](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)
- ~9,800 order line items across 2014–2017, covering Sales, Profit, Discount, Quantity, Region, Category, Sub-Category, Customer Segment, and Ship Mode.

## Tools Used

| Tool | Purpose |
|---|---|
| Python (Pandas, Jupyter) | Data cleaning, feature engineering, exploratory analysis |
| Power BI Desktop | Data modeling (DAX), interactive dashboard |
| GitHub | Version control, project documentation |

## Methodology

1. **Cleaning** — checked for duplicates, nulls, and invalid values (0 duplicates, 0 nulls, no negative/zero sales rows found); confirmed data was clean enough to proceed without row removal.
2. **Feature engineering** — created `Profit Margin`, `Discount Bucket`, `Is Unprofitable` flag, and date fields.
3. **Exploratory analysis** — tested the discount-margin relationship at the bucket and exact-discount level, checked sub-category and regional breakdowns, and tested the discount-vs-order-volume correlation.
4. **Dashboard** — built a star-schema-style model in Power BI with a dedicated `Dim_Date` table and DAX measures using **dollar-weighted margin** (`Total Profit / Total Sales`) rather than a simple average of per-order margins, to avoid the distortion that occurs when small orders skew an unweighted average.
5. **Recommendation** — quantified the dollar impact of capping discounts above the identified breakeven point.

## Key Findings

1. **A healthy-looking 12.47% overall profit margin hides a major problem: 26.31% of all orders (roughly 1 in 4) lose money.** Averages at the aggregate level mask this — it only becomes visible when orders are segmented by discount level.
2. **Profit margin turns negative once discount exceeds ~20%, and the effect is sharp, not gradual.** The unprofitable-order rate jumps from under 10% (at 0–20% discount) to ~90–100% at 21%+ discount — almost every order discounted beyond this point loses money.
3. **The 31%+ discount bucket alone accounts for the majority of total losses** (-$125K of roughly -$135K in combined losses from the two highest discount buckets), despite representing a relatively small share of total order volume.
4. **Furniture (specifically Tables and Bookcases) is the clearest "stop discounting" case** — both sub-categories combine above-average discounting with negative total profit. Furniture as a category shows a visible profit and margin collapse compared to Technology and Office Supplies.
5. **Central region discounts more heavily than other regions and shows a corresponding dip in profit margin** — directly linking a regional discount policy to a regional profitability problem.
6. **Discounting isn't uniformly bad — it depends on the product.** Binders are discounted the most of any sub-category (37.2%) and remain solidly profitable ($30.2K profit), showing some products can absorb heavy discounting while others (Tables, Bookcases) cannot.
7. **Estimated recoverable profit: ~$138,500** — the approximate amount lost specifically to orders discounted above the ~20% breakeven threshold. This is the figure the capping recommendation is built on.

## Recommendation

Cap discounts at approximately 20% for margin-sensitive sub-categories (particularly Furniture — Tables and Bookcases), review Central region's discount governance against other regions, and preserve flexibility for sub-categories like Binders and Accessories that remain profitable even at high discount levels. Estimated profit recovery: **~$138,500**.

## Dashboard

<img width="1318" height="756" alt="Executive summary" src="https://github.com/user-attachments/assets/16a53033-5401-4527-b0fc-31737b529fc9" />

([images/Discount-Deep-Dive.png])


## Repository Structure

```
retail-discount-roi-analysis/
├── README.md
├── data/
│   ├── raw/                  (original dataset)
│   └── cleaned/               (cleaned CSV output from the notebook)
├── notebooks/
│   └── discountroi_cleaning.ipynb
├── dashboard/
│   └── discount_roi_dashboard.pbix
├── images/
│   ├── dashboard_page1.png
│   └── dashboard_page2.png
└── insights.md                (detailed finding write-ups)
```

## Limitations & Next Steps

- Findings are based on historical correlation, not a controlled experiment — an actual A/B test of a discount cap would be needed to confirm causation before implementing the recommendation company-wide.
- The discount-vs-order-volume relationship was found to be weak (r ≈ 0.22), suggesting discounting isn't meaningfully driving extra demand in this dataset, but this deserves a dedicated experiment rather than relying on historical correlation alone.
- With more time/data, next steps would include: testing an actual discount cap on a subset of stores/regions, incorporating shipping/return costs into true contribution margin, and extending the regional analysis with store-level or population-normalized benchmarks.
