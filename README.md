# 1-	Introduction
This project outlines a comprehensive data engineering pipeline utilizing various Azure services. We'll leverage Azure Data Factory to automate data retrieval, store it securely in Azure Data Lake Gen2, perform transformations using Azure Databricks, build data models in Azure Synapse Analytics, and finally create interactive visualizations with Power BI.
Following are the components and steps involved in this project:
# 1.	Data Ingestion (Azure Data Factory):
Azure Data Factory serves as our data ingestion platform. It enables us to collect Airbnb data and NYPD shooting list from various sources. Data Factory’s data connectors and scheduling capabilities are invaluable for automated ingestion.
# 2.	Data Storage (Azure Data Lake Gen2):
Processed Airbnb and NYPD shootings data is processed and saved in Azure Data Lake Gen2. This storage solution provides scalable, secure, and cost-effective storage, which is critical for accommodating the increasing volume of epidemic data.
# 3.	Data Transformation (Azure Databricks):
We utilize Azure Databricks for data transformation and processing tasks. Databricks clusters allow us to perform data cleansing, normalization, and feature engineering, preparing the Airbnb and NYPD shootings incidents data.
# 4.	 Data Modeling (Azure Synapse):
Azure Synapse serves as our data modeling and analytics platform. We employ it to build data models, perform SQL-based queries, and conduct complex analytical tasks on the above-mentioned dataset.
# 5.	Data Visualization (Power BI):
Power BI is the tool of choice for data visualization. We create interactive dashboards and reports to present the Airbnb and shooting incidents data insights, enabling stakeholders to make informed decisions.
By orchestrating data flow, transformation, storage, modeling, and visualization using Azure services, we aim to provide actionable insights from this critical dataset.
