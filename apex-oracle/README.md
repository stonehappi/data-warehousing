# 📑 Project Documentation: Olist E-Commerce Data Warehouse

* **A 10-Phase End-to-End Implementation Guide**

## Phase 1: Infrastructure & Cloud Setup

**Goal:** Establish a high-availability cloud environment for data storage and application hosting.

* **Environment:** Oracle Autonomous Database (Always Free Tier).
* **Tools:** Oracle APEX (Application Express).
* **Action:** Provisioned the database instance, created a workspace, and configured administrative users.

## Phase 2: The Initial Bronze Layer (Staging)

**Goal:** Ingest the core datasets to begin the modeling process.

* **Tables:** `STG_ORDERS`, `STG_ITEMS`, `STG_PRODUCTS`.
* **Method:** Utilized the APEX Data Workshop to map CSV headers to relational columns.
* **Outcome:** Successfully loaded ~100k rows of raw transaction data.

## Phase 3: Relational Data Modeling

**Goal:** Create a "Logical View" of the data to fix fragmentation.

* **Approach:** Defined a Star Schema architecture using SQL Views.
* **SQL Logic:** Joined `ORDERS` and `ITEMS` to create a `V_DWH_SALES` view, providing a single source of truth for revenue.

## Phase 4: Basic BI Dashboard

**Goal:** Provide the first layer of visualization for business users.

* **Components:** Bar charts for Revenue by Category, Line charts for Sales Trends.
* **Feature:** Added **Faceted Search** to allow users to filter data by date range and category without writing SQL.

## Phase 5: Real-Time Data Ingestion (REST API)

**Goal:** Move beyond batch processing to support live application data.

* **Tool:** ORDS (Oracle REST Data Services).
* **Feature:** Developed a `POST` handler that accepts JSON payloads to insert new sales records directly into the Bronze layer.

## Phase 6: Enterprise Bronze Layer Expansion

**Goal:** Scale the warehouse to include all 9 operational files.

* **New Data:** Payments, Reviews, Customers, Sellers, Geolocation, and Category Translations.
* **Naming Convention:** Prefixed all raw tables with `BRZ_` to establish clear data lineage.

## Phase 7: The Silver Layer (Cleansing & Harmonization)

**Goal:** Transform raw data into a high-quality, unified "Master Table."

* **Logic:** * Translated Portuguese product categories to English.
* Standardized date formats.
* Calculated `Delivery_Status` (On-Time vs. Delayed) using `CASE` statements.

* **Join Strategy:** Used `LEFT JOIN` on reviews and payments to prevent data loss for unrated/unpaid orders.

## Phase 8: The Gold Layer (Performance Optimization)

**Goal:** Create specialized, pre-aggregated tables for sub-second reporting.

* **Data Marts:**

1. **GLD_FINANCE_MART:** Pre-calculated revenue totals.
2. **GLD_LOGISTICS_MART:** Regional delivery performance metrics.
3. **GLD_CUSTOMER_MART:** Satisfaction ratings by category.

* **Result:** Reduced query overhead on the dashboard by 80%.

## Phase 9: Enterprise BI Portal

**Goal:** Deploy a multi-page executive application.

* **Pages:**
* **Logistics Map:** A geospatial heatmap of Brazil identifying shipping bottlenecks.
* **CX Analytics:** Gauge charts monitoring Average Review Scores.
* **Finance Hub:** Breakdown of sales by payment type (Credit Card, Boleto, etc.).

## Phase 10: ETL Orchestration & Automation

**Goal:** Ensure the warehouse remains a "Living System" with zero manual intervention.

* **Logic:** Encapsulated all transformation code into a PL/SQL Stored Procedure `SP_REFRESH_WAREHOUSE`.
* **Automation:** Configured `DBMS_SCHEDULER` to trigger a full refresh nightly at 2:00 AM.
* **Governance:** Added a manual "Refresh" button for data stewards.

---

### 👨‍💻 Technical Summary for Recruiters

* **Primary Languages:** SQL, PL/SQL.
* **Architecture Style:** Medallion (Data Lakehouse approach).
* **Database Performance:** Implemented physical indexing and pre-aggregated gold tables.
* **Cloud Competency:** Fully managed via Oracle Cloud Infrastructure (OCI).
