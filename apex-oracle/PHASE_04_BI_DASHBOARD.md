# Phase 4: Basic BI Dashboard

## 4.1 Overview
Phase 4 focuses on **Data Visualization**. Raw tables and views provide the logic, but the dashboard provides the "Story." The goal was to build a user-friendly interface in Oracle APEX that allows executives to monitor Sales Performance and Category Trends in real-time using the view created in Phase 3.

## 4.2 Key Dashboard Components
To provide a comprehensive view of the business, I implemented two primary UI patterns:
1.  **Analytical Charts:** Visualizing high-level trends.
2.  **Faceted Search:** Empowering users to discover specific data points through dynamic filtering.



## 4.3 Implementation Steps

### Step 1: Creating the Application
1.  Navigated to **App Builder** in Oracle APEX.
2.  Created a new application named **Olist Executive Analytics**.
3.  Set the theme to **Universal Theme** (Vita) for a modern, responsive mobile-friendly look.

### Step 2: Implementing Faceted Search (The "Discovery" Tool)
Instead of a simple table, I used **Faceted Search** to allow users to filter millions of rows effortlessly.
1.  Created a new page using the **Faceted Search** template.
2.  **Data Source:** `V_DWH_SALES_PERFORMANCE`.
3.  **Configured Facets:** * `PRODUCT_CATEGORY_NAME` (Checkbox)
    * `SALE_MONTH` (Checkbox)
    * `PRICE` (Range Slider)



### Step 3: Building Visual Analytics
I added a "Dashboard" region at the top of the page with the following charts:
* **Top 5 Categories (Bar Chart):** * `SQL: SELECT product_category_name, SUM(total_order_value) FROM V_DWH_SALES_PERFORMANCE GROUP BY product_category_name ORDER BY 2 DESC`
* **Monthly Sales Trend (Line Chart):**
    * `SQL: SELECT sale_month, SUM(total_order_value) FROM V_DWH_SALES_PERFORMANCE GROUP BY sale_month ORDER BY 1 ASC`

## 4.4 Technical Highlights
* **Low-Code Efficiency:** By using Oracle APEX, I was able to build a production-ready BI interface in hours rather than days, focusing on SQL logic rather than CSS/HTML.
* **Responsive Design:** The dashboard automatically adjusts for viewing on mobile devices, ensuring stakeholders have access to data anywhere.
* **Dynamic Data Binding:** Because the dashboard is linked directly to the `V_DWH_SALES_PERFORMANCE` view, any data added to the Bronze layer updates the charts instantly.

## 4.5 Deliverables
* ✅ Fully functional Web Application.
* ✅ Interactive Sales Dashboard with Bar and Line visualizations.
* ✅ Faceted Search interface for ad-hoc data discovery.

## 4.6 Why This Matters for the Interview
This phase proves I understand **Data Democratization**. In an interview, I can explain that I didn't just build a database; I built a *solution*. I chose **Faceted Search** because it reduces the "technical barrier," allowing non-technical managers to find the data they need without asking a developer to write a custom SQL query.