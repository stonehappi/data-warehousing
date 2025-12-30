# Phase 5: Real-Time Data Ingestion (REST API)

## 5.1 Overview
While Phase 2 focused on batch processing (CSV uploads), Phase 5 introduces **Real-Time Data Ingestion**. The goal was to transform the database into a service-oriented architecture by exposing **REST Endpoints**. This allows external applications, webhooks, or mobile apps to push sales data directly into the Bronze layer without manual intervention.

## 5.2 Technical Stack
* **Engine:** Oracle REST Data Services (ORDS).
* **Format:** JSON (JavaScript Object Notation).
* **Protocols:** HTTPS / REST (POST Method).



## 5.3 Implementation Steps

### Step 1: Enabling REST Services
Before creating endpoints, I enabled REST access for the database schema via **SQL Workshop > RESTful Services**.

### Step 2: Creating the POST Template
I developed a RESTful module designed to handle incoming sales transactions. 
* **Module Name:** `olist.api`
* **Template:** `sales/`
* **Method:** `POST`

### Step 3: Writing the PL/SQL Handler
The handler parses the incoming JSON payload and maps the values to the `STG_ORDERS` and `STG_ITEMS` tables.

**PL/SQL Logic:**
```sql
BEGIN
    -- Insert into Staging Orders
    INSERT INTO STG_ORDERS (order_id, customer_id, order_status, order_purchase_timestamp)
    VALUES (:order_id, :customer_id, 'created', SYSTIMESTAMP);

    -- Insert into Staging Items
    INSERT INTO STG_ITEMS (order_id, product_id, price, freight_value)
    VALUES (:order_id, :product_id, :price, :freight);
    
    COMMIT;
END;

```

### Step 4: Testing the Endpoint

I verified the API using a standard JSON payload via a REST client (like Postman or cURL).

**Example Payload:**

```json
{
    "order_id": "API_REF_999",
    "customer_id": "CUST_001",
    "product_id": "PROD_55",
    "price": 120.50,
    "freight": 15.00
}

```

## 5.4 Key Technical Decisions

* **Decoupling:** By pushing data into the `STG_` (Bronze) tables first, I ensured that the API remains fast. The heavy transformation logic (Silver/Gold) is handled separately by the database, preventing API timeouts.
* **JSON Mapping:** Utilized ORDS parameter mapping to automatically bind JSON keys to SQL variables (e.g., `:order_id`).

## 5.5 Deliverables

* ✅ Enabled REST Schema in Oracle APEX.
* ✅ Fully functional POST endpoint for real-time sales ingestion.
* ✅ Documented API payload structure for external developers.

## 5.6 Why This Matters for the Interview

This phase demonstrates **System Integration** skills. I can explain how to bridge the gap between "Static Data" (CSVs) and "Live Data" (APIs). In an interview, this shows that I understand how modern e-commerce platforms operate—where data is constantly flowing from front-end websites into back-end warehouses via microservices.