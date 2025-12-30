# Phase 10: ETL Orchestration & Automation

## 10.1 Overview
The final phase of the project transitions the Data Warehouse from a manual build to an **Automated Pipeline**. In a production environment, data engineers do not run scripts manually. Phase 10 implements the "Brain" of the system: a centralized **ETL Orchestration** engine that ensures the Silver and Gold layers are always synchronized with the latest data from the Bronze landing zone.

## 10.2 The Automation Framework
The orchestration is built using two core Oracle Database features:
1.  **PL/SQL Stored Procedures:** To encapsulate the transformation logic.
2.  **DBMS_SCHEDULER:** To manage the execution timing and monitoring.



## 10.3 Implementation Steps

### Step 1: Creating the Master ETL Procedure
I consolidated the logic from Phases 7 and 8 into a single, idempotent stored procedure. This procedure follows a **Truncate-and-Load** pattern to ensure data consistency.

```sql
CREATE OR REPLACE PROCEDURE SP_REFRESH_WAREHOUSE AS
BEGIN
    -- Step 1: Refresh Silver Layer (Cleansing & Joins)
    EXECUTE IMMEDIATE 'TRUNCATE TABLE SLR_SALES_MASTER';
    INSERT INTO SLR_SALES_MASTER 
    SELECT /* Logic from Phase 7 */;

    -- Step 2: Refresh Gold Marts (Aggregations)
    EXECUTE IMMEDIATE 'TRUNCATE TABLE GLD_FINANCE_MART';
    INSERT INTO GLD_FINANCE_MART SELECT /* Logic from Phase 8 */;
    
    -- Step 3: Refresh Logistics Mart
    EXECUTE IMMEDIATE 'TRUNCATE TABLE GLD_LOGISTICS_MART';
    INSERT INTO GLD_LOGISTICS_MART SELECT /* Logic from Phase 8 */;

    COMMIT;
EXCEPTION
    WHEN OTHERS THEN
        ROLLBACK;
        RAISE;
END;

```

### Step 2: Scheduling the Nightly Job

I configured the Oracle Database to trigger this procedure every night at 2:00 AM, ensuring that when stakeholders log in to the APEX Portal in the morning, the data is up-to-date.

```sql
BEGIN
    DBMS_SCHEDULER.CREATE_JOB (
        job_name        => 'DAILY_WAREHOUSE_REFRESH',
        job_type        => 'STORED_PROCEDURE',
        job_action      => 'SP_REFRESH_WAREHOUSE',
        repeat_interval => 'FREQ=DAILY; BYHOUR=2; BYMINUTE=0',
        enabled         => TRUE
    );
END;

```

### Step 3: Ad-Hoc "On-Demand" Refresh

To provide flexibility, I added a **"Refresh Data"** button on the Administrative page of the APEX app. This allows a Data Steward to trigger the `SP_REFRESH_WAREHOUSE` procedure manually if an emergency data update is required.

## 10.4 Key Technical Decisions

* **Atomicity:** I wrapped the entire refresh in a single transaction. If any part of the 9-way join fails, the `ROLLBACK` command ensures the Gold Marts aren't left in a partial or corrupted state.
* **Idempotency:** The script is designed to be run multiple times without creating duplicate data, thanks to the `TRUNCATE` logic.
* **Performance:** I included `/*+ APPEND */` hints in the SQL (where applicable) to perform direct-path inserts, significantly speeding up the movement of 100k+ rows.

## 10.5 Deliverables

* ✅ `SP_REFRESH_WAREHOUSE` PL/SQL Procedure.
* ✅ Automated `DBMS_SCHEDULER` Job.
* ✅ Administrative UI for manual pipeline control.

## 10.6 Why This Matters for the Interview

This phase demonstrates **Senior-Level Thinking**. I can explain to an interviewer that anyone can run a query once, but a Data Engineer builds **Pipelines**. This phase proves I understand **Error Handling**, **Job Scheduling**, and the importance of **Data Freshness**. It shows I can build a system that maintains itself, allowing the business to trust the data every single day.
