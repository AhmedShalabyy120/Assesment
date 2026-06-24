# Sales & Forecast Analytics Platform

## Project Overview

This project demonstrates an end-to-end analytics solution built on Microsoft Fabric using a Medallion Architecture (Bronze, Silver, Gold). The solution ingests raw JSON files from Azure Data Lake Storage, transforms and models the data using PySpark and Pandas, and delivers business insights through Power BI semantic models and dashboards.

The project was developed to showcase modern Data Engineering and Analytics Engineering practices, including ETL orchestration, data modeling, dimensional design, forecasting analysis, and Power BI reporting.

---

## Business Objective

The objective of this project is to:

* Ingest raw sales and forecast data from Azure Data Lake Storage.
* Build a scalable Medallion Architecture using Microsoft Fabric.
* Clean, standardize, and transform raw data into analytical datasets.
* Create a Star Schema model for reporting and analysis.
* Compare actual sales performance against forecasted values.
* Deliver interactive Power BI dashboards for business users.

---

## Technology Stack

* Microsoft Fabric
* OneLake
* Lakehouse
* Fabric Data Pipelines
* PySpark
* Pandas
* Delta Tables
* Power BI
* DAX
* Azure Data Lake Storage Gen2

---

## Solution Architecture

```text
Azure Data Lake Storage
        │
        ▼
Fabric Pipeline
        │
        ▼
Lakehouse Files (Bronze)
        │
        ▼
Bronze Delta Tables
        │
        ▼
Silver Layer
(Data Cleaning & Standardization)
        │
        ▼
Gold Layer
(Fact & Dimension Modeling)
        │
        ▼
Power BI Semantic Model
        │
        ▼
Interactive Dashboard
```

---

## Data Ingestion

Raw JSON files are stored in Azure Data Lake Storage Gen2.

A Microsoft Fabric Pipeline performs the following operations:

1. Reads source files from Azure Data Lake Storage.
2. Retrieves metadata dynamically.
3. Iterates through files using a ForEach activity.
4. Copies JSON files into the Lakehouse Bronze Files layer.
5. Stores raw files without transformation.

Pipeline Name:

```text
pl_orion
```

Key Activities:

* Get Metadata
* ForEach
* Copy Activity

---

## Medallion Architecture

### Bronze Layer

Purpose:

Store raw source data exactly as received.

Input:

* Sales.json
* Forecast.json

Activities:

* Load raw JSON files from Lakehouse Files.
* Convert JSON structures into Bronze Delta tables.
* Preserve source data with minimal transformation.

Output:

* bronze_sales
* bronze_forecast

---

### Silver Layer

Purpose:

Clean and standardize source data.

Activities:

* Data profiling and exploration.
* Missing value analysis.
* Duplicate detection and removal.
* Data type corrections.
* Column name standardization.
* Whitespace trimming.
* Data quality validation.

Tools Used:

* PySpark
* Pandas

Output:

* Cleaned Sales dataset.

---

### Gold Layer

Purpose:

Create analytical datasets optimized for reporting.

Activities:

* Build Fact and Dimension tables.
* Create Star Schema model.
* Prepare Actual vs Forecast reporting structure.

Output Tables:

#### Fact Table

**gold fact_sales**

Columns:

* CustomerKey
* ProductKey
* GeographyKey
* DateKey
* OrderDate
* Quantity
* Net_Price
* TotalRevenue

#### Dimension Tables

**gold dim_customers**

* Customer_Code
* CustomerKey
* Name
* Education
* Occupation

**gold dim_product**

* ProductKey
* Product_Name
* Brand
* Category
* Subcategory

**gold dim_geography**

* GeographyKey
* City
* State
* CountryRegion
* Continent

**gold dim_date**

* DateKey
* Date
* Year
* Month
* Quarter
* MonthName
* Weekday
* MonthShortcutName
* WeekdayShortcut
* Quater_Name

#### Forecast Table

**gold forecast**

* Brand
* CountryRegion
* Forecast
* Year

---

## Power BI Semantic Model

Two semantic models were created:

### Import Mode (Final Solution)

Selected as the primary reporting model due to performance and offline usability.

### Direct Lake Model

Created for evaluation and testing purposes.

---

## Data Model

The final semantic model follows a Star Schema design.

### Core Relationships

Fact Table:

* gold fact_sales

Dimensions:

* gold dim_customers
* gold dim_product
* gold dim_geography
* gold dim_date

Supporting Dimensions:

* Country
* Year

Additional Analytical Table:

* gold forecast

This design enables:

* Sales analysis
* Forecast analysis
* Actual vs Forecast comparison
* Country-level reporting
* Year-level reporting

---

## DAX Measures

### KPI Measures

* Total_Sales
* Countries
* Cities
* # Products
* Total Products
* # Customers
* Total Customers


### Sales Analytics

* Total_Quantity
* Total_Forecast
* Variance
* Variance %

### Year-over-Year Analysis

* Sales 2008
* Sales 2009
* Sales_LY
* YoY Growth %
* YoY Growth % Color

### Product Analysis

* Product Share %

### Customer Intelligence

* Top Customer Info
* Top Customer Trend

---

## Key Assumptions

1. Forecast data is provided at Brand + Country + Year grain.
2. Sales data is stored at transaction level.
3. Country and Year helper dimensions are used to enable common filtering between Actual and Forecast datasets.
4. Import mode is used for the final reporting solution.
5. All relationships are active and validated before reporting.

---

## Repository Contents

```text
├── Pipeline
│   └── pl_orion.json
│
├── Notebooks
│   ├── bronze_orion.ipynb
│   ├── silver_sales_orion.ipynb
│   ├── gold_sales_orion.ipynb
│   ├── gold_forecast_orion.ipynb
│   └── dim_date.ipynb
│
├── Data
│   ├── Sample Sales Data
│   ├── Sample Forecast Data
│   └── Structured Outputs
│
├── Power BI
│   ├── Semantic Model
│   └── Dashboard.pbix
│
└── README.md
```

---

## Deliverables

* Fabric Data Pipeline
* PySpark ETL Notebooks
* Structured Delta Tables
* Star Schema Data Model
* Power BI Semantic Model
* Interactive Dashboard
* Project Documentation

---

## Author

Ahmed Shalaby

Data Analyst | Analytics Engineer | Microsoft Fabric & Power BI Developer
