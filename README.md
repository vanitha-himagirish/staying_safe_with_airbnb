# Data Engineering Project – Airbnb listings and Shooting incident analysis in New York
## Introduction
This project outlines a comprehensive data engineering pipeline utilizing various Azure services. We'll leverage Azure Data Factory to automate data retrieval, store it securely in Azure Data Lake Gen2, perform transformations using Azure Databricks, build data models in Azure Synapse Analytics, and finally create interactive visualizations with Power BI.
Following are the components and steps involved in this project:
## Data Ingestion (Azure Data Factory):
Azure Data Factory serves as our data ingestion platform. It enables us to collect Airbnb data and NYPD shooting list from various sources. Data Factory’s data connectors and scheduling capabilities are invaluable for automated ingestion.
## Data Storage (Azure Data Lake Gen2):
Processed Airbnb and NYPD shootings data is processed and saved in Azure Data Lake Gen2. This storage solution provides scalable, secure, and cost-effective storage, which is critical for accommodating the increasing volume of epidemic data.
## Data Transformation (Azure Databricks):
We utilize Azure Databricks for data transformation and processing tasks. Databricks clusters allow us to perform data cleansing, normalization, and feature engineering, preparing the Airbnb and NYPD shootings incidents data.
## Data Modeling (Azure Synapse):
Azure Synapse serves as our data modeling and analytics platform. We employ it to build data models, perform SQL-based queries, and conduct complex analytical tasks on the above-mentioned dataset.
## Data Visualization (Power BI):
Power BI is the tool of choice for data visualization. We create interactive dashboards and reports to present the Airbnb and shooting incidents data insights, enabling stakeholders to make informed decisions.
By orchestrating data flow, transformation, storage, modeling, and visualization using Azure services, we aim to provide actionable insights from this critical dataset.

# Architecture
![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/43574cb3-1ec3-4688-9b23-e6eea65d8957)


The following steps were used to build the end-to-end pipeline:
## Step 1: Creating a Resource Group 
- Created Resource Group name - airbnb-safety-reporting-adf
![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/2f4fcc0d-992e-4212-9bd4-adeec2473214)

## Step 2: Creating Azure Storage account.
- Created Storage account name -airbnbsafetysa

## Step 3: Download Azure storage explorer.

## Step 4: Creating Azure Lake storage Gen2
- Used as Datalake for data analysis
- Airbnb data - airbnbsafetydl
- Enable hirerarchial namespace in Advanced tab
![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/42563eb1-f007-4d3c-97aa-8ba74afc4cf6)
![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/e7e24721-ae9c-4f39-8e7a-b60e3399f22a)

### Azure Data Factory:
## Step 6: Data Ingestion pipeline – pl_ingest_listings_data – Ingestion of Airbnb data from Azure blob storage to Data Lake
### Copy activity-Azure blob storage to Data Lake
  ### Source
  -	Storage account - airbnbsafetysa
  -	Container-airbnblisting
  -	File-listings.csv.gz
  ### Sink
  - Storage account-airbnbsafetydl
  - Container-raw
  - File: airbnb/listings.csv
    
The above pipeline copy activity consists of following steps:
1. Lined service - source - ls_ablob_airbnbsafetysa
2. Source dataset -ds_listings_raw_gz
3. Linked service -sink - ls_adls_airbnbsafetydl
4. Sink dataset - ds_listings_raw_csv
5. Pipeline-pl_ingest_listings_data
6. Copyactivity - Copy Airbnb listings data

![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/b7e9d41f-d76d-41ed-aeb2-2f670df14620)


![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/9b189d79-82e4-4f1c-933e-77436bfdcdea)


![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/55b2cfd4-2f75-445f-8314-9cb803b7e687)

## Step 7: Ingesting NYPD data from http://
1.	Create Linked service - ls_nypd_shooting_incidents
2.	Source-https://github.com/vanitha-himagirish/staying_safe_with_airbnb/tree/main/data
3.	Sink
    - Storage - airbnbsafetydl
    - Container-raw
    - File: nypd_shooting_incidents
4. Create datasets -ds_nypd_shooting_incidents
5. Create pipeline: pl_ingest_nypd_shooting_incidents
6. Copy Activity - to copy from http to DataLake

![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/bb830271-3575-458d-9075-03d2ad5697ce)

![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/1a631c09-fcc1-4b13-8bc4-dc49e3f43d43)

Triggers - create trigger and attach to pipeline

## Step 8: Creating data flows for transformation – removing certain columns
1. Data Flow 1 - listings transformation
2. Use dataset - airbnbsafetydl/listings
3. Requirement
4. Source Transformation
5. Filter Transformation
6. Select Transformation
7. Sink Transformation

