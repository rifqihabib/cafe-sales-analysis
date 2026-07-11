# Cafe Sales Data Analysis Project

[🇮🇩 Baca versi Bahasa Indonesia](README.id.md)

## Overview
This project demonstrates a complete, end-to-end data analysis workflow — from raw, messy transactional data to actionable business recommendations — using Python and Pandas. It covers data cleaning, exploratory data analysis, feature engineering, and executive reporting on a cafe sales dataset.

## Dataset
- **Source**: [Cafe Sales — Dirty Data for Cleaning Training](https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training) (Kaggle)
- **Final analyzed size**: 9,035 transactions × 15 columns (after cleaning and feature engineering)
- **Description**: Synthetic cafe transaction data, originally containing missing values, disguised placeholder values (`"UNKNOWN"`, `"ERROR"`), inconsistent formatting, and logical inconsistencies between fields.

## Project Workflow
1. **Data Cleaning** (`01_data_cleaning.ipynb`) — detected disguised missing values, recovered missing `Item` values via unique price-to-item mapping, resolved missing numerical fields through mathematical cross-derivation (`Quantity`, `Price Per Unit`, `Total Spent`), standardized text formatting, and corrected data types
2. **Exploratory Data Analysis** (`02_eda_visualization.ipynb`) — visualized sales patterns using three complementary item-performance metrics (frequency, quantity, revenue), payment method distribution, and time-based trends
3. **Feature Engineering** (`03_feature_engineering.ipynb`) — derived new fields (`Day of Week`, `Is Weekend`, `Spending Category`, `Effective Unit Price`) to support deeper analysis
4. **Insight & Reporting** (`04_insight_reporting.ipynb`) — synthesized findings into an executive report with data-driven recommendations

## Tools & Libraries
- Python 3, Pandas, NumPy
- Matplotlib, Seaborn
- Google Colab / Jupyter Notebook

## Key Metrics
| Metric | Value |
|---|---|
| Total Transactions | 9,035 |
| Total Revenue | 79,816.00 |
| Average Transaction Value | 8.83 |
| Top Item (by frequency) | Coffee (1,239 transactions) |
| Top Item (by quantity sold) | Coffee (3,759 units) |
| Top Item (by revenue) | Salad (18,190.00) |
| Dominant Payment Method | Digital Wallet (54.7%) |
| Dominant Order Type | Takeaway (69.9%) |

## Key Findings

### Finding 1: Coffee dominates popularity, but Salad dominates profitability
Coffee leads in both transaction frequency (1,239 transactions) and sales volume (3,759 units), but **Salad generates the highest total revenue** (18,190.00) despite lower frequency (1,212 transactions) — driven by a higher price per unit. Salad's revenue per transaction (15.01) is roughly **2.5× higher** than Coffee's (6.07). Cookie shows the lowest revenue-per-transaction (2.99) among top items. Coffee and Cookie act as high-frequency **traffic drivers**, while Salad is the primary **profitability driver**.

### Finding 2: Medium-value transactions contribute more than half of revenue, despite being a minority by count
| Category | % Transactions | % Revenue | Revenue:Transaction Ratio |
|---|---|---|---|
| Low (< 10) | 62.36% | 34.43% | 0.55× |
| Medium (10–25) | 34.79% | **57.48%** | **1.65×** |
| High (≥ 25) | 2.86% | 8.08% | 2.82× |

Low-value transactions dominate by count but under-contribute to revenue relative to their share. Medium-value transactions are the most efficient segment in aggregate — 34.79% of transactions generating 57.48% of revenue.

**Methodology note**: The "High" threshold (≥25) coincides with the dataset's maximum possible transaction value (5 units × the highest-priced item, Salad at 5.0 per unit). It therefore represents transactions at the ceiling of the price×quantity structure, not a broad range of high spenders.

### Finding 3: Takeaway preference is consistent across all payment methods — no correlation
Takeaway represents 69.25%–70.13% of transactions across Cash, Credit Card, and Digital Wallet alike — a spread of less than 1 percentage point. Takeaway preference is a broad customer behavior independent of payment method choice.

