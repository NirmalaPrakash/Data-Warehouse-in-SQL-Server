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

## c) Gold Layer (Curated/Business-Level)
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
