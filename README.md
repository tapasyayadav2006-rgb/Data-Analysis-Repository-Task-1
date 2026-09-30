# Data-Analysis-Repository-Task-1

### 📁 Project Title: Sales Data Quality & Cleaning Analysis

This project centers on the end-to-end audit, validation, and missing-value resolution of a transactional dataset containing **1,000 distinct orders** recorded between January 1, 2025, and January 1, 2026. 

---

### 📊 Dataset Overview
The collection spans retail transactions across multiple cities and product categories. The data pipeline accounts for 12 primary structural metrics alongside 2 custom operational attributes introduced to track system compliance and isolate duplicate records.

#### Key Structural Metrics:
*   **Total Transactions Monitored:** 1,000 records
*   **Original Fields:** 12
*   **Audit Attributes Added:** 2 (`Transaction_Key`, `Data_Quality_Flag`)
*   **Date Window:** 2025-01-01 to 2026-01-01

---

### 🔍 Data Integrity Issues & Rules Applied

| Evaluation Check | Detected Discrepancies | Applied Action / Interpretation |
| :--- | :--- | :--- |
| **Missing Customer Age** | 20 instances | Filled missing values using the computed dataset **median age of 41**. |
| **Missing Customer City** | 13 instances | Updated null values to a categorical label of **'Unknown'**. |
| **Duplicate System IDs** | 8 instances | Isolated repetitive instances belonging strictly to a single ID: **`ORD100050`**. Records were preserved and flagged to protect sequential data. |
| **Mathematical Mismatches** | 0 instances | Verified that `Quantity × Unit_Price` matches `Total_Sales` perfectly across all rows. |
| **Value Inconsistencies** | 0 instances | Confirmed zero instances of negative values or zeroes across prices and quantities. |

---

### 🛠️ Fields Architecture Reference

1.  **`Order_ID`** *(Text)*: The core identifier supplied in the transaction stream. Contains identical overlaps flagged during processing.
2.  **`Transaction_Key`** *(Text)*: A composite surrogate key generated dynamically (`Order_ID` + row instance count) to enforce absolute uniqueness without overwriting raw legacy system data.
3.  **`Order_Date`** ^(Date)^: Formatted date string standardized to system datetime type for temporal aggregation.
4.  **`Customer_ID` / `Customer_Name`** *(Text)*: Unique relational alphanumeric key matching individual retail accounts and names.
5.  **`Age`** *(Integer)*: Customer age profile, fixed using median interpolation to secure unbroken profiles for demographic analysis.
6.  **`Gender`** *(Categorical)*: Segmented split tracking customer identifier groups (**Male** / **Female**).
7.  **`City`** *(Categorical)*: Regional location of purchase execution (e.g., Bengaluru, Mumbai, Pune, Patna, Delhi, Gaya, Kolkata, Hyderabad).
8.  **`Product` / `Category`** *(Categorical)*: Line item indicators tracking individual types (**Rice**, **Book**, **Laptop**, **Mobile**, **Chair**, **Shoes**) and operational families.
9.  **`Quantity`** *(Integer)*: Volume units ordered.
10. **`Unit_Price`** *(Numeric)*: Evaluated unit currency cost.
11. **`Total_Sales`** *(Numeric)*: Final financial sum per sequence line, matching the evaluated line calculation.
12. **`Data_Quality_Flag`** *(Text)*: Real-time operational validation layer tracking individual cleansing modifications (e.g., *Age imputed*, *Duplicate Order_ID*, *City set to Unknown*, or *OK*).