### Finding 4: No strong weekly pattern — demand is relatively stable throughout the week
Daily revenue ranges from 10,790.50 (Wednesday, lowest) to 11,701.00 (Thursday, highest) — roughly an 8.4% spread. Average transaction value is nearly identical between weekdays (8.833) and weekends (8.837), a difference of less than 0.05%.

## Synthesis
The four findings point to a common thread: **revenue growth opportunity lies in increasing value per transaction (upsell), not in increasing visit frequency or targeting specific times or payment methods.** Visit frequency is already high and stable, and payment/order-type patterns show no exploitable correlation. The most realistic lever is shifting transaction composition — bundling high-frequency, low-margin items (Coffee, Cookie) with higher-value items (Salad), and encouraging Low-tier transactions to move into the more efficient Medium tier.

## Recommendations
1. **Bundle Salad (or other high-margin items) with high-frequency, low-revenue items like Coffee and Cookie** to increase average transaction value without sacrificing existing visit frequency.
2. **Design small upsell incentives targeting Low-tier transactions** (62.36% of all transactions, e.g., "add an item for a small discount") to shift them into the Medium tier, which is proportionally the most revenue-efficient segment.
3. **Maintain uniform Takeaway-focused operational investment** (packaging, pickup speed) across all payment methods, since preference for Takeaway does not vary by how customers pay.
4. **Avoid concentrating promotional budget on weekends or specific days**; since demand is stable throughout the week, budget is better allocated toward upsell strategies (Recommendations 1–2) than time-based targeting.

## Data Quality & Methodology Notes
- Missing `Item` values were recovered via price-to-item mapping, applied only where item price was unique (unambiguous); items sharing a price (Cake/Juice at 3.0, Sandwich/Smoothie at 4.0) were intentionally excluded from this recovery to avoid incorrect guesses.
- Missing `Quantity`, `Price Per Unit`, and `Total Spent` values were recovered through mathematical cross-derivation from the other two fields where possible, rather than statistical imputation (median), preserving greater accuracy.
- Duplicate detection intentionally includes `Transaction ID`; transactions with identical attributes but different IDs are treated as legitimate, independent transactions rather than data errors, given the dataset's low categorical cardinality (8 items, 3 payment methods, 2 locations, day-level date granularity) makes coincidental matches statistically expected.
- The `Hour` and `Time of Day` features were excluded from analysis, as the source `Transaction Date` field contains date-only information without a time component.

## Project Structure
```
cafe-sales-analysis/
├── README.md
├── README.id.md
├── data/
│   ├── raw/
│   │   └── dirty_cafe_sales.csv
│   └── cleaned/
│       ├── cafe_sales_cleaned_final.csv
│       └── cafe_sales_featured.csv
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_eda_visualization.ipynb
│   ├── 03_feature_engineering.ipynb
│   └── 04_insight_reporting.ipynb
├── images/
│   └── summary_dashboard.png
├── reports/
│   └── executive_report.md
└── requirements.txt
```

## How to Reproduce
```bash
git clone https://github.com/your-username/cafe-sales-analysis.git
cd cafe-sales-analysis
pip install -r requirements.txt
jupyter notebook notebooks/01_data_cleaning.ipynb
```

## Lessons Learned
- Standard `.isnull()` checks are insufficient on their own — categorical columns must be inspected individually for disguised placeholder values.
- When a field can be mathematically derived from other clean fields (e.g., `Total Spent = Quantity × Price Per Unit`), recalculation is more accurate than statistical imputation.
- Duplicate detection logic should be chosen deliberately based on dataset context — including a unique identifier column can be the correct choice when coincidental attribute matches are plausible, not a bug to "fix" by default.
- The item purchased most frequently is not always the most valuable one — revenue-per-item analysis often reveals different priorities than transaction-count analysis alone.
- A segmentation threshold that coincides with a dataset's structural maximum should be documented explicitly, so readers don't misinterpret its meaning.

## Author
*Muchammad Rifqi Habib*
*(https://www.linkedin.com/in/muchammad-rifqi-habib-a450b22a6/)*
