# data-warehouse-project
Building a modern datawarehouse using sql-server, including ETL processes, data modelling and analytics.

# 📦 Data Warehouse Project

A modern data warehouse built on **SQL Server**, following the **Medallion Architecture** (Bronze → Silver → Gold). This project takes raw CRM and ERP data and transforms it into a clean, analytics-ready star schema.

## 🏗️ Architecture

The project moves data through three layers:

```
Source CSVs  →  🥉 Bronze  →  🥈 Silver  →  🥇 Gold
 (CRM/ERP)      (raw load)    (cleaned)    (star schema)
```

- **Bronze** — Raw data loaded as-is from CSV files using `BULK INSERT`.
- **Silver** — Cleaned, standardized, and enriched data.
- **Gold** — Business-ready views modeled as a star schema (dimensions + fact table), ready for reporting and analytics.

## 📁 Project Structure

```
data-warehouse-project/
│
├── datasets/                  # Source CSV files
│   ├── crm/                   # CRM data (customers, products, sales)
│   └── erp/                   # ERP data (location, customer, category)
│
├── scripts/
│   ├── create-database/       # Database & schema creation
│   ├── bronze/                # Bronze layer DDL + load procedure
│   ├── silver/                # Silver layer DDL + load procedure
│   └── gold/                  # Gold layer views (star schema)
│
└── tests/                     # Data quality checks for Silver & Gold layers
```

## ⭐ Gold Layer (Star Schema)

| Object | Type | Description |
|---|---|---|
| `gold.dim_customers` | Dimension | Customer details merged from CRM and ERP sources |
| `gold.dim_products` | Dimension | Active product catalog with category info |
| `gold.fact_sales` | Fact | Sales transactions linked to customers and products |

## 🚀 Getting Started

### Prerequisites
- SQL Server (e.g., SQL Server Express or Developer Edition)
- SQL Server Management Studio (SSMS) or another SQL client

### Setup

1. **Create the database and schemas**
   Run `scripts/create-database/initializing database.sql`.
   This creates the `DataWarehouse` database with `bronze`, `silver`, and `gold` schemas.

2. **Build and load the Bronze layer**
   - Run `scripts/bronze/ddl.sql` to create the bronze tables.
   - Run `scripts/bronze/stored procedure.sql` to create the load procedure.
   - Execute it:
     ```sql
     EXEC bronze.load_bronze;
     ```
   > ⚠️ Update the file paths inside the stored procedure to point to your local copy of the `datasets` folder before running.

3. **Build and load the Silver layer**
   - Run `scripts/silver/ddl.sql` to create the silver tables.
   - Run `scripts/silver/stored procedure.sql` to create the load/transform procedure, then execute it the same way.

4. **Create the Gold layer views**
   Run `scripts/gold/ddl.sql` to create the final star-schema views.

5. **Run data quality checks**
   Use `tests/quality_silver.sql` and `tests/quality_gold.sql` to validate the data at each stage.

## 📊 Querying the Warehouse

Once the Gold layer is built, you can query it directly for analytics:

```sql
SELECT *
FROM gold.fact_sales fs
JOIN gold.dim_customers dc ON fs.customer_key = dc.customer_key
JOIN gold.dim_products dp ON fs.product_key = dp.product_key;
```

## 🛠️ Tech Stack

- **Database:** SQL Server
- **ETL:** T-SQL (stored procedures, `BULK INSERT`)
- **Modeling:** Star schema (Kimball-style dimensional modeling)

## 📄 License

No license specified yet. Add one if you plan to share or open-source this project.