![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/e5ccbf37-e648-45f2-a9b3-e1cd108cbf92

## Step 8: Azure Databricks – for transformation 

During the data transformation stage, we use Azure Databricks with its notebook environment to write and run Spark code. To get started, we set up Azure Databricks by creating the necessary resources and computing power. But before we jump into coding, we need to make a secure connection between Azure Databricks and the Azure Storage accounts where our raw data is stored.

To create this secure connection, we built an application using Azure's 'App registrations' feature. This application provides an 'Application (client) ID' and a 'Directory (tenant) ID' that will be used for authentication.

1. Click on Azure Home - > Microsoft Entra ID
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/20acd63c-8a13-410f-a47e-b91e30d3b39e)
2. Create App – airbnb-safety-app
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/687d9960-79cd-4689-b8b0-4d88aaa7f2b1)
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/940e69b4-1b77-4c25-ba1d-428ed4e14036)
3. Create client secret. Next, we went to the 'Certificates & secrets' section of the application management and created a client secret key . This secret key is essential     for the secure connection between Azure Databricks and your Azure Storage accounts. It acts like a password, allowing authorized access for smooth data transformation       and processing.
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/8b30ddce-29a7-47e5-ac18-a08d66224d5e)
4. Application(client) id - 857a0579-a3f1-4f5d-ae7b-580d214a527e
   Directory(tenant) id - ecbacd17-e9a2-448c-b342-9fa6bd7cd985
   Secret - 5****~********.nF170e2N7tVama.Y2OkKPnbhm
   airbnbsafetydl - Create IAM access and assign access to airbnb-safety-app
5. The last step is to set up permissions in Azure Storage. We do this by assigning the 'Storage Blob Data Contributor' role to Azure Databricks. This role gives Azure 
   Databricks the necessary permissions to read, write, and delete data in your Azure Storage blob containers. This allows Azure Databricks to work efficiently with your       data during the transformation process.
6. With everything securely connected, we can now jump into writing Spark code in our Databricks notebook. The first step will be to establish a connection between the         storage container in Azure Storage and the notebook.
7. Setup – Mounting folders
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/bb4f3077-cb78-485b-bbc4-3baa6aeb2144)
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/7f6ef751-3529-4d07-a6b7-55056e64ca26)
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/4e35e78a-5c58-4556-8a6c-d279cd73134e)
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/280927f8-f709-4c08-939d-69558dca7d21)
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/08b58a55-28fc-4faf-ba77-e99220c1b6cf)
8. After that, we should configure our Spark session using the command: Spark.
   Now, we can begin our actual data work by first reading the data. We can utilize various PySpark SQL DataFrame methods, such as .show() and .printSchema(), to view a
   gain a better understanding of the data.After reviewing the data and considering our requirements, we have decided to extract dimension and fact tables from our dataset.
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/e3793857-c5ce-4706-8599-8712b10a3ace)
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/feadfdac-1878-4dc9-806c-8e0996b8a042)
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/87ef3e05-5e29-42b8-a9bd-150df94c8b68)
   ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/878fe974-54b6-47a9-9e17-24523a972faa)
9. Saving the dimension and fact tables in Azure datalake Gen2
    ![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/c5144522-fd24-42b2-8390-79965c83267a)
10. Now all the target data is stored under airbnbsafetydl/transformed/
    
## Step 9: Azure Synapse analytics

We have our data in the tables, we will proceed to load it into the Lake Database in Azure Synapse Analytics, enabling us to create our models.
First, we need to set up our Azure Synapse workspace. By creating our Synapse Studio, we also create another Storage Account: Azure Data Lake Storage Gen2 (ADLS Gen2).

![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/fed9c08a-6ad5-4d47-9dee-f1c5be1f3660)

To use Azure Synapse for working with this data, we should copy the files from the ‘Transformed’ container into our ADLS Gen2. For this purpose, we will utilize a pipeline containing a copy activity from our source with the linked service: AzureBlobStorage, to our destination with the linked service: Default Storage account for our Synapse workspace (ADLS Gen2).

Now, in the data part of the Synapse workspace, we add a Lake Database named ‘airbnb_safery.’ Following this, we create external tables from the data lake. To do this, we specify the External table name (which will be the same as ‘dim_host,’ ‘dim_listings,’ etc.), the Linked service (which will be ‘ADLS Gen2’), and the Input file or folder. This input will specify the path to the files.

![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/144a5b08-9c7f-4fce-8d94-15f69e04489d)

## Step 10: Visualization – Power BI

![image](https://github.com/vanitha-himagirish/staying_safe_with_airbnb/assets/55011879/25e94f90-592f-49e8-bd37-da8689f70b17)












   





   











