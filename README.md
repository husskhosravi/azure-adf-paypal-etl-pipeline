# 🧾 Azure Data Factory: PayPal Transaction Processing Pipeline

## 🔍 Overview  
This project showcases an Azure Data Factory (ADF) pipeline designed to process PayPal transaction data, associate it with product information, and extract valuable business insights. The pipeline demonstrates effective use of cloud-based ETL processes for financial data integration.


## 🏗️ Pipeline Architecture  
The solution consists of multiple interconnected pipelines:

### 1️⃣ Data Copy Pipeline  
This pipeline handles the initial data movement:
![PayPalCopyPipeline](https://github.com/husskhosravi/azure-datafactory-synapse-pipelines/blob/main/screenshots/PayPalCopyPipeline.png)

- Copies source CSV files (`Products.csv` and `PayPal Payments.csv`) from `"hkblob"` storage to `"adfblobstorage"`
- Includes error handling with a failure path
- Triggers the transformation pipeline upon successful completion

### 2️⃣ Transformation Pipeline  
![PayPalTransformation_workflow](https://github.com/husskhosravi/azure-datafactory-synapse-pipelines/blob/main/screenshots/PayPalTransformation_workflow.png)

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
![AzureDataStudioQuery](https://github.com/husskhosravi/azure-datafactory-synapse-pipelines/blob/main/screenshots/AzureDataStudioQuery.png)

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

### 📦 Data Export  
The final step in the workflow runs an SQL query to extract processed data from the staging database:  
![Exporttocsv_workflow](https://github.com/husskhosravi/azure-datafactory-synapse-pipelines/blob/main/screenshots/Exporttocsv_workflow.png)

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
- End-to-end ETL workflow design  

---

# 📊 Azure Synapse Analytics: Automated Reporting Pipeline

## 🔍 Overview  
This extension demonstrates a separate reporting pipeline built in **Azure Synapse Analytics**, showcasing how customer data can be transformed into business-ready reports and prepared for automated distribution.
![AzureSynapseAnalytics_workflow](https://github.com/husskhosravi/azure-datafactory-synapse-pipelines/blob/main/screenshots/AzureSynapseAnalytics_workflow.png)

## 🧱 Pipeline Architecture  
The Synapse pipeline performs the following operations:

1. **Data Extraction**:  
   - Connects to the *Customer* table in Azure SQL Database  
   - Pulls data using a SQL query  

2. **Report Generation**:  
   - Transforms raw customer data into a reporting format  
   - Applies business rules and formatting logic  

3. **Dynamic Storage**:  
   - Outputs reports to Azure Blob Storage as CSV  
   - Uses a dynamic folder naming convention that ensures each report is timestamped and stored in a structured format for easier archiving, retrieval, and auditing:
```sql
concat('MonthlyReport', toString(currentDate()))
```

4. **Distribution Automation (Planned)**:  
   - Designed to integrate with **Azure Logic Apps**  
   - Intended to send report download links via email  

## 📩 Future Enhancements  
- Enable **Logic Apps** integration for automated email distribution  
- Add **Power BI** dashboard generation as a downstream output  
- Include **data quality checks** pre-export  
- Add **access control** for report visibility based on user roles  
