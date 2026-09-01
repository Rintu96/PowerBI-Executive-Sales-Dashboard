# 📊 Executive Sales & Profit Dashboard

An end-to-end interactive Power BI analytical solution built for executive leadership to analyze revenue trajectories, product performance, and regional order distributions.

---

## 📸 Dashboard Preview

> **UI Theme:** Dark Mode (`#1E293B` Navy Canvas, glowing visual card accents)  
> **Source Dataset:** `SalesData_1000Rows_WithIssues_copy.csv`

---

## 🧰 Tools & Technologies Used

* **Business Intelligence:** Microsoft Power BI Desktop
* **Data Transformation:** Power Query (M Language, Math Imputation)
* **Data Modeling & Analytics:** DAX (Data Analysis Expressions)
* **Design & Layout:** Custom UI Mockup Grid, Hex Accent Styling (`#1E293B`, `#38BDF8`, `#34D399`)

---

## 🛠️ Data Preparation & Cleaning (Power Query M)

The raw transactional data contained missing fields, irregular date formats, and duplicate records. A multi-step ETL pipeline was executed in Power Query Editor prior to modeling

### 1. Handling Missing Values (Formula Imputation)
Rather than dropping records or imputing static zeros, missing numeric metrics were calculated dynamically using mathematical relationship formulas:

* **`UnitPrice` Imputation:** Created custom column substituting nulls via `[Sales] / [Quantity].
* **`Sales` Imputation:** Created custom column substituting nulls via `[Quantity] * [UnitPrice].
* **`Cost` Imputation:** Created custom column substituting nulls via `[Sales] - [Profit]`.
* **`Profit` Imputation:** Created custom column substituting nulls via `[Sales] - [Cost]`.

*Post-imputation: Original columns containing nulls were deleted, and custom columns were renamed back to `UnitPrice`, `Sales`, `Cost`, and `Profit` before explicitly setting Data Type to `Decimal Number`.*

### 2. Handling Categorical & Date Anomalies
* **Text Columns (`CustomerName`, `Region`, `Product`):** Replaced `null` values with `"Unknown"` using **Replace Values**
* **Order Date (`OrderDate`):** Standardized format to **Date** type using locale handling and fill-down imputation
* **Deduplication:** Applied **Remove Duplicates** across the table using primary identifier keys.

---

## 📐 Data Modeling & DAX Calculations

### Calculated Table (`EastRegionOrders`)
Isolated transaction records originating exclusively from the East region:
```dax
EastRegionOrders = 
FILTER(
    'SalesData_1000Rows_WithIssues_copy', 
    'SalesData_1000Rows_WithIssues_copy'[Region] = "East"
)

