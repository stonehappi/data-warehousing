# Phase 7: The Silver Layer (Cleansing & Harmonization)

## 7.1 Overview
In Phase 7, the data moves from the raw landing zone (Bronze) to the **Silver Layer**. The goal of this phase is to perform **Data Harmonization**—cleansing, filtering, and joining all 9 disparate datasets into a single, high-quality "Master Table." This layer represents the validated "Truth" of the business operations.

## 7.2 Transformation & Cleansing Rules
To prepare the data for enterprise analytics, I applied the following transformation logic:
* **Translation:** Standardized Portuguese product categories into English using the `BRZ_TRANSLATION` lookup table.
* **Handling Nulls:** Used `NVL` functions to ensure that orders without categories are labeled as 'UNCATEGORIZED'.
* **Enrichment:** Joined Customer and Seller states to enable "Origin-to-Destination" logistics analysis.
* **Feature Engineering:** Created a `DELIVERY_PERFORMANCE` flag by comparing actual delivery dates against estimated delivery dates.



## 7.3 Implementation Steps

### Step 1: Creating the Silver Master Table
I created a physical table to store the cleansed results. Unlike a view, a physical table in the Silver layer allows for indexing and faster downstream processing for the Gold layer.

```sql
CREATE TABLE SLR_SALES_MASTER (
    order_id           VARCHAR2(50),
    order_date         TIMESTAMP,
    product_category   VARCHAR2(100),
    price              NUMBER,
    freight_value      NUMBER,
    payment_type       VARCHAR2(50),
    review_score       NUMBER,
    customer_state     VARCHAR2(50),
    seller_state       VARCHAR2(50),
    delivery_status    VARCHAR2(20)
);

```

### Step 2: Executing the 9-Way Join (ETL Logic)

This SQL script serves as the primary "Transform" engine of the ETL pipeline.

```sql
INSERT INTO SLR_SALES_MASTER
SELECT 
    i.order_id,
    o.order_purchase_timestamp,
    UPPER(NVL(t.product_category_name_english, NVL(p.product_category_name, 'OTHER'))) as category,
    i.price,
    i.freight_value,
    pay.payment_type,
    rev.review_score,
    c.customer_state,
    s.seller_state,
    CASE 
        WHEN o.order_delivered_customer_date <= o.order_estimated_delivery_date THEN 'ON TIME'
        ELSE 'DELAYED'
    END as delivery_status
FROM BRZ_ITEMS i
JOIN BRZ_ORDERS o ON i.order_id = o.order_id
JOIN BRZ_PRODUCTS p ON i.product_id = p.product_id
JOIN BRZ_CUSTOMERS c ON o.customer_id = c.customer_id
JOIN BRZ_SELLERS s ON i.seller_id = s.seller_id
LEFT JOIN BRZ_PAYMENTS pay ON o.order_id = pay.order_id
LEFT JOIN BRZ_REVIEWS rev ON o.order_id = rev.order_id
LEFT JOIN BRZ_TRANSLATION t ON p.product_category_name = t.product_category_name;

```

## 7.4 Key Technical Decisions

* **Left Joins for Experience Data:** I used `LEFT JOIN` for the `REVIEWS` and `PAYMENTS` tables. This ensures that sales records are not dropped if a customer hasn't left a review yet.
* **Data Indexing:** I implemented B-Tree indexes on `order_date` and `product_category` within the Silver table to optimize the performance of the aggregation scripts in Phase 8.

## 7.5 Deliverables

* ✅ A unified `SLR_SALES_MASTER` table containing cleansed, English-language data.
* ✅ Documented ETL logic for 9-way data integration.
* ✅ Verified "On-Time vs Delayed" performance metrics.

## 7.6 Why This Matters for the Interview

This phase is where I demonstrate **Data Quality Engineering**. I can explain how I handled messy, real-world data (Portuguese labels, missing reviews) to create a reliable dataset. In an interview, being able to explain the logic behind the `CASE` statement for delivery performance shows that I can translate vague business requirements into concrete SQL logic.