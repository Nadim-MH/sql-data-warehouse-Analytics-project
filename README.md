# 🏢 Enterprise SQL Data Warehouse (Medallion Architecture)

![SQL Server](https://img.shields.io/badge/SQL_Server-CC2927?style=for-the-badge&logo=microsoft-sql-server&logoColor=white)
![Data Architecture](https://img.shields.io/badge/Architecture-Medallion-FFB000?style=for-the-badge)
![Data Engineering](https://img.shields.io/badge/Data_Engineering-End_to_End-0078D4?style=for-the-badge)

## 📌 Project Overview
This project is an end-to-end **Data Warehouse** built from scratch using **Microsoft SQL Server**. It integrates fragmented data from two distinct source systems (**CRM** and **ERP**) into a centralized repository using the industry-standard **Medallion Architecture** (Bronze, Silver, Gold). 

The goal of this project is to transform raw, inconsistent transactional data into a highly structured, business-ready **Star Schema** to empower BI tools and data analysts.

---

## 🏗️ Architecture Design (Medallion Architecture)

The data pipeline follows a strict 3-tier architecture:

### 🥉 1. Bronze Layer (Raw Data)
- Acts as the landing zone for raw data extracted from source systems.
- Data is stored exactly as it arrives without any transformations.
- Loaded using `BULK INSERT` from CSV files.
- **Sources Integrated:**
  - `crm_sales_details`, `crm_cust_info`, `crm_prd_info` (CRM System)
  - `erp_px_cat_g1v2`, `erp_cust_az12`, `erp_loc_a101` (ERP System)

### 🥈 2. Silver Layer (Cleansed & Conformed Data)
- Data is extracted from the Bronze layer, cleansed, and standardized.
- **Transformations applied:**
  - Handling `NULL` values and standardizing data formats (e.g., mapping marital status and gender).
  - Normalizing text fields (e.g., removing prefixes, handling unwanted spaces).
  - Resolving schema mismatches between CRM and ERP data.
  - Recalculating missing or invalid metrics (e.g., Sales = Quantity * Price).
  - Deduplication and handling historical data boundaries (Start/End dates).
- Automated via Stored Procedures (`EXEC silver.load_silver`).
- Validated with robust quality checks.

### 🥇 3. Gold Layer (Business & Reporting Data)
- The final presentation layer optimized for reporting and analytics.
- Modeled using **Star Schema** (Dimension and Fact tables):
  - `dim_customers`: Unified customer profiles combining CRM and ERP demographics.
  - `dim_products`: Unified product hierarchy combining CRM product details with ERP category data.
  - `fact_sales`: Transactional sales measures.
- Built using SQL `VIEWS` to provide real-time access to the cleansed Silver data.
- Includes advanced business views (`report_customers`, `report_products`) to calculate KPIs like Customer Lifetime Value (CLV), Recency, and Average Order Value.

---

## 📂 Project Structure

```text
📦 sql-data-warehouse-project
 ┣ 📂 scripts
 ┃ ┣ 📂 bronze          # DDL and ingestion scripts (BULK INSERT)
 ┃ ┣ 📂 silver          # Cleansing logic and ETL Stored Procedures
 ┃ ┗ 📂 gold            # Star schema views (Dimensions, Facts, and Reports)
 ┣ 📂 tests
 ┃ ┣ 📜 quality_checks_silver.sql  # SQL scripts for data standardizing & anomaly detection
 ┃ ┗ 📜 quality_checks_gold.sql    # SQL scripts to validate referential integrity
 ┣ 📂 diagrams
 ┃ ┗ 📜 ER_Diagram.png             # Entity-Relationship diagram mapping CRM/ERP
 ┗ 📜 README.md                    # Project documentation
```

---

## ⚙️ Key Technologies & Skills Demonstrated
- **Database:** Microsoft SQL Server (T-SQL)
- **Data Modeling:** Star Schema, Dimensional Modeling, Medallion Architecture.
- **Data Integration:** ETL/ELT pipelines using Stored Procedures and `BULK INSERT`.
- **Data Quality:** Automated QA scripts to test referential integrity, null constraints, and business logic.
- **Analytics:** Window functions and complex aggregations for KPI calculation.

---

## 🚀 How to Run the Project

1. **Database Setup:** 
   Run the setup script to create the `DataWarehouse` database and schemas (`bronze`, `silver`, `gold`).
2. **Load Bronze Data:** 
   Run the DDL script to create bronze tables, then execute `EXEC bronze.load_bronze` to bulk load data from CSV files.
3. **Execute Silver Pipeline:** 
   Run the DDL script for silver tables, then execute `EXEC silver.load_silver` to cleanse and transform the data.
4. **Deploy Gold Views:** 
   Execute the scripts in the `gold` folder to create the dimensional models and reporting views.
5. **Run Tests:** 
   Verify data integrity by running the scripts in the `tests/` directory.

---

## 🔮 Future Enhancements (Roadmap)
- [ ] **CI/CD Pipeline:** Implement GitHub Actions to automate SQL Linting and deployment of Stored Procedures/Views.
- [ ] **Data Orchestration:** Schedule pipeline execution using tools like Apache Airflow or SQL Server Agent.
- [ ] **Cloud Migration:** Migrate the architecture to **Microsoft Fabric / Azure Synapse Analytics** utilizing PySpark.
