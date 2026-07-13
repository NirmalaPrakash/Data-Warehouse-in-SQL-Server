# Data-Warehouse-in-SQL-Server
A **Data Warehouse** serves as a centralized, integrated, and reliable source of data for reporting and analytics. By implementing ETL processes, proper architecture, data modeling, and governance, organizations can transform raw data into meaningful business insights efficiently and accurately.

OR

A Data Warehouse is a master data bank used by businesses to make smart decisions. Instead of handling day-to-day transactions, it gathers information from various company systems (like sales, inventory, and customer databases), organizes it into a single format, and stores it so you can analyze long-term trends

OR

A Data Warehouse is a **subject-oriented, integrated, time-variant, and non-volatile** collection of data in support of management's decision-making process.

The four core concepts mean:
## 1. Subject-oriented: 
Data is grouped around specific business topics (such as Customers or Products) rather than by how applications process it.
## 2. Integrated: 
Information from multiple disconnected sources (e.g., CRM and ERP systems) is cleaned and standardised to ensure it matches perfectly.
## 3. Time-variant: 
It acts as a timeline, holding historical data over months or years so you can compare past and present performance.
## 4. Non-volatile: 
Once data is stored, it becomes read-only. It doesn’t change or get deleted during daily operations, ensuring a permanent "source of truth".

---
## Key Points

## 1. Purpose of a Data Warehouse
   - Centralizes data from multiple source systems.
   - Provides a single source of truth for reporting.
   - Stores historical data.
   - Improves decision-making.
   - Reduces manual reporting efforts.
   - Supports large-scale data processing.

## 2. Problems Without a Data Warehouse
  - Analysts manually collect data.
  - Reports become inconsistent.
  - Different departments use different versions of data.
  - High risk of human errors.
  - Difficult to integrate multiple data sources.
  - Poor scalability for large datasets.

## 3. Data Warehouse Architecture Approaches
- **a) Data Warehouse**
  - Structured data.
  - Reporting-focused.
    
- **b) Data Lake**
  - Structured and unstructured data.
  - Supports advanced analytics.
    
- **c) Data Lakehouse**
  - Combines Data Warehouse and Data Lake benefits.
    
- **d) Data Mesh**
  - Decentralized data ownership across business domains.

### 4. What is CRM and ERP?

**CRM (Customer Relationship Management)** is a system used to manage interactions with customers and prospects.

Its main purpose is to:

* Track customers
* Manage sales activities
* Store customer information
* Improve customer relationships

#### Example CRM Data

| CustomerID | CustomerName | Email                                           | Phone      | City     |
| ---------- | ------------ | ----------------------------------------------- | ---------- | -------- |
| 101        | Nirmala Prakash   | [nimmu@gmail.com](mailto:prakash@gmail.com)     | 9380025025 | Bangalore |

#### Popular CRM Systems

* Salesforce
* Microsoft Dynamics 365
* HubSpot CRM
* Zoho CRM

#### CRM Modules

```text
Customers
    │
    ├── Leads
    ├── Opportunities
    ├── Contacts
    ├── Sales
    └── Customer Support
```

---

### What is ERP?

**ERP (Enterprise Resource Planning)** is a system used to manage internal business operations and resources.

Its main purpose is to:

* Manage products
* Inventory
* Finance
* Purchasing
* Manufacturing
* Human Resources

#### Example ERP Data

| ProductID | ProductName | Quantity | Price |
| --------- | ----------- | -------- | ----- |
| P101      | Laptop      | 500      | 60000 |

#### Popular ERP Systems

* SAP ERP
* Oracle ERP Cloud
* Microsoft Dynamics 365 Finance
* Odoo

#### ERP Modules

```text
Business Operations
        │
        ├── Finance
        ├── Inventory
        ├── Procurement
        ├── Manufacturing
        ├── HR
        └── Supply Chain
```

---

## CRM vs ERP

| Feature           | CRM                     | ERP                           |
| ----------------- | ----------------------- | ----------------------------- |
| Focus             | Customers               | Business Operations           |
| Users             | Sales Team              | Finance, HR, Operations       |
| Data              | Customers, Leads, Sales | Products, Inventory, Finance  |
| Goal              | Increase Revenue        | Improve Efficiency            |
| External/Internal | External (Customers)    | Internal (Business Processes) |

---

## In Data Warehouse Projects

Usually data comes from both CRM and ERP systems.

