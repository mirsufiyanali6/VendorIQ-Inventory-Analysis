# Data Dictionary — VendorIQ Dataset

Six core tables, representing one year of purchasing, sales, and inventory activity for a multi-store liquor retail business. This document defines every column and lists the data quality issues found (and fixed) during the EDA phase.

---

## 1. `begin_inventory`
Opening stock position by store and product, captured at the start of the reporting period.

**Rows:** 206,529 &nbsp;·&nbsp; **Columns:** 9

| Column | Type | Description |
|---|---|---|
| `InventoryId` | string | Composite key identifying store + product + inventory record |
| `Store` | int | Store number |
| `City` | string | City where the store is located |
| `Brand` | int | Product brand code |
| `Description` | string | Product name |
| `Size` | string | Bottle/pack size (e.g. `750mL`) |
| `onHand` | int | Quantity on hand at the start of the period |
| `Price` | decimal | Retail price per unit |
| `startDate` | date | Date the opening inventory was recorded |

---

## 2. `end_inventory`
Closing stock position by store and product, captured at the end of the reporting period.

**Rows:** 224,489 &nbsp;·&nbsp; **Columns:** 9

| Column | Type | Description |
|---|---|---|
| `InventoryId` | string | Composite key identifying store + product + inventory record |
| `Store` | int | Store number |
| `City` | string | City where the store is located |
| `Brand` | int | Product brand code |
| `Description` | string | Product name |
| `Size` | string | Bottle/pack size |
| `onHand` | int | Quantity on hand at the end of the period |
| `Price` | decimal | Retail price per unit |
| `endDate` | date | Date the closing inventory was recorded |

---

## 3. `purchases`
Line-level purchase order transactions between the business and its vendors.

**Rows:** 2,372,474 &nbsp;·&nbsp; **Columns:** 16

| Column | Type | Description |
|---|---|---|
| `InventoryId` | string | Composite key linking to the store/product inventory record |
| `Store` | int | Store number |
| `Brand` | int | Product brand code |
| `Description` | string | Product name |
| `Size` | string | Bottle/pack size |
| `VendorNumber` | int | Unique vendor identifier |
| `VendorName` | string | Vendor name |
| `PONumber` | int | Purchase order number |
| `PODate` | date | Date the purchase order was placed |
| `ReceivingDate` | date | Date the order was received into stock |
| `InvoiceDate` | date | Date the vendor invoice was issued |
| `PayDate` | date | Date the invoice was paid |
| `PurchasePrice` | decimal | Cost per unit paid to the vendor |
| `Quantity` | int | Units purchased on this line |
| `Dollars` | decimal | Total line value (`PurchasePrice × Quantity`) |
| `Classification` | int | Internal product classification code |

---

## 4. `purchase_prices`
Vendor list price / cost reference table — one row per product per vendor.

**Rows:** 12,260 &nbsp;·&nbsp; **Columns:** 9

| Column | Type | Description |
|---|---|---|
| `Brand` | int | Product brand code |
| `Description` | string | Product name |
| `Price` | decimal | Retail list price |
| `Size` | string | Bottle/pack size |
| `Volume` | int | Volume in mL |
| `Classification` | int | Internal product classification code |
| `PurchasePrice` | decimal | Vendor's cost price per unit |
| `VendorNumber` | int | Unique vendor identifier |
| `VendorName` | string | Vendor name |

---

## 5. `sales`
Line-level point-of-sale transactions.

**Rows:** 12,825,363 &nbsp;·&nbsp; **Columns:** 14

| Column | Type | Description |
|---|---|---|
| `InventoryId` | string | Composite key linking to the store/product inventory record |
| `Store` | int | Store number |
| `City` | string | City where the store is located |
| `Brand` | int | Product brand code |
| `Description` | string | Product name |
| `Size` | string | Bottle/pack size |
| `SalesDate` | date | Date of the sale |
| `SalesDollars` | decimal | Total revenue for the transaction line |
| `SalesPrice` | decimal | Selling price per unit |
| `SalesQuantity` | int | Units sold on this line |
| `VendorNo` | int | Vendor identifier for the product sold |
| `VendorName` | string | Vendor name |
| `Classification` | int | Internal product classification code |
| `ExciseTax` | decimal | Excise tax applied to the transaction |

---

## 6. `vendor_invoice`
Vendor-level invoice, freight, and approval records.

**Rows:** 5,543 &nbsp;·&nbsp; **Columns:** 10

| Column | Type | Description |
|---|---|---|
| `VendorNumber` | int | Unique vendor identifier |
| `VendorName` | string | Vendor name |
| `InvoiceDate` | date | Date the invoice was issued |
| `PONumber` | int | Associated purchase order number |
| `PODate` | date | Date the purchase order was placed |
| `PayDate` | date | Date the invoice was paid |
| `Quantity` | int | Total units invoiced |
| `Dollars` | decimal | Total invoice value |
| `Freight` | decimal | Freight/shipping cost on the invoice |
| `Approval` | string | Invoice approver name, or `Pending` if not yet approved |

---

## Known Data Quality Notes

Issues identified and resolved during the EDA phase (`03_Python/Exploratory_Data_Analysis.ipynb`):

| Issue | Table / Column | Rows Affected | Resolution |
|---|---|---:|---|
| Missing city values | `end_inventory.City` | 1,284 | Filled with `"Unknown"` |
| Incomplete reference row (missing description, size, price) | `purchase_prices` | 1 | Row dropped |
| Missing product size on purchase records | `purchases.Size` | 3 | Repaired by cross-referencing the brand's known size in `purchase_prices` |
| Missing/blank invoice approver | `vendor_invoice.Approval` | 5,169 | Filled with `"Pending"` |
| Duplicate rows | All 6 tables | 0 | Verified — no duplicates found |
| Inconsistent whitespace | All text columns/headers | — | Stripped leading/trailing whitespace |
| Non-standard date formats | All date columns | — | Coerced to proper `datetime` type |
| Negative numeric values | All numeric columns | 0 | Verified — no negative/erroneous values found |

**Join keys:**
- `sales`, `purchases`, `begin_inventory`, `end_inventory` join on `InventoryId`
- `sales`, `purchases`, `purchase_prices` join on the composite key `Brand + Description + Size`
- `purchases`, `purchase_prices`, `vendor_invoice` join on `VendorNumber` / `VendorNo`
