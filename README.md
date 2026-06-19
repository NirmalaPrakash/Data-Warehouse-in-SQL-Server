# Data-Warehouse-in-SQL-Server
A **Data Warehouse** serves as a centralized, integrated, and reliable source of data for reporting and analytics. By implementing ETL processes, proper architecture, data modeling, and governance, organizations can transform raw data into meaningful business insights efficiently and accurately.

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
    -	High risk of human errors.
    -	Difficult to integrate multiple data sources.
    -	Poor scalability for large datasets.

## 3. Data Warehouse Architecture Approaches
      **a) Data Warehouse**
          -	Structured data.
          -	Reporting-focused.
      **b) Data Lake**
          -	Structured and unstructured data.
          -	Supports advanced analytics.
      **c) Data Lakehouse**
          -	Combines Data Warehouse and Data Lake benefits.
      **d) Data Mesh**
          -	Decentralized data ownership across business domains.

## 4. Data Warehouse Modeling Approaches
      - **1.Inmon Approach:** Staging → Enterprise Data Warehouse → Data Mart
      - **2.Kimball Approach:** Staging → Data Mart
      - **3.Data Vault: Staging → Raw Vault → Business Vault → Data Mart
      - **4.Medallion Architecture: Bronze → Silver → Gold

Here's a clear breakdown of the medallion architecture, which is widely used in modern data lakehouse design.

## What It Is medallion architecture 
The medallion architecture is a layered approach to organizing data as it moves through an ETL/ELT pipeline, progressively refining it through three stages: **Bronze → Silver → Gold**. 

Each layer represents an increasing level of data quality and business readiness.

## Bronze Layer (Raw)
This is the landing zone for raw data, captured as-is from source systems.

- **Purpose:** Preserve an unaltered historical record of source data
- **Characteristics:**
  - No transformation (or minimal — just adding metadata like load timestamp, source system, batch ID)
  - Schema matches the source exactly
  - Append-only, immutable
  - Acts as your single source of truth for replay/reprocessing if downstream logic changes
- **Example:** Raw CSV exports from an ERP system, JSON from an API, exact copies of OLTP tables

## Silver Layer (Cleansed/Conformed)
Data here is validated, cleaned, and standardized — but still fairly granular (not yet aggregated for specific business questions).

- **Purpose:** Create a trusted, queryable version of the data
- **Characteristics:**
  - Deduplication, null handling, data type corrections
  - Business rules and validation applied
  - Conformed dimensions (e.g., standardizing "USA," "U.S.A," "United States" into one value)
  - Joins across sources to build unified entities (e.g., combining customer data from CRM + billing)
  - Slowly Changing Dimension (SCD) logic often implemented here
- **Example:** A clean `Customers` table with deduplicated records, standardized addresses, validated emails

## Gold Layer (Curated/Business-Level)
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

<img width="1202" height="500" alt="image" src="https://github.com/user-attachments/assets/591b7da4-fa0c-4ae6-af56-22674bd275f5" />

## Why It Matters for SSIS/Power BI Work
In an SSIS context, this often translates to:
- **Bronze:** A package extracting source data into raw staging tables (minimal transformation)
- **Silver:** Data Flow Tasks doing lookups, deduplication, derived columns, and conforming dimensions
- **Gold:** Aggregation transformations and final loads into your data mart, which Power BI then connects to via Import or DirectQuery

## Bronze Rules
* All names must start with the source system name, and table names must match their original names without renaming.
* <sourcesystem>_
* <sourcesystem> : Name of the source system (e.g., crm, erp).
   * : Exact table name from the source system.
   * Example: crm_customer_info → Customer information from the CRM system.

## Silver Rules
* All names must start with the source system name, and table names must match their original names without renaming.
* <sourcesystem>_
* <sourcesystem> : Name of the source system (e.g., crm, erp).
   * : Exact table name from the source system.
   * Example: crm_customer_info → Customer information from the CRM system.

## Gold Rules
* All names must use meaningful, business-aligned names for tables, starting with the category prefix.
* <category>_
* <category> : Describes the role of the table, such as dim (dimension) or fact (fact table).
   * : Descriptive name of the table, aligned with the business domain (e.g., customers, products, sales).
   * Examples:
   * dim_customers → Dimension table for customer data.
      * fact_sales → Fact table containing sales transactions.
   
## Technical Columns
* All technical columns must start with the prefix dwh_, followed by a descriptive name indicating the column's purpose.
* dwh_<column_name>
* dwh : Prefix exclusively for system-generated metadata.
   * <column_name> : Descriptive name indicating the column's purpose.
   * Example: dwh_load_date → System-generated column used to store the date when the record was loaded.

## Stored Procedure
* All stored procedures used for loading data must follow the naming pattern: load_<layer>.
* <layer> : Represents the layer being loaded, such as bronze, silver, or gold.
   * Example:
   * load_bronze → Stored procedure for loading data into the Bronze layer.
      * load_silver → Stored procedure for loading data into the Silver layer.
   
<img width="1154" height="870" alt="image" src="https://github.com/user-attachments/assets/8366221e-335a-4461-b445-38f5c3f4879e" />