```text
        CRM System
       (Customers)
            │
            ▼
        ERP System
 (Products, Orders, Finance)
            │
            ▼
       ETL Process
 Extract → Transform → Load
            │
            ▼
      Data Warehouse
            │
            ▼
       Gold Layer
            │
            ▼
        Power BI
        Reports
        Dashboards
```

### Real Example

**CRM Source**

```text
CustomerID = 1001
CustomerName = Nirmala
Gender = Female
City = Bangalore
```

**ERP Source**

```text
CustomerID = 1001
OrderID = 5001
Product = Laptop
Amount = 50000
```

**Gold Layer (Data Warehouse)**

```text
CustomerName | City      | Product | Amount
-------------|-----------|---------|--------
Nirmala      | Bangalore | Laptop  | 50000
```

## 5. Data Warehouse Modeling Approaches
  - **1.Inmon Approach:** Staging →Enterprise Data Warehouse →Data Mart
  - **2.Kimball Approach:** Staging → Data Mart
  - **3.Data Vault:** Staging → Raw Vault → Business Vault → Data Mart
  - **4.Medallion Architecture:** Bronze → Silver →Gold

Here's a clear breakdown of the medallion architecture, which is widely used in modern data lakehouse design.

## 6. What It Is medallion architecture 
The medallion architecture is a layered approach to organizing data as it moves through an ETL/ELT pipeline, progressively refining it through three stages: **Bronze → Silver → Gold**. 

Each layer represents an increasing level of data quality and business readiness.

## a) Bronze Layer (Raw)
This is the landing zone for raw data, captured as-is from source systems.

- **Purpose:** Preserve an unaltered historical record of source data
- **Characteristics:**
  - No transformation (or minimal — just adding metadata like load timestamp, source system, batch ID)
  - Schema matches the source exactly
  - Append-only, immutable
  - Acts as your single source of truth for replay/reprocessing if downstream logic changes
- **Example:** Raw CSV exports from an ERP system, JSON from an API, exact copies of OLTP tables

## b) Silver Layer (Cleansed/Conformed)
Data here is validated, cleaned, and standardized — but still fairly granular (not yet aggregated for specific business questions).

- **Purpose:** Create a trusted, queryable version of the data
- **Characteristics:**
  - Deduplication, null handling, data type corrections
  - Business rules and validation applied
  - Conformed dimensions (e.g., standardizing "USA," "U.S.A," "United States" into one value)
  - Joins across sources to build unified entities (e.g., combining customer data from CRM + billing)
  - Slowly Changing Dimension (SCD) logic often implemented here
- **Example:** A clean `Customers` table with deduplicated records, standardized addresses, validated emails

## c) Gold Layer (Acurated/Business-Level)
This is the consumption layer — optimized for reporting, dashboards, and analytics.

- **Purpose:** Serve specific business use cases
- **Characteristics:**
  - Aggregated, denormalized for performance
  - Often modeled as star schemas (fact and dimension tables)
  - Tailored to specific domains (Sales Gold, Finance Gold, etc.)
  - This is what Power BI, Tableau, or other BI tools connect to
- **Example:** A `Sales_Summary` table aggregated by month, region, and product category, ready for a Power BI dashboard

## Visual Flow
<img width="700" height="250" alt="image" src="https://github.com/user-attachments/assets/c41c8f8a-b572-45c7-9824-1f783b926887" />
<img width="1200" height="500" alt="image" src="https://github.com/user-attachments/assets/591b7da4-fa0c-4ae6-af56-22674bd275f5" />

## 7. Why It Matters for SSIS/Power BI Work
In an SSIS context, this often translates to:
- _**Bronze:**_ A package extracting source data into raw staging tables (minimal transformation)
- _**Silver:**_ Data Flow Tasks doing lookups, deduplication, derived columns, and conforming dimensions
- _**Gold:**_ Aggregation transformations and final loads into your data mart, which Power BI then connects to via Import or DirectQuery

## 8. Naming Conventions Rules
The project uses:

<img width="800" height="533" alt="image" src="https://github.com/user-attachments/assets/d4e3679c-bc39-4cdc-bc59-adb7654c7027" />

## We use _snake_case_ Rule
## Bronze Rules
* All names must start with the source system name, and table names must match their original names without renaming.
* **sourcesystem.entity**
   * _**sourcesystem:**_ Name of the source system (Ex: CRM, ERP).
   * _**entity:**_ Exact table name from the source system.
   * **Example:** **crm_customer_info** → Customer information from the CRM system.

