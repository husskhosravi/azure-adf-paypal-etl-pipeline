# 🧾 Azure Data Factory: PayPal Transaction Processing Pipeline

## 🔍 Overview  
This project showcases an Azure Data Factory (ADF) pipeline designed to process PayPal transaction data, associate it with product information, and extract valuable business insights. The pipeline demonstrates effective use of cloud-based ETL processes for financial data integration.

This GitHub repository contains documentation and screenshots of the pipeline implementation rather than deployable code, as Azure Data Factory pipelines are configured directly in the Azure portal.

## 🏗️ Pipeline Architecture  
The solution consists of multiple interconnected pipelines:

### 1️⃣ Data Copy Pipeline  
This pipeline handles the initial data movement:
![PayPalCopyPipeline](https://github.com/user-attachments/assets/b16befd2-3279-4d06-ac22-4dc95912bfc2)

- Copies source CSV files (`Products.csv` and `PayPal Payments.csv`) from `"hkblob"` storage to `"adfblobstorage"`
- Includes error handling with a failure path
- Triggers the transformation pipeline upon successful completion

### 2️⃣ Transformation Pipeline  
![PayPalTransformation_workflow](https://github.com/user-attachments/assets/5975653d-16a3-4e10-aafc-80b08d5a7e9c)

The main data processing pipeline:

#### 📥 Data Sources  
- **PayPalTransactions**: Imports transaction data from `PayPal Payments.csv`  
- **PayPalProducts**: Imports product details from `Products.csv`

#### 🔄 Data Transformations  
- **PayPalJoin**: Performs a left outer join between PayPal transactions and product data  
  - Links transaction records with corresponding product information  
  - Join condition based on relationships between `PayPalTransactions` and `PayPalProducts`  
- **StatusFilter**: Filters transaction data  
  - Includes only transactions with "Paid" status  
  - Excludes pending, failed, or cancelled transactions  

#### 📤 Data Destination  
- **TransferToDB**: Loads processed data to destination database  
  - Creates structured dataset ready for analysis  
  - Supports financial reporting and business intelligence  
  - Can be triggered manually or set on a schedule (hourly, daily, etc.)  
  - Pipeline execution logs provide runtime statistics and error tracking  
![AzureDataStudioQuery](https://github.com/user-attachments/assets/db694823-b975-4c8f-883e-16e55510a6fe)

## 💼 Business Value  
This pipeline delivers several key business benefits:

- **Financial Reconciliation**: Accurately tracks successful PayPal transactions  
- **Product Performance Analysis**: Links sales data with product information  
- **Revenue Tracking**: Focuses on completed ("Paid") transactions for accurate revenue reporting  
- **Data Integration**: Combines disparate data sources into a unified view  
- **Reporting Readiness**: Prepares data for dashboard visualisations and reports  

## ⚙️ Technical Implementation  

### 🧱 Prerequisites  
- Azure subscription  
- Azure Data Factory service  
- Storage account for source data  
- Destination database (Azure SQL Database recommended)  

### 🛠️ Configuration  
The pipeline is configured with:
- Data flow debug enabled for development  
- Parameterised dataset connections for flexibility  
- Error handling for production reliability  

### 🚀 Deployment  
Instructions for implementing this pipeline:

1. Create these components in your own Azure Data Factory instance  
2. Configure connection strings to your data sources  
3. Upload source data to your storage location  
4. Set up a trigger or manually run the pipeline  

### 📦 Data Export  
The final step in the workflow runs an SQL query to extract processed data from the staging database:  
![Exporttocsv_workflow](https://github.com/user-attachments/assets/9492ef5e-4ae4-472b-b1f1-7de8e5a382d6)

```sql
SELECT [product_id (metadata)], [Amount]
FROM [hkdatabase].staging.[PayPalData]
```

This query:
- Extracts key business metrics from the transformed data  
- Creates a summary dataset
- Saves the results back to `"adfblobstorage"` as a CSV file for reporting and analysis  

## 🔮 Future Enhancements  
Potential extensions to this pipeline include:

- Adding data quality validation steps  
- Implementing incremental loading patterns  
- Creating automated reporting with Power BI  
- Adding transaction trend analysis  
- Implementing real-time processing with Event Hubs  

## 🧠 Skills Demonstrated  
This project showcases the following technical skills:

- Azure Data Factory pipeline design and implementation  
- Multi-pipeline orchestration and dependencies  
- Azure Blob Storage data movement  
- Error handling and failure paths  
- Azure SQL Database integration  
- Data transformation logic  
- SQL query-based data extraction  
- Pipeline monitoring and debugging  
- Financial data processing  
- End-to-end ETL workflow design  
