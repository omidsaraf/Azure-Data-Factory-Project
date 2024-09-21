# Azure Data Factory 
End to end project

In this project, the goal is to ingest data from various departments with different file formats. The process involves:
Landing Data: Initial stage where data is ingested.
Raw or Bronze Layer: The next stage where raw data is stored.
Data Cleansing: Cleaning the data in the subsequent layer.
Structured Data: Finally, defining the desired structure and storing the data in Azure SQL DB.
This approach ensures that data can be stored relationally from start to finish, allowing for the creation of BI and a Data Warehouse if needed.

1- **Requirements**:
- Create a Subscription account, then create a Resource Group under it, and then create Resources under it. (It should be like a hierarchy)
- Create Storage as a Resource, including Blob Storage and Data Lake Gen2 Storage.
- Create Containers for both Storages: Landing for Blob Storage and Raw, Cleansed, Structured for Data Lake Gen2 Storage.
- Create Azure Data Factory as a Resource to prepare for the next stage.



**Debug**:
-The Data Flow in the Sink section must be set to Single.
-The Linked Server should be in Legacy mode.
-The Data Preview in the Sink should display data.
-The Azure SQL DB must return records.