## Silver Rules
* All names must start with the source system name, and table names must match their original names without renaming.
* _**sourcesystem.entity**_
    * _**sourcesystem:**_ Name of the source system (Ex: CRM, ERP).
    * _**entity:**_ Exact table name from the source system.
    * **Example:** _**crm_customer_info**_ → Customer information from the CRM system.

## Gold Rules
* All names must use meaningful, business-aligned names for tables, starting with the category prefix.
* _**category.entity**_
   * _**category:**_ Describes the role of the table, such as dim (dimension) or fact (fact table).
   * _**entity:**_ Descriptive name of the table, aligned with the business domain (e.g., customers, products, sales).
   * **Examples:**
      * _**dim_customers**_ → Dimension table for customer data.
      * _**fact_sales**_ → Fact table containing sales transactions.
   
## Technical Columns
* All technical columns must start with the prefix dwh_, followed by a descriptive name indicating the column's purpose.
* _**dwh.column_name**_
   * _**dwh:**_ Prefix exclusively for system-generated metadata.
   * _**column_name:**_ Descriptive name indicating the column's purpose.
   * **Example:** _**dwh_load_date**_ → System-generated column used to store the date when the record was loaded.

## Stored Procedure
* All stored procedures used for loading data must follow the naming pattern: load_<layer>.
* _**layer:**_ Represents the layer being loaded, such as bronze, silver, or gold.
   * **Example:**
   * _**load_bronze**_ → Stored procedure for loading data into the Bronze layer.
   * _**load_silver**_ → Stored procedure for loading data into the Silver layer.

## 9. Final Flow Diagram of Data Warehouse
<img width="1100" height="870" alt="image" src="https://github.com/user-attachments/assets/8366221e-335a-4461-b445-38f5c3f4879e" />

---

```text

data-warehouse-project/
├── datasets/             # Raw data from ERP and CRM systems.
│
├── docs/                 # Project documentation, architecture diagrams, and outputs.
│   ├── bronze/
│   │   ├── data_flow_bronze.drawio   # Data flow diagram: Source -> Bronze (Draw.io).
│   │   ├── bronze_data_schema.md     # Schema of the bronze layer tables.
│   │   └── bronze_output_examples/   # Example of the data after the bronze layer processing.
│   ├── silver/
│   │   ├── data_cleaning_output/     # Examples of data after cleaning.
│   │   ├── data_flow_silver.drawio   # Data flow diagram: Bronze -> Silver (Draw.io).
│   │   ├── Data_Integration.drawio   # Data integration diagram (Draw.io).
│   │   └── silver_data_schema.md     # Schema of the silver layer tables.
│   ├── gold/
│   │   ├── output/                 # Examples of the data after the gold layer processing.
│   │   ├── data_catalog.md         # Data dictionary for the Gold layer, including field descriptions.
│   │   ├── data_flow_gold.drawio   # Data flow diagram: Silver -> Gold (Draw.io).
│   │   ├── data_models.drawio      # Star schema diagram (Draw.io).
│   │   └── gold_data_schema.md     # Schema of the gold layer tables.
│   └── warehouse/
│       ├── naming_conventions.md      # Naming conventions for tables, columns, etc.
│       ├── data_architecture.drawio   # Overall data warehouse architecture diagram (Draw.io).
│       └── etl.drawio                 # ETL process diagram, showcasing techniques and methods (Draw.io).
│
├── scripts/                             # SQL scripts for ETL and transformations.
│   ├── bronze/
│   │   └── load_raw_data.sql           # Scripts to load data from the 'datasets' directory into the bronze layer.
│   ├── silver/
│   │   └── transform_clean_data.sql    # Scripts to clean and transform the data in the bronze layer.
│   └── gold/
│       ├── create_analytical_views.sql # Scripts to create views for analysis in the gold layer.
│       └── populate_dimensions.sql     # Scripts to populate dimension tables.
│   └── init_database.sql               # Script to create the database and schemas.
│
├── tests/                       # Test scripts and quality control files (e.g., data quality checks).
│   └── data_quality_checks.sql  # SQL scripts for data quality checks.
│
├── report/                      # Analysis scripts and reports.
│   ├── 1_gold_layer_datasets/   # Datasets used for reporting and analysis.
│   ├── 2_eda_scripts/           # Exploratory Data Analysis (EDA) scripts.
│   │   └── basic_eda.ipynb      # Jupyter notebook containing basic EDA.
│   ├── 3_advanced_eda/          # Advanced EDA scripts and analyses.
│   │   └── advanced_eda.ipynb   # Jupyter notebook containing advanced EDA.
│   ├── output/                  # Output from the analysis (e.g., charts, tables).
│   ├── 12_report_customers.sql  # SQL script for the customer report.
│   └── 13_report_products.sql   # SQL script for the product report.
│
├── README.md                   # Project overview, instructions, and report summaries.
├── LICENSE                     # License information.
└── requirements.txt            # Project dependencies (e.g.pgsql libraries).

```

