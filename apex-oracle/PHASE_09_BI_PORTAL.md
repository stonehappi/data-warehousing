# Phase 9: Enterprise BI Portal

## 9.1 Overview
In Phase 9, I transitioned from data engineering to **Business Intelligence (BI) Development**. The goal was to build a multi-departmental portal in Oracle APEX that provides a "360-degree view" of the company. Unlike the basic dashboard in Phase 4, this portal uses the **Gold Data Marts** to provide specialized, high-speed insights for Finance, Logistics, and Customer Success teams.

## 9.2 Portal Architecture
The application was designed with a modular navigation structure, allowing different stakeholders to access their relevant KPIs through a centralized "Executive Home" page.



## 9.3 Implementation Steps

### Step 1: Navigation & Layout
* **Shared Components:** Configured a side-navigation menu to categorize the application into three functional zones: Finance, Logistics, and Quality.
* **Global Filters:** Implemented a page-zero (Global) filter for "Year" and "State," allowing users to filter the entire portal with a single selection.

### Step 2: The Finance Dashboard (Mart A)
* **Objective:** Track revenue health and payment efficiency.
* **Visualization:** * **Donut Chart:** Revenue breakdown by `payment_type`.
    * **Key Metric Cards:** Displaying Total Revenue, Avg Order Value, and Total Transaction Count.
* **Data Source:** `GLD_FINANCE_MART`.

### Step 3: The Logistics Map (Mart C)
* **Objective:** Identify geographic shipping delays.
* **Visualization:** * **Geospatial Map:** A "Heat Map" of Brazil where states are shaded based on the volume of `DELAYED` orders.
    * **Logic:** This allows the supply chain team to visualize where to improve carrier partnerships.
* **Data Source:** `GLD_LOGISTICS_MART`.



### Step 4: The Customer Quality Report (Mart B)
* **Objective:** Monitor product sentiment.
* **Visualization:** * **Top/Bottom 10 List:** Using the `avg_rating`, I created a list of categories that are performing poorly to trigger quality control reviews.
* **Data Source:** `GLD_CUSTOMER_MART`.

## 9.4 Key Technical Decisions
* **Utilization of Gold Tables:** By sourcing all charts from the Gold layer, the pages load in under **200ms**, even with the large underlying Olist dataset.
* **Interactive Grid:** On the detail pages, I implemented Interactive Grids, enabling users to export filtered data to CSV or PDF for external meetings.
* **Conditional Formatting:** Added "Traffic Light" colors (Red/Yellow/Green) to the review scores and delivery statuses to provide immediate visual cues to management.

## 9.5 Deliverables
* ✅ Multi-page Executive BI Portal.
* ✅ Geospatial logistics visualization.
* ✅ Department-specific data drill-downs.

## 9.6 Why This Matters for the Interview
This phase demonstrates **User-Centric Design**. I can explain that a Data Warehouse is only as good as the decisions it enables. By building a portal with clear navigation and specialized pages, I ensured that the data is "Actionable." I can speak to the **Map Integration** specifically as a way to simplify complex geographic data for non-technical stakeholders.