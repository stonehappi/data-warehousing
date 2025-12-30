# Phase 6: Enterprise Bronze Layer Expansion

## 6.1 Overview
In Phase 6, the project scales from a simplified 3-table model to a comprehensive **Enterprise Data Warehouse** architecture. The goal was to ingest the remaining 6 datasets from the Olist repository, bringing the total to 9 files. This expansion allows for 360-degree analytics across logistics, customer satisfaction, and financial payment patterns.

## 6.2 The Complete Bronze Schema
In professional data engineering, the **Bronze Layer** (or Landing Zone) preserves the raw state of data. I implemented a strict naming convention using the `BRZ_` prefix for all tables to distinguish raw data from downstream transformations.

| Table Name | Business Domain | Key Data Points |
| :--- | :--- | :--- |
| `BRZ_CUSTOMERS` | Stakeholders | Unique Customer IDs, Zip Codes, City, State |
| `BRZ_SELLERS` | Partners | Seller Locations, Lead Times |
| `BRZ_PAYMENTS` | Finance | Credit Cards, Vouchers, Installment Counts |
| `BRZ_REVIEWS` | Experience | Star Ratings (1-5), Survey Comments |
| `BRZ_GEOLOCATION` | Logistics | Latitude, Longitude, Map Coordinates |
| `BRZ_TRANSLATION` | Support | Portuguese to English Category Mapping |



## 6.3 Implementation Steps

### Step 1: Standardized Ingestion
Using the **APEX Data Workshop**, I uploaded the remaining 6 CSV files. 
* **Critical Setting:** I ensured that Geolocation coordinates were stored as `NUMBER` and all ID fields remained `VARCHAR2` to prevent leading-zero truncation.
* **Integrity Check:** Verified that the record counts in the Oracle Database matched the source CSV files exactly.

### Step 2: The Translation Lookup
A key component of this phase was the ingestion of the `product_category_name_translation` file. This table acts as a **Reference Dimension**, allowing the warehouse to translate local Portuguese data into a global English format for executive reporting.

### Step 3: Data Audit (Quality Control)
After the 9-file load, I executed a **Data Health Check** to identify missing values or orphans (e.g., reviews without corresponding orders).

**Audit SQL:**
```sql
SELECT 'Reviews' as source, COUNT(*) as total_rows FROM BRZ_REVIEWS
UNION ALL
SELECT 'Payments', COUNT(*) FROM BRZ_PAYMENTS
UNION ALL
SELECT 'Geolocation', COUNT(*) FROM BRZ_GEOLOCATION;

```

## 6.4 Key Technical Decisions

* **Raw Preservation:** I consciously decided not to fix null values or typos in this phase. In a Medallion Architecture, the Bronze layer must remain a **"Source of Truth"** that reflects the source system's flaws for future debugging.
* **Storage Optimization:** Given the size of the `BRZ_GEOLOCATION` table (1M+ rows), I verified that the Oracle Autonomous Database was utilizing compression to optimize storage costs within the Free Tier limits.

## 6.5 Deliverables

* ✅ 9 Raw Tables (`BRZ_`) fully populated and indexed.
* ✅ Documented Data Dictionary for the Olist dataset.
* ✅ Verified relational links between all 9 datasets.

## 6.6 Why This Matters for the Interview

This phase demonstrates **Scalability and Organization**. I can explain to an interviewer that while a simple 3-table join is easy, managing **9 disparate data sources** requires a structured architecture. By implementing the `BRZ_` prefix and a landing zone strategy, I showed that I can build a system that remains organized as the complexity of the business increases.