---

# STEP 1

## Silver: PROCEDURE

```sql
/*
===============================================================================
Stored Procedure: Load Silver Layer (Bronze -> Silver)
===============================================================================
Script Purpose:
    This stored procedure performs the ETL (Extract, Transform, Load) process to 
    populate the 'silver' schema tables from the 'bronze' schema.
	Actions Performed:
		- Truncates Silver tables.
		- Inserts transformed and cleansed data from Bronze into Silver tables.
		
Parameters:
    None. 
	  This stored procedure does not accept any parameters or return any values.

Usage Example:
    EXEC Silver.load_silver;
===============================================================================
*/
USE DataWarehouse;
GO

CREATE OR ALTER PROCEDURE silver.load_silver 
AS
BEGIN
    DECLARE @start_time DATETIME, @end_time DATETIME, @batch_start_time DATETIME, @batch_end_time DATETIME; 
    BEGIN TRY
        SET @batch_start_time = GETDATE();
        PRINT '================================================';
        PRINT 'Loading Silver Layer';
        PRINT '================================================';

		PRINT '------------------------------------------------';
		PRINT 'Loading CRM Tables';
		PRINT '------------------------------------------------';

		-- Loading silver.crm_cust_info
        SET @start_time = GETDATE();
		PRINT '>> Truncating Table: silver.crm_cust_info';
		TRUNCATE TABLE silver.crm_cust_info;
		PRINT '>> Inserting Data Into: silver.crm_cust_info';
		INSERT INTO silver.crm_cust_info (
			cst_id, 
			cst_key, 
			cst_firstname, 
			cst_lastname, 
			cst_marital_status, 
			cst_gndr,
			cst_create_date
		)
		SELECT
			cst_id,
			cst_key,
			TRIM(cst_firstname) AS cst_firstname,
			TRIM(cst_lastname) AS cst_lastname,
			CASE 
				WHEN UPPER(TRIM(cst_marital_status)) = 'S' THEN 'Single'
				WHEN UPPER(TRIM(cst_marital_status)) = 'M' THEN 'Married'
				ELSE 'n/a'
			END AS cst_marital_status, -- Normalize marital status values to readable format
			CASE 
				WHEN UPPER(TRIM(cst_gndr)) = 'F' THEN 'Female'
				WHEN UPPER(TRIM(cst_gndr)) = 'M' THEN 'Male'
				ELSE 'n/a'
			END AS cst_gndr, -- Normalize gender values to readable format
			cst_create_date
		FROM (
			SELECT
				*,
				ROW_NUMBER() OVER (PARTITION BY cst_id ORDER BY cst_create_date DESC) AS flag_last
			FROM bronze.crm_cust_info
			WHERE cst_id IS NOT NULL
		) t
		WHERE flag_last = 1; -- Select the most recent record per customer
		SET @end_time = GETDATE();
        PRINT '>> Load Duration: ' + CAST(DATEDIFF(SECOND, @start_time, @end_time) AS NVARCHAR) + ' seconds';
        PRINT '>> -------------';

		-- Loading silver.crm_prd_info
        SET @start_time = GETDATE();
		PRINT '>> Truncating Table: silver.crm_prd_info';
		TRUNCATE TABLE silver.crm_prd_info;
		PRINT '>> Inserting Data Into: silver.crm_prd_info';
		INSERT INTO silver.crm_prd_info (
			prd_id,
			cat_id,
			prd_key,
			prd_nm,
			prd_cost,
			prd_line,
			prd_start_dt,
			prd_end_dt
		)
		SELECT
			prd_id,
			REPLACE(SUBSTRING(prd_key, 1, 5), '-', '_') AS cat_id, -- Extract category ID
			SUBSTRING(prd_key, 7, LEN(prd_key)) AS prd_key,        -- Extract product key
			prd_nm,
			ISNULL(prd_cost, 0) AS prd_cost,
			CASE 
				WHEN UPPER(TRIM(prd_line)) = 'M' THEN 'Mountain'
				WHEN UPPER(TRIM(prd_line)) = 'R' THEN 'Road'
				WHEN UPPER(TRIM(prd_line)) = 'S' THEN 'Other Sales'
				WHEN UPPER(TRIM(prd_line)) = 'T' THEN 'Touring'
				ELSE 'n/a'
			END AS prd_line, -- Map product line codes to descriptive values
			CAST(prd_start_dt AS DATE) AS prd_start_dt,
			CAST(
				LEAD(prd_start_dt) OVER (PARTITION BY prd_key ORDER BY prd_start_dt) - 1 
				AS DATE
			) AS prd_end_dt -- Calculate end date as one day before the next start date
		FROM bronze.crm_prd_info;
        SET @end_time = GETDATE();
        PRINT '>> Load Duration: ' + CAST(DATEDIFF(SECOND, @start_time, @end_time) AS NVARCHAR) + ' seconds';
        PRINT '>> -------------';

        -- Loading crm_sales_details
        SET @start_time = GETDATE();
		PRINT '>> Truncating Table: silver.crm_sales_details';
		TRUNCATE TABLE silver.crm_sales_details;
		PRINT '>> Inserting Data Into: silver.crm_sales_details';
		INSERT INTO silver.crm_sales_details (
			sls_ord_num,
			sls_prd_key,
			sls_cust_id,
			sls_order_dt,
			sls_ship_dt,
			sls_due_dt,
			sls_sales,
			sls_quantity,
			sls_price
		)
		SELECT 
			sls_ord_num,
			sls_prd_key,
			sls_cust_id,
			CASE 
				WHEN sls_order_dt = 0 OR LEN(sls_order_dt) != 8 THEN NULL
				ELSE CAST(CAST(sls_order_dt AS VARCHAR) AS DATE)
			END AS sls_order_dt,
			CASE 
				WHEN sls_ship_dt = 0 OR LEN(sls_ship_dt) != 8 THEN NULL
				ELSE CAST(CAST(sls_ship_dt AS VARCHAR) AS DATE)
			END AS sls_ship_dt,
			CASE 
				WHEN sls_due_dt = 0 OR LEN(sls_due_dt) != 8 THEN NULL
				ELSE CAST(CAST(sls_due_dt AS VARCHAR) AS DATE)
			END AS sls_due_dt,
			CASE 
				WHEN sls_sales IS NULL OR sls_sales <= 0 OR sls_sales != sls_quantity * ABS(sls_price) 
					THEN sls_quantity * ABS(sls_price)
				ELSE sls_sales
			END AS sls_sales, -- Recalculate sales if original value is missing or incorrect
			sls_quantity,
			CASE 
				WHEN sls_price IS NULL OR sls_price <= 0 
					THEN sls_sales / NULLIF(sls_quantity, 0)
				ELSE sls_price  -- Derive price if original value is invalid
			END AS sls_price
		FROM bronze.crm_sales_details;
        SET @end_time = GETDATE();
        PRINT '>> Load Duration: ' + CAST(DATEDIFF(SECOND, @start_time, @end_time) AS NVARCHAR) + ' seconds';
        PRINT '>> -------------';

        -- Loading erp_cust_az12
        SET @start_time = GETDATE();
		PRINT '>> Truncating Table: silver.erp_cust_az12';
		TRUNCATE TABLE silver.erp_cust_az12;
		PRINT '>> Inserting Data Into: silver.erp_cust_az12';
		INSERT INTO silver.erp_cust_az12 (
			cid,
			bdate,
			gen
		)
		SELECT
			CASE
				WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid, 4, LEN(cid)) -- Remove 'NAS' prefix if present
				ELSE cid
			END AS cid, 
			CASE
				WHEN bdate > GETDATE() THEN NULL
				ELSE bdate
			END AS bdate, -- Set future birthdates to NULL
			CASE
				WHEN UPPER(TRIM(gen)) IN ('F', 'FEMALE') THEN 'Female'
				WHEN UPPER(TRIM(gen)) IN ('M', 'MALE') THEN 'Male'
				ELSE 'n/a'
			END AS gen -- Normalize gender values and handle unknown cases
		FROM bronze.erp_cust_az12;
	    SET @end_time = GETDATE();
        PRINT '>> Load Duration: ' + CAST(DATEDIFF(SECOND, @start_time, @end_time) AS NVARCHAR) + ' seconds';
        PRINT '>> -------------';

		PRINT '------------------------------------------------';
		PRINT 'Loading ERP Tables';
		PRINT '------------------------------------------------';

        -- Loading erp_loc_a101
        SET @start_time = GETDATE();
		PRINT '>> Truncating Table: silver.erp_loc_a101';
		TRUNCATE TABLE silver.erp_loc_a101;
		PRINT '>> Inserting Data Into: silver.erp_loc_a101';
		INSERT INTO silver.erp_loc_a101 (
			cid,
			cntry
		)
		SELECT
			REPLACE(cid, '-', '') AS cid, 
			CASE
				WHEN TRIM(cntry) = 'DE' THEN 'Germany'
				WHEN TRIM(cntry) IN ('US', 'USA') THEN 'United States'
				WHEN TRIM(cntry) = '' OR cntry IS NULL THEN 'n/a'
				ELSE TRIM(cntry)
			END AS cntry -- Normalize and Handle missing or blank country codes
		FROM bronze.erp_loc_a101;
	    SET @end_time = GETDATE();
        PRINT '>> Load Duration: ' + CAST(DATEDIFF(SECOND, @start_time, @end_time) AS NVARCHAR) + ' seconds';
        PRINT '>> -------------';
		
		-- Loading erp_px_cat_g1v2
		SET @start_time = GETDATE();
		PRINT '>> Truncating Table: silver.erp_px_cat_g1v2';
		TRUNCATE TABLE silver.erp_px_cat_g1v2;
		PRINT '>> Inserting Data Into: silver.erp_px_cat_g1v2';
		INSERT INTO silver.erp_px_cat_g1v2 (
			id,
			cat,
			subcat,
			maintenance
		)
		SELECT
			id,
			cat,
			subcat,
			maintenance
		FROM bronze.erp_px_cat_g1v2;
		SET @end_time = GETDATE();
		PRINT '>> Load Duration: ' + CAST(DATEDIFF(SECOND, @start_time, @end_time) AS NVARCHAR) + ' seconds';
        PRINT '>> -------------';

		SET @batch_end_time = GETDATE();
		PRINT '=========================================='
		PRINT 'Loading Silver Layer is Completed';
        PRINT '   - Total Load Duration: ' + CAST(DATEDIFF(SECOND, @batch_start_time, @batch_end_time) AS NVARCHAR) + ' seconds';
		PRINT '=========================================='
		
	END TRY
	BEGIN CATCH
		PRINT '=========================================='
		PRINT 'ERROR OCCURED DURING LOADING BRONZE LAYER'
		PRINT 'Error Message' + ERROR_MESSAGE();
		PRINT 'Error Message' + CAST (ERROR_NUMBER() AS NVARCHAR);
		PRINT 'Error Message' + CAST (ERROR_STATE() AS NVARCHAR);
		PRINT '=========================================='
	END CATCH
END

-- Execute silver.load_silver
```

