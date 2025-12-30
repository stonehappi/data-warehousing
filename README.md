# 🎓 Course Syllabus: Modern Data Warehousing & ETL Engineering

## **Course Overview**

This course provides a comprehensive, hands-on deep dive into building an enterprise-grade Data Warehouse using the **Medallion Architecture**. Students will progress from raw data ingestion to building automated, cloud-based Analytics Portals using the Oracle Ecosystem.

* **Platform:** Oracle Cloud Infrastructure (OCI)
* **Database:** Oracle Autonomous Database
* **Tools:** SQL, PL/SQL, Oracle APEX, ORDS (REST)
* **Architecture:** Bronze / Silver / Gold (Lakehouse Pattern)

---

## **Module 1: Infrastructure & Data Foundations**

* **Introduction to Cloud Warehousing:** Understanding OCI, Autonomous Database types (ADW vs. ATP).
* **Environment Setup:** Provisioning cloud instances and configuring APEX Workspaces.
* **The Bronze Layer Concept:** Designing raw landing zones and handling multi-format data ingestion (CSV/XLS).

## **Module 2: Relational Modeling & Initial Visualization**

* **Star Schema Design:** Identifying Fact and Dimension tables within the Olist E-commerce dataset.
* **Relational Logic:** Building SQL Views to create "Single Source of Truth" logic.
* **Rapid Prototyping:** Building the first "Basic Dashboard" to validate data models.

## **Module 3: Advanced Ingestion & Scaling**

* **RESTful Data Services (ORDS):** Building API endpoints for real-time data streaming.
* **Enterprise Expansion:** Managing complex relationships across 9+ disparate data sources.
* **Data Lineage:** Tracking data flow from the API/CSV source to the final database table.

## **Module 4: The Silver Layer (Transformation & Cleansing)**

* **Data Harmonization:** Techniques for joining 9 tables in a single SQL operation.
* **Feature Engineering:** Creating calculated metrics like `Delivery_Performance` and `Order_Profitability`.
* **Cleaning the Mess:** Handling NULLs, mapping foreign language categories (Portuguese to English), and data type standardization.

## **Module 5: The Gold Layer (Performance Optimization)**

* **Data Mart Strategy:** Why and how to split a Master Table into Finance, Logistics, and CX Marts.
* **Pre-Aggregation:** Using physical tables to optimize query concurrency and dashboard speed.
* **Database Indexing:** B-Tree and Bitmap indexing strategies for high-volume analytics.

## **Module 6: Business Intelligence & Executive Reporting**

* **UI/UX for Data:** Designing for the executive "persona" (Finance vs. Logistics users).
* **Geospatial Analytics:** Implementing heatmaps and spatial data to track shipping bottlenecks.
* **Advanced APEX Components:** Faceted Search, Interactive Grids, and Charting Engines.

## **Module 7: Orchestration, Automation & Governance**

* **PL/SQL Programming:** Encapsulating ETL logic into robust, reusable Stored Procedures.
* **Pipeline Automation:** Scheduling nightly refreshes using `DBMS_SCHEDULER`.
* **Error Handling & Monitoring:** Building "Self-Healing" pipelines with transaction control (Rollback/Commit).

---

## **Learning Outcomes**

By the end of this course, students will have built a production-ready Data Warehouse and be able to:

1. **Architect** a Medallion-style data pipeline from scratch.
2. **Write** complex 9-way SQL joins and PL/SQL automation scripts.
3. **Deploy** a cloud-based BI Portal used for executive decision-making.
4. **Optimize** database performance for sub-second dashboard loading.
