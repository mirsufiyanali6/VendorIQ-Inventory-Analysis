# KPI Definitions — VendorIQ

Every metric shown on the Power BI dashboards and referenced in the business conclusion report, defined with its formula and business purpose.

---

## Revenue & Sales

| KPI | Formula | Business Purpose |
|---|---|---|
| **Total Revenue** | `SUM(SalesDollars)` | Total sales value generated across all vendors, products, and stores |
| **Average Selling Price** | `SUM(SalesDollars) / SUM(SalesQuantity)` | Average price customers pay per unit sold |
| **Total Sales Quantity** | `SUM(SalesQuantity)` | Total units sold |
| **Revenue Contribution %** | `Vendor/Brand Revenue ÷ Total Revenue × 100` | Share of total revenue driven by a given vendor, brand, or product |

## Procurement & Purchasing

| KPI | Formula | Business Purpose |
|---|---|---|
| **Total Purchase Cost** | `SUM(Dollars)` from `purchases` | Total amount spent acquiring inventory from vendors |
| **Average Purchase Price** | `SUM(Dollars) / SUM(Quantity)` | Average unit cost paid across all purchase orders |
| **Average Purchase Quantity** | `SUM(Quantity) / COUNT(PONumber)` | Average order size per purchase order, used to test bulk-buying impact on cost |
| **Total Purchase Orders** | `COUNT(DISTINCT PONumber)` | Total number of purchase orders placed in the period |
| **Total Purchase Quantity** | `SUM(Quantity)` | Total units purchased across all vendors |

## Profitability

| KPI | Formula | Business Purpose |
|---|---|---|
| **Gross Profit** | `SalesDollars − (SalesQuantity × PurchasePrice)` | Profit earned per transaction, vendor, or brand after cost of goods |
| **Profit Margin %** | `Gross Profit ÷ SalesDollars × 100` | Profitability as a percentage of revenue — the core benchmark for vendor and brand comparison |
| **Blended Margin** | `Total Gross Profit ÷ Total Revenue × 100` | Company-wide profitability across all vendors combined |

## Inventory

| KPI | Formula | Business Purpose |
|---|---|---|
| **Beginning Inventory Value** | `SUM(onHand × Price)` from `begin_inventory` | Value of stock on hand at the start of the period |
| **Ending Inventory Value** | `SUM(onHand × Price)` from `end_inventory` | Value of stock on hand at the end of the period |
| **Inventory Change** | `Ending Inventory Value − Beginning Inventory Value` | Net growth or reduction in inventory investment over the period |
| **Inventory Turnover** | `Total Sales Quantity ÷ Average Inventory Quantity` | How many times inventory is sold and replaced over the period — a core efficiency measure |
| **High-Value Inventory Outlier** | Product inventory value beyond the statistical outlier threshold (IQR/Z-score based) | Flags products tying up disproportionate working capital relative to the rest of the catalog |

## Vendor Performance

| KPI | Formula | Business Purpose |
|---|---|---|
| **Active Vendors** | `COUNT(DISTINCT VendorNumber)` | Number of vendors with recorded activity in the period |
| **Strategic High-Performing Vendor** | Vendor ranked in the top revenue/margin tier via statistical classification | Identifies vendors warranting priority relationship management |
| **Low-Margin Outlier Vendor** | Vendor profit margin flagged as a statistical outlier on the low end | Identifies vendors requiring pricing or contract review |

## Statistical Benchmarks

| KPI | Formula / Method | Business Purpose |
|---|---|---|
| **Correlation Coefficient (r)** | Pearson correlation between two metrics (e.g. sales vs. profit) | Measures the strength and direction of the relationship between two business variables |
| **p-value** | Statistical significance test output | Confirms whether an observed pattern is statistically meaningful (p < 0.05) or likely due to chance |
| **95% Confidence Interval** | `Sample Mean ± 1.96 × Standard Error` | The range within which the true population value (e.g. average margin) is expected to fall with 95% confidence |
| **Skewness** | Distribution skewness coefficient | Indicates whether a metric (e.g. revenue) is concentrated among a small group (right-skewed) or evenly spread |
