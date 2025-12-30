# Phase 1: Infrastructure & Cloud Setup

## 1.1 Overview
The foundation of this Data Warehouse is built on the **Oracle Cloud Infrastructure (OCI)**. The goal of this phase was to provision a high-performance, scalable environment capable of handling large-scale e-commerce datasets while maintaining 24/7 availability for the analytics portal.

## 1.2 Environment Specifications
* **Cloud Provider:** Oracle Cloud Infrastructure (OCI)
* **Database:** Oracle Autonomous Transaction Processing (ATP) / Autonomous Data Warehouse (ADW)
* **Storage:** 20 GB (Always Free Tier)
* **Compute:** 1 OCPU
* **Application Framework:** Oracle APEX (Application Express) 23.x+



## 1.3 Implementation Steps

### Step 1: Provisioning the Autonomous Database
1. Logged into the OCI Console.
2. Navigated to **Oracle Database > Autonomous Database**.
3. Created a new database instance named `OLIST_DWH`.
4. Selected the **Always Free** configuration to ensure zero-cost infrastructure for the portfolio project.
5. Configured the ADMIN credentials and allowed network access from all IP addresses (for development flexibility).

### Step 2: APEX Workspace Configuration
1. Accessed the **Database Actions** menu.
2. Launched **Oracle APEX**.
3. Created a dedicated Workspace named `OLIST_WORKSPACE`.
4. Created a primary Developer user with administrative privileges to manage the application lifecycle.

### Step 3: Network & Security
* **Security Lists:** Configured the OCI Virtual Cloud Network (VCN) to allow HTTPS (Port 443) traffic.
* **Authentication:** Set up the internal APEX authentication scheme for initial development, with plans for role-based access in Phase 10.



## 1.4 Deliverables
* ✅ Active Oracle Autonomous Database instance.
* ✅ Configured APEX Workspace.
* ✅ Administrative and Developer access credentials.
* ✅ Connection string for external SQL clients (SQL Developer / VS Code).

## 1.5 Why This Matters for the Interview
By completing Phase 1, I demonstrated **Cloud Literacy**. I can explain how to provision enterprise-grade resources, manage cloud costs (by utilizing the Free Tier), and set up the necessary middleware (ORDS) to bridge the gap between a raw database and a user-facing application.