# STEP 2

## Bronze: PROCEDURE

```sql
/*
===============================================================================
Stored Procedure: Load Bronze Layer (Source -> Bronze)
===============================================================================
Script Purpose:
    This stored procedure loads data into the 'bronze' schema from external CSV files. 
    It performs the following actions:
    - Truncates the bronze tables before loading data.
    - Uses the `BULK INSERT` command to load data from csv Files to bronze tables.

Parameters:
    None. 
	  This stored procedure does not accept any parameters or return any values.

Usage Example:
    EXEC bronze.load_bronze;
===============================================================================
*/
CREATE OR ALTER PROCEDURE bronze.load_bronze AS
BEGIN
	DECLARE @start_time DATETIME, @end_time DATETIME, @batch_start_time DATETIME, @batch_end_time DATETIME; 
	BEGIN TRY
		SET @batch_start_time = GETDATE();
		PRINT '================================================';
		PRINT 'Loading Bronze Layer';
		PRINT '================================================';

		PRINT '------------------------------------------------';
		PRINT 'Loading CRM Tables';
		PRINT '------------------------------------------------';

		SET @start_time = GETDATE();
		PRINT '>> Truncating Table: bronze.crm_cust_info';
		TRUNCATE TABLE bronze.crm_cust_info;
		PRINT '>> Inserting Data Into: bronze.crm_cust_info';
		BULK INSERT bronze.crm_cust_info
		FROM 'C:\sql\dwh_project\datasets\source_crm\cust_info.csv'
		WITH (
			FIRSTROW = 2,
			FIELDTERMINATOR = ',',
			TABLOCK
		);
		SET @end_time = GETDATE();
		PRINT '>> Load Duration: ' + CAST(DATEDIFF(second, @start_time, @end_time) AS NVARCHAR) + ' seconds';
		PRINT '>> -------------';

        SET @start_time = GETDATE();
		PRINT '>> Truncating Table: bronze.crm_prd_info';
		TRUNCATE TABLE bronze.crm_prd_info;

		PRINT '>> Inserting Data Into: bronze.crm_prd_info';
		BULK INSERT bronze.crm_prd_info
		FROM 'C:\sql\dwh_project\datasets\source_crm\prd_info.csv'
		WITH (
			FIRSTROW = 2,
			FIELDTERMINATOR = ',',
			TABLOCK
		);
		SET @end_time = GETDATE();
		PRINT '>> Load Duration: ' + CAST(DATEDIFF(second, @start_time, @end_time) AS NVARCHAR) + ' seconds';
		PRINT '>> -------------';

        SET @start_time = GETDATE();
		PRINT '>> Truncating Table: bronze.crm_sales_details';
		TRUNCATE TABLE bronze.crm_sales_details;
		PRINT '>> Inserting Data Into: bronze.crm_sales_details';
		BULK INSERT bronze.crm_sales_details
		FROM 'C:\sql\dwh_project\datasets\source_crm\sales_details.csv'
		WITH (
			FIRSTROW = 2,
			FIELDTERMINATOR = ',',
			TABLOCK
		);
		SET @end_time = GETDATE();
		PRINT '>> Load Duration: ' + CAST(DATEDIFF(second, @start_time, @end_time) AS NVARCHAR) + ' seconds';
		PRINT '>> -------------';

		PRINT '------------------------------------------------';
		PRINT 'Loading ERP Tables';
		PRINT '------------------------------------------------';
		
		SET @start_time = GETDATE();
		PRINT '>> Truncating Table: bronze.erp_loc_a101';
		TRUNCATE TABLE bronze.erp_loc_a101;
		PRINT '>> Inserting Data Into: bronze.erp_loc_a101';
		BULK INSERT bronze.erp_loc_a101
		FROM 'C:\sql\dwh_project\datasets\source_erp\loc_a101.csv'
		WITH (
			FIRSTROW = 2,
			FIELDTERMINATOR = ',',
			TABLOCK
		);
		SET @end_time = GETDATE();
		PRINT '>> Load Duration: ' + CAST(DATEDIFF(second, @start_time, @end_time) AS NVARCHAR) + ' seconds';
		PRINT '>> -------------';

		SET @start_time = GETDATE();
		PRINT '>> Truncating Table: bronze.erp_cust_az12';
		TRUNCATE TABLE bronze.erp_cust_az12;
		PRINT '>> Inserting Data Into: bronze.erp_cust_az12';
		BULK INSERT bronze.erp_cust_az12
		FROM 'C:\sql\dwh_project\datasets\source_erp\cust_az12.csv'
		WITH (
			FIRSTROW = 2,
			FIELDTERMINATOR = ',',
			TABLOCK
		);
		SET @end_time = GETDATE();
		PRINT '>> Load Duration: ' + CAST(DATEDIFF(second, @start_time, @end_time) AS NVARCHAR) + ' seconds';
		PRINT '>> -------------';

		SET @start_time = GETDATE();
		PRINT '>> Truncating Table: bronze.erp_px_cat_g1v2';
		TRUNCATE TABLE bronze.erp_px_cat_g1v2;
		PRINT '>> Inserting Data Into: bronze.erp_px_cat_g1v2';
		BULK INSERT bronze.erp_px_cat_g1v2
		FROM 'C:\sql\dwh_project\datasets\source_erp\px_cat_g1v2.csv'
		WITH (
			FIRSTROW = 2,
			FIELDTERMINATOR = ',',
			TABLOCK
		);
		SET @end_time = GETDATE();
		PRINT '>> Load Duration: ' + CAST(DATEDIFF(second, @start_time, @end_time) AS NVARCHAR) + ' seconds';
		PRINT '>> -------------';

		SET @batch_end_time = GETDATE();
		PRINT '=========================================='
		PRINT 'Loading Bronze Layer is Completed';
        PRINT '   - Total Load Duration: ' + CAST(DATEDIFF(SECOND, @batch_start_time, @batch_end_time) AS NVARCHAR) + ' seconds';
		PRINT '=========================================='
	END TRY
	BEGIN CATCH
		PRINT '=========================================='
		PRINT 'ERROR OCCURED DURING LOADING BRONZE LAYER'
		PRINT 'Error Message' + ERROR_MESSAGE();
		PRINT 'Error Message' + CAST (ERROR_NUMBER() AS NVARCHAR);
		PRINT 'Error Message' + CAST (ERROR_STATE() AS NVARCHAR);
		PRINT '=========================================='
	END CATCH
END

```


