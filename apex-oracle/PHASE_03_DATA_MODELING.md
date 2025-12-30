# Phase 3: Relational Data Modeling

## 3.1 Overview
In Phase 3, the project transitions from "Flat File" storage to a **Relational Star Schema**. The goal was to establish logical connections between the staging tables created in Phase 2. By creating SQL Views, I built a foundation that allows for complex business questions to be answered without duplicating data.

## 3.2 The Conceptual Schema
The design follows a **Star Schema** approach where:
* **Fact Data:** Sales transactions and order items.
* **Dimension Data:** Product details and order status.



## 3.3 Implementation Steps

### Step 1: Defining Relationships
I identified common keys across the datasets to enable relational joins:
* `STG_ORDERS.order_id` ↔ `STG_ITEMS.order_id`
* `STG_ITEMS.product_id` ↔ `STG_PRODUCTS.product_id`

### Step 2: Creating the Performance View
Instead of forcing the dashboard to query three tables simultaneously, I created a **Unified Business View**. This simplifies the data layer for end-users.

**SQL Logic (Executed in SQL Workshop):**
```sql
CREATE OR REPLACE VIEW V_DWH_SALES_PERFORMANCE AS
SELECT 
    o.order_id,
    o.order_purchase_timestamp as sale_date,
    p.product_category_name,
    i.price,
    i.freight_value,
    (i.price + i.freight_value) as total_order_value,
    TO_CHAR(o.order_purchase_timestamp, 'YYYY-MM') as sale_month
FROM STG_ORDERS o
JOIN STG_ITEMS i ON o.order_id = i.order_id
JOIN STG_PRODUCTS p ON i.product_id = p.product_id;

```

### Step 3: Data Integrity Checks

To ensure the joins didn't result in "Data Fan-out" (duplicated rows), I verified that the total revenue in the View matched the sum of the raw `STG_ITEMS` table.

## 3.4 Key Technical Decisions

* **Use of Views:** I chose SQL Views over physical tables at this stage to maintain "Live" data connections. This ensures that any new data added to the Bronze layer (Phase 2) is immediately visible in the Analytics layer.
* **Calculated Columns:** I implemented the `total_order_value` logic (Price + Freight) inside the view to ensure "Single Source of Truth" for revenue calculations across all reports.

## 3.5 Deliverables

* ✅ Logical ERD defining table relationships.
* ✅ `V_DWH_SALES_PERFORMANCE` view for dashboard consumption.
* ✅ Optimized SQL join logic for high-concurrency querying.

## 3.6 Why This Matters for the Interview

This phase demonstrates my ability to **Model Data for Business**. Interviewers look for candidates who understand that raw data is rarely useful on its own. By creating a relational view, I showed how to transform technical data structures into a business-friendly format, reducing complexity for the front-end developers and BI analysts.
