# 🧠 Data Engineering Project – Online Retail Dataset

## 📌 Overview
This end-to-end Data Engineering project demonstrates how to build a robust BI system using the **Online Retail Dataset**. The solution integrates **ETL (SSIS)**, **Data Warehousing (SQL Server / SSMS)**, **OLAP Cube (SSAS)**, and **Business Intelligence Visualization** using **Excel and Power BI**.

## 📁 Dataset Details
- **Source**: Kaggle – [Online Retail Dataset](https://www.kaggle.com/datasets/lakshmi25npathi/online-retail-dataset)
- **Records**: ~500,000
- **Fields**: InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country

> Additional fields such as **Category**, **Department**, and **Region** were derived/added during transformation.

## 🧱 Project Architecture

```plaintext
Raw Data (CSV, TXT, SQL)
    ⬇ (SSIS ETL)
Staging Tables (SQL Server)
    ⬇ (SSIS Transformation)
Data Warehouse (Star Schema - Fact & Dimensions)
    ⬇ (SSAS Cube)
OLAP Reports (Excel, Power BI)

## 🔧 Tools & Technologies

| Tool/Tech                              | Purpose                                 |
| -------------------------------------- | --------------------------------------- |
| SQL Server (SSMS)                      | Hosting Staging & Data Warehouse tables |
| SSIS (SQL Server Integration Services) | ETL - Extract, Transform, Load          |
| SSAS (SQL Server Analysis Services)    | Building OLAP Cube                      |
| Excel + PowerPivot                     | Cube-connected OLAP reporting           |
| Power BI                               | Interactive Dashboards                  |
| Visual Studio                          | Development of SSIS & SSAS packages     |

## ⚙️ ETL with SSIS

### 📌 Data Sources:

* `OnlineRetail.sql` – Raw transactional data
* `products.txt` – Product metadata
* `customers.csv` – Customer-region mapping

### 🚧 SSIS ETL Flow:

1. **Extract**: Load data from CSV, TXT, and SQL sources.
2. **Clean**:

   * Remove nulls, invalid values
   * Format dates, fix missing values
   * Filter out negative quantities and prices
3. **Transform**:

   * Create calculated columns like `Sales = Quantity × UnitPrice`
   * Create surrogate keys for dimensions
   * Map Region from Country using lookups
4. **Load**:

   * Load into **Staging Tables** in SQL Server
   * Then insert into **Data Warehouse tables**

## 🗃️ Data Warehouse Design (Star Schema)

### 🔸 Fact Table

* **FactSales**:

  * InvoiceNo, DateKey, ProductKey, CustomerKey, Quantity, UnitPrice, TotalSales

### 🔹 Dimension Tables

* **DimProduct**: Product metadata (Category, Department)
* **DimCustomer**: Includes Region and SCD history
* **DimDate**: Date dimension (Year, Quarter, Month, Day)

### ✅ Features:

* Surrogate keys
* Cleaned, conformed dimensions
* Implemented Slowly Changing Dimension (Type 2) on `DimCustomer`

## 📊 OLAP Cube with SSAS

### SSAS Cube Configuration

* **Measures**:

  * Total Sales
  * Quantity Sold
  * Average Unit Price

* **Dimensions**:
  * Date (Year → Quarter → Month)
  * Product
  * Customer

### Cube Features

* Hierarchies (Time-based, Product Department → Category)
* KPIs (e.g., Revenue per Region)
* Drill-down / Roll-up
* Aggregations and partitions for performance

## 📈 Excel OLAP Reporting

* Connected Excel to **SSAS cube** using PowerPivot.
* Created PivotTables and charts with:

  * Sales by Region and Category
  * Yearly and Monthly Sales Trends
  * Drill-down and Slicers (e.g., by Department, Quarter)

## 📊 Power BI Dashboards

### Reports Built:

1. **Sales Overview**:

   * Total Sales by Region and Product Category
   * Bar and Pie Charts
2. **Cascading Slicers**:

   * Filter by Country → Product → Year
3. **Drill-Down Reports**:

   * Navigate from Year → Quarter → Month
4. **Drill-Through Reports**:

   * Select a region and jump to detailed product/customer view

### Features:

* Interactive navigation
* Responsive filters
* KPIs and cards
* Tooltips and bookmarks

## 📂 Folder Structure

```plaintext
data-engineering-online-retail/
├── SSIS_Packages/                # ETL packages (.dtsx)
├── SSAS_Cube_Project/            # OLAP cube files
├── SQL_Scripts/                  # Staging & DWH schema + population scripts
├── PowerBI_Reports/              # .pbix files
├── Excel_OLAP_Reports/           # .xlsx with PivotTables
├── Documentation/
│   └── Project_Report.pdf        # Full explanation of pipeline
├── Screenshots/
│   ├── SSIS_ETL_Flow.png
│   ├── SSAS_Cube_Config.png
│   └── PowerBI_Dashboard.png
└── README.md                     # This file
```
## 📊 Sample Queries (SQL Server)

```sql
-- Example: Create Fact Table
SELECT 
    s.InvoiceNo,
    d.DateKey,
    p.ProductKey,
    c.CustomerKey,
    s.Quantity,
    s.UnitPrice,
    (s.Quantity * s.UnitPrice) AS TotalSales
INTO FactSales
FROM StagingSales s
JOIN DimDate d ON CAST(s.InvoiceDate AS DATE) = d.FullDate
JOIN DimProduct p ON s.ProductCode = p.ProductCode
JOIN DimCustomer c ON s.CustomerID = c.CustomerID;
```
## 🎯 Outcomes & Learnings

* Built a fully operational BI pipeline using Microsoft Stack
* Implemented real-world data cleansing and transformation in SSIS
* Designed and deployed a performant OLAP cube in SSAS
* Created powerful insights with Excel + Power BI
* Hands-on practice with DW concepts like **star schema**, **SCD**, and **cube design**