# STEP 3

## Gold: View

```sql
/*
===============================================================================
DDL Script: Create Gold Views
===============================================================================
Script Purpose:
    This script creates views for the Gold layer in the data warehouse. 
    The Gold layer represents the final dimension and fact tables (Star Schema)

    Each view performs transformations and combines data from the Silver layer 
    to produce a clean, enriched, and business-ready dataset.

Usage:
    - These views can be queried directly for analytics and reporting.
===============================================================================
*/

-- =============================================================================
-- Create Dimension: gold.dim_customers
-- =============================================================================
IF OBJECT_ID('gold.dim_customers', 'V') IS NOT NULL
    DROP VIEW gold.dim_customers;
GO

CREATE VIEW gold.dim_customers AS
SELECT
    ROW_NUMBER() OVER (ORDER BY cst_id) AS customer_key, -- Surrogate key
    ci.cst_id                          AS customer_id,
    ci.cst_key                         AS customer_number,
    ci.cst_firstname                   AS first_name,
    ci.cst_lastname                    AS last_name,
    la.cntry                           AS country,
    ci.cst_marital_status              AS marital_status,
    CASE 
        WHEN ci.cst_gndr != 'n/a' THEN ci.cst_gndr -- CRM is the primary source for gender
        ELSE COALESCE(ca.gen, 'n/a')  			   -- Fallback to ERP data
    END                                AS gender,
    ca.bdate                           AS birthdate,
    ci.cst_create_date                 AS create_date
FROM silver.crm_cust_info ci
LEFT JOIN silver.erp_cust_az12 ca
    ON ci.cst_key = ca.cid
LEFT JOIN silver.erp_loc_a101 la
    ON ci.cst_key = la.cid;
GO

-- =============================================================================
-- Create Dimension: gold.dim_products
-- =============================================================================
IF OBJECT_ID('gold.dim_products', 'V') IS NOT NULL
    DROP VIEW gold.dim_products;
GO

CREATE VIEW gold.dim_products AS
SELECT
    ROW_NUMBER() OVER (ORDER BY pn.prd_start_dt, pn.prd_key) AS product_key, -- Surrogate key
    pn.prd_id       AS product_id,
    pn.prd_key      AS product_number,
    pn.prd_nm       AS product_name,
    pn.cat_id       AS category_id,
    pc.cat          AS category,
    pc.subcat       AS subcategory,
    pc.maintenance  AS maintenance,
    pn.prd_cost     AS cost,
    pn.prd_line     AS product_line,
    pn.prd_start_dt AS start_date
FROM silver.crm_prd_info pn
LEFT JOIN silver.erp_px_cat_g1v2 pc
    ON pn.cat_id = pc.id
WHERE pn.prd_end_dt IS NULL; -- Filter out all historical data
GO

-- =============================================================================
-- Create Fact Table: gold.fact_sales
-- =============================================================================
IF OBJECT_ID('gold.fact_sales', 'V') IS NOT NULL
    DROP VIEW gold.fact_sales;
GO

CREATE VIEW gold.fact_sales AS
SELECT
    sd.sls_ord_num  AS order_number,
    pr.product_key  AS product_key,
    cu.customer_key AS customer_key,
    sd.sls_order_dt AS order_date,
    sd.sls_ship_dt  AS shipping_date,
    sd.sls_due_dt   AS due_date,
    sd.sls_sales    AS sales_amount,
    sd.sls_quantity AS quantity,
    sd.sls_price    AS price
FROM silver.crm_sales_details sd
LEFT JOIN gold.dim_products pr
    ON sd.sls_prd_key = pr.product_number
LEFT JOIN gold.dim_customers cu
    ON sd.sls_cust_id = cu.customer_id;
GO


```

---
