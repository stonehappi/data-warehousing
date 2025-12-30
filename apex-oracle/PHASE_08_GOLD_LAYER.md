# Phase 8: The Gold Layer (Business Data Marts)

## 8.1 Overview
The **Gold Layer** is the final destination for high-value data. In this phase, I transitioned from the "general-purpose" Master Table (Silver) to specialized **Data Marts**. These tables are pre-aggregated, meaning the complex calculations (sums, averages, counts) are performed during the ETL process rather than when the dashboard loads. This ensures sub-second performance for executive users.



## 8.2 Data Mart Design
I designed three distinct marts to serve different business departments:
1.  **Finance Mart:** Focused on revenue streams and payment efficiency.
2.  **Customer Satisfaction Mart:** Focused on product quality and category sentiment.
3.  **Logistics Mart:** Focused on geographic delivery performance.

## 8.3 Implementation Steps

### Step 1: Finance Mart (Revenue & Payments)
This mart squashes 100k+ rows into a summarized view of how money enters the business.

```sql
CREATE TABLE GLD_FINANCE_MART AS
SELECT 
    payment_type,
    COUNT(order_id) as total_transactions,
    SUM(price) as total_revenue,
    ROUND(AVG(price), 2) as avg_order_value
FROM SLR_SALES_MASTER
GROUP BY payment_type;

```

### Step 2: Customer Experience Mart (Quality Control)

This mart isolates product categories that are high-performers versus those with quality issues.

```sql
CREATE TABLE GLD_CUSTOMER_MART AS
SELECT 
    product_category,
    ROUND(AVG(review_score), 2) as avg_rating,
    COUNT(review_score) as review_count
FROM SLR_SALES_MASTER
WHERE review_score IS NOT NULL
GROUP BY product_category;

```

### Step 3: Logistics Mart (Regional Performance)

This mart organizes data by state to identify where the supply chain is failing.

```sql
CREATE TABLE GLD_LOGISTICS_MART AS
SELECT 
    customer_state,
    delivery_status,
    COUNT(*) as order_volume
FROM SLR_SALES_MASTER
GROUP BY customer_state, delivery_status;

```

## 8.4 Key Technical Decisions

* **Pre-Aggregation:** I chose to store these as physical tables rather than Views. In an interview, I can explain that this is an **"Optimization for Concurrency."** Even if 50 executives open the dashboard at once, the database only reads the pre-calculated rows, avoiding a massive scan of the Silver table.
* **Separation of Concerns:** By splitting data into marts, I ensured that the Finance team doesn't have to wade through Logistics data to find their KPIs, simplifying the development of department-specific dashboard pages.

## 8.5 Deliverables

* ✅ `GLD_FINANCE_MART`: Revenue analysis by payment type.
* ✅ `GLD_CUSTOMER_MART`: Product sentiment analysis.
* ✅ `GLD_LOGISTICS_MART`: Regional bottleneck reporting.

## 8.6 Why This Matters for the Interview

This phase proves I understand **Database Performance Tuning**. I can discuss the trade-offs between storage and speed. By "paying" a small amount in storage to keep these Gold tables, I "save" massive amounts of CPU and time when the business users query the data. It shows a **Business-First** mindset—prioritizing the user experience of the final dashboard.
