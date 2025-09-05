# Generative AI Data Architecture

## Overview
This repository contains a project demonstrating the **full data lifecycle** for the Online Retail dataset, including extraction, transformation, loading (ETL), data warehouse design, and analysis. The goal is to create a reusable workflow for handling raw retail data and enabling business intelligence.

<img width="822" height="596" alt="generative-ai-data-architecture" src="https://github.com/user-attachments/assets/5a94fba6-e04f-429f-8f5c-408010dec810" />

The **dataset** used is publicly available under the **CC BY 4.0 license**. No proprietary data was used.

---

## Data Architecture

**Project Flow:**

Online_Retail.xlsx → ETL Process → Staging Tables → Data Warehouse → Analysis & BI


**Details:**

- **Source Data**: Excel file (`Online_Retail.xlsx`)  
- **ETL**: Cleans the data, removes duplicates, converts data types, and handles missing values  
- **Staging Tables**: Optional intermediate tables  
- **Data Warehouse**: Star schema with a fact table (`FactSales`) and dimension tables (`DimCustomer`, `DimProduct`, `DimDate`)  
- **Analysis**: SQL queries and Python (pandas, matplotlib, seaborn)  

---

## Data Warehouse Schema

### Fact Table: `FactSales`
- `InvoiceNo` (PK)  
- `StockCode` (FK → DimProduct)  
- `CustomerID` (FK → DimCustomer)  
- `Quantity`  
- `UnitPrice`  
- `InvoiceDate` (FK → DimDate)  

### Dimension Tables

**DimCustomer**  
- `CustomerID` (PK)  
- `Country`  

**DimProduct**  
- `StockCode` (PK)  
- `ProductDescription`  

**DimDate**  
- `InvoiceDate` (PK)  
- `Day`, `Month`, `Year`, `Weekday`  

---

## ERD

You can visualize the schema using [dbdiagram.io](https://dbdiagram.io) with this code:

```sql
Table FactSales {
  InvoiceNo varchar [pk]
  StockCode varchar
  CustomerID int
  Quantity int
  UnitPrice float
  InvoiceDate date
}

Table DimCustomer {
  CustomerID int [pk]
  Country varchar
}

Table DimProduct {
  StockCode varchar [pk]
  ProductDescription varchar
}

Table DimDate {
  InvoiceDate date [pk]
  Day int
  Month int
  Year int
  Weekday varchar
}

Ref: FactSales.CustomerID > DimCustomer.CustomerID
Ref: FactSales.StockCode > DimProduct.StockCode
Ref: FactSales.InvoiceDate > DimDate.InvoiceDate
```

## ETL Pipeline

Extract: Reads Online_Retail.xlsx using pandas

Transform:

Remove duplicates

Convert InvoiceDate to datetime

Normalize Country text

Convert Quantity and UnitPrice to numeric

Drop rows missing CustomerID

Load: Insert data into SQLite tables: FactSales, DimCustomer, DimProduct, DimDate

Test Query: Example SQL join to confirm data integrity

## Infrastructure Requirements

Python 3.x

Jupyter Notebook

SQL database (SQLite for testing; PostgreSQL/MySQL recommended for production)

Pandas, SQLAlchemy, Matplotlib/Seaborn for analysis

Optional: Tableau, PowerBI, or Plotly for visualization

## Analysis & Data Mining

Sales analysis by country, product, and month

Customer segmentation and behavior analysis

Optional clustering and association rules for insights

## Notes

The dataset is publicly available under CC BY 4.0 license.

All code is written in Python and tested in Jupyter Notebook.

The ETL pipeline can be adapted for other retail datasets.
