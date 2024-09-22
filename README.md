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


2- **Access Control**:

By combining these methods, you can leverage the strengths of each approach to create a secure and efficient data pipeline in Azure Data Factory:

 ***A-Key Vault***
   -Keeping Secrets for Storage Account and SQL Server Password
   -Give List and Read access to ADF

 ***B-System-assigned Managed Identity***:
   - **Automatically created** and managed by Azure.
   - **Tied to the lifecycle** of the ADF instance.
   - Ideal for **temporary and service-specific access** needs.
   - Use for accessing resources like **Azure Key Vault** and **Azure Storage** without managing credentials.
   - Use for general and temporary access to resources like Azure Key Vault and Azure Storage.

 ***C-User-assigned Managed Identity***:
   - **Created and managed** by the user.
   - Can be **shared across multiple services**.
   - Suitable for scenarios requiring **persistent and fine-grained access control**.
   - Use for scenarios where you need to **share identities** across multiple ADF instances or other Azure services.

 ***D-Service Principal***:
   - **Manually created** in Azure Active Directory.
   - Requires **management of client secrets** or certificates.
   - Use for scenarios where **specific permissions** and **roles** need to be assigned.
   - Ideal for requiring specific permissions,**automated deployments** and **CI/CD pipelines**.
   - for creating the linked service.

3- **Security Measures in Azure SQL Database**
By implementing these measures, you can ensure a comprehensive security setup for your Azure SQL Database, protecting data at rest, in transit, and in use, while also controlling access and monitoring for threats
   ***3-1-Transparent Data Encryption (TDE)***: Encrypts the entire database to protect data at rest.
   ***3-2-Always Encrypted***: Encrypts sensitive columns to protect data in use, ensuring that data remains encrypted during transactions.
   ***3-3-Row-Level Security (RLS)**: Controls access to rows in a table based on the user's role, ensuring that users can only access the data they are authorized to view.
   ***3-4-Dynamic Data Masking (DDM)***: Masks sensitive data in the result set of a query, providing an additional layer of security by obfuscating data.
   ***3-5-Auditing and Threat Detection***: Tracks database activities and detects potential threats, helping to identify and respond to suspicious activities.

2- **Debug**:
- The Data Flow in the Sink section must be set to Single.
- The Linked Server should be in Legacy mode.
- The Data Preview in the Sink should display data.
- The Azure SQL DB must return records.
