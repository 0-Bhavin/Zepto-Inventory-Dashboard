# Zepto Inventory Dashboard

A data analytics project that cleans, explores, and analyzes a real-world **Zepto** (quick-commerce grocery delivery) product catalog using **SQL**, and presents the findings in an interactive **Power BI** dashboard.

![Power BI Dashboard](Power%20BI%20Dashboard/Dashboard_image.png)

## Overview

This project works through the full lifecycle of an inventory analytics task:

1. **Explore** the raw product catalog to understand its structure and spot data-quality issues.
2. **Clean** the data — remove invalid records and fix unit/currency errors.
3. **Analyze** the cleaned data with SQL to answer business questions about pricing, discounts, stock, and inventory weight.
4. **Visualize** the results in a Power BI dashboard for at-a-glance inventory insights.

## Dataset

The raw data (`data/zepto_v2.csv`) is a snapshot of Zepto's product catalog, with **3,732 products** across **14 categories**:

`Fruits & Vegetables`, `Dairy, Bread & Batter`, `Munchies`, `Packaged Food`, `Beverages`, `Personal Care`, `Home & Cleaning`, `Health & Hygiene`, `Biscuits`, `Cooking Essentials`, `Meats, Fish & Eggs`, `Chocolates & Candies`, `Ice Cream & Desserts`, `Paan Corner`

**Columns:**

| Column | Description |
|---|---|
| `Category` | Product category |
| `name` | Product name |
| `mrp` | Maximum Retail Price (in paise in the raw file, converted to rupees during cleaning) |
| `discountPercent` | Discount percentage applied to the MRP |
| `availableQuantity` | Units currently available in inventory |
| `discountedSellingPrice` | Final selling price after discount |
| `weightInGms` | Product weight in grams |
| `outOfStock` | Whether the product is currently out of stock |
| `quantity` | Order quantity unit |

`data/cleaned_zepto_v2.csv` is the output of the cleaning step, with a `sku_id` added as a unique identifier per row.

## Project Structure

```
Zepto-Inventory-Dashboard-main/
├── data/
│   ├── zepto_v2.csv              # Raw product catalog
│   └── cleaned_zepto_v2.csv      # Cleaned dataset (with sku_id)
├── sql/
│   ├── data_exploration.sql      # Initial exploration & data-quality checks
│   ├── data_cleaning.sql         # Cleaning steps (remove $0 items, fix currency units)
│   └── data_analysis.sql         # Business-question queries (Q1–Q8)
└── Power BI Dashboard/
    ├── zepto-inventory-dashboard.pbix   # Power BI report file
    └── Dashboard_image.png              # Dashboard screenshot
```

## SQL Workflow

### 1. Data Exploration (`sql/data_exploration.sql`)
- Row counts and sample records
- Checks for `NULL` values across all key columns
- Lists distinct product categories
- Breaks down in-stock vs. out-of-stock counts
- Finds products with multiple SKUs (duplicate names)

### 2. Data Cleaning (`sql/data_cleaning.sql`)
- Identifies and removes products with `mrp = 0` (invalid/placeholder records)
- Converts `mrp` and `discountedSellingPrice` from paise to rupees (divides by 100)

### 3. Data Analysis (`sql/data_analysis.sql`)
Answers key inventory and pricing questions, including:

- **Q1:** Top 10 best-value products by discount percentage
- **Q2:** High-MRP products (> ₹300) that are out of stock
- **Q3:** Estimated revenue per category (`discountedSellingPrice × availableQuantity`)
- **Q4:** Products with MRP > ₹500 but discount < 10% (low-discount, high-value items)
- **Q5:** Top 5 categories by average discount percentage
- **Q6:** Price-per-gram for products over 100g, sorted by best value
- **Q7:** Products bucketed by weight into Low / Medium / Bulk tiers
- **Q8:** Total inventory weight per category

## Power BI Dashboard

The `.pbix` file in `Power BI Dashboard/` visualizes the cleaned dataset and the insights from the SQL analysis, covering:

- Category-wise revenue and inventory weight
- Stock availability (in-stock vs. out-of-stock) breakdown
- Discount distribution across categories
- Best-value and high-MRP/out-of-stock product call-outs

Open `zepto-inventory-dashboard.pbix` in Power BI Desktop to explore the full interactive report. A static preview is shown above.

## Tools Used

- **SQL** (SQL Server syntax — e.g. `TOP`, `dbo` schema) for data exploration, cleaning, and analysis
- **Power BI** for dashboard visualization
- **CSV** as the raw and cleaned data format

## How to Reproduce

1. Load `data/zepto_v2.csv` into a SQL Server database as a table named `zepto` (or `zepto_v2`, per the analysis script).
2. Run `sql/data_exploration.sql` to inspect the raw data.
3. Run `sql/data_cleaning.sql` to remove invalid rows and convert currency units.
4. Run `sql/data_analysis.sql` to generate the analytical query results.
5. Open `Power BI Dashboard/zepto-inventory-dashboard.pbix` in Power BI Desktop, point it at the cleaned data/table, and refresh to regenerate the visuals.
