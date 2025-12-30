# Phase 2: The Initial Bronze Layer (Staging)

## 2.1 Overview
The **Bronze Layer** acts as the landing zone for raw data. The objective of this phase was to ingest the core e-commerce datasets into the Oracle Autonomous Database without applying complex transformations. This ensures that a "raw" copy of the data is always available for auditing or reprocessing.

## 2.2 Data Source Details
For the initial staging, the three core files from the Olist Brazilian E-Commerce dataset were utilized:
1.  **Orders Table:** Primary transaction headers (dates, status).
2.  **Items Table:** Individual product lines per order (price, shipping cost).
3.  **Products Table:** Metadata regarding the items sold.



## 2.3 Implementation Steps

### Step 1: Data Preparation
* Verified the CSV encoding (UTF-8) to ensure Brazilian Portuguese characters (accents/special characters) would import correctly.
* Identified primary keys (e.g., `order_id`, `product_id`) to ensure data uniqueness during the load.

### Step 2: Ingestion via APEX Data Workshop
1. Navigated to **SQL Workshop > Utilities > Data Workshop**.
2. Selected **Load Data**.
3. Uploaded the files individually:
    * **Target Table:** `STG_ORDERS`, `STG_ITEMS`, `STG_PRODUCTS`.
    * **Configuration:** Allowed APEX to auto-detect column types (VARCHAR2 for IDs, NUMBER for prices, and TIMESTAMP for dates).
4. Executed the load and verified the row counts against the original CSV files.



### Step 3: Schema Validation
After loading, the following SQL was executed in **SQL Commands** to verify the data integrity:
```sql
SELECT 'STG_ORDERS' as table_name, count(*) as record_count FROM STG_ORDERS
UNION ALL
SELECT 'STG_ITEMS', count(*) FROM STG_ITEMS
UNION ALL
SELECT 'STG_PRODUCTS', count(*) FROM STG_PRODUCTS;

```

## 2.4 Database Objects Created

| Table Name | Description | Key Columns |
| --- | --- | --- |
| `STG_ORDERS` | Raw order headers | `order_id`, `order_status`, `purchase_timestamp` |
| `STG_ITEMS` | Transactional details | `order_id`, `product_id`, `price`, `freight_value` |
| `STG_PRODUCTS` | Product catalog | `product_id`, `product_category_name` |

## 2.5 Deliverables

* ✅ Three functional staging tables populated with ~100k records.
* ✅ Data validation report confirming zero records were rejected during ingestion.
* ✅ Documented mapping of CSV headers to Oracle Database columns.

## 2.6 Why This Matters for the Interview

In an interview, I can discuss **Data Ingestion Patterns**. I chose to load data into staging tables first rather than transforming it "on the fly." This follows the **ELT (Extract, Load, Transform)** philosophy, which is standard in modern cloud data warehousing because it leverages the massive computing power of the database for the transformation step.