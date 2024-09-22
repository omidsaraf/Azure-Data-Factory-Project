Azure Data Factory
End to end project
In this project, the goal is to ingest data from various departments with different file formats. The process involves: Landing Data: Initial stage where data is ingested. Raw or Bronze Layer: The next stage where raw data is stored. Data Cleansing: Cleaning the data in the subsequent layer. Structured Data: Finally, defining the desired structure and storing the data in Azure SQL DB. This approach ensures that data can be stored relationally from start to finish, allowing for the creation of BI and a Data Warehouse if needed.
1- Requirements:
•	Create a Subscription account, then create a Resource Group under it, and then create Resources under it.
•	Create Storage accounts, including Blob Storage and Data Lake Gen2 Storage.
•	Create Containers for both Storages: Landing for Blob Storage and Raw, Cleansed, Structured for Data Lake Gen2 Storage.
•	Create Azure Data Factory as a Resource to prepare for the next stage.
•	Create Azure Data Factories for Test & Production.
•	Use Auto Integration Runtime
•	Create Azure SQL DB
•	Meet Security Measures for Blob Storage Access Control and Azure SQL DB
•	Create Linked Services for Storage Accounts, Azure SQL DB
2- Azure SQL DB:
As one of the steps we are required to move transformed data into tables so we need to create those tables’ Structure within a Database.
2-1- Deploy Azure SQL DB which includes to Create SQL Server that must have access to Azure Services and resources via Firewall
2-2- Create Schema and Tables’ structures
2-3- Alter Database to add Transparent Data Encryption (TDE) to protect data at rest
2-4- Using CLS method to Encrypt sensitive columns to protect data in use, ensuring that data remains encrypted during transactions.
2-5- Using RLS controls access to rows in a table based on the user's role, ensuring that users can only access the data they are authorized to view
2-6- Using Dynamic Data Masking that masks sensitive data in the result set of a query, providing an additional layer of security by obfuscating data.
2-7- Enable Auditing and Threat Protection Tracks database activities and detects potential threats, helping to identify and respond to suspicious activities.
3- Access Control:
By combining these methods, you can leverage the strengths of each approach to create a secure and efficient data pipeline in Azure Data Factory:
3-1- Key Vault -Keeping Secrets for Storage Account and SQL Server Password -Give List and Read access to ADF
3-2- System-assigned Managed Identity:
•	Automatically created and managed by Azure.
•	Tied to the lifecycle of the ADF instance.
•	Ideal for temporary and service-specific access needs.
•	Use for accessing resources like Azure Key Vault and Azure Storage without managing credentials.
•	Use for general and temporary access to resources like Azure Key Vault and Azure Storage.
3-3- User-assigned Managed Identity:
•	Created and managed by the user.
•	Can be shared across multiple services.
•	Suitable for scenarios requiring persistent and fine-grained access control.
•	Use for scenarios where you need to share identities across multiple ADF instances or other Azure services.
3-4- Service Principal:
•	Manually created in Azure Active Directory.
•	Requires management of client secrets or certificates.
•	Use for scenarios where specific permissions and roles need to be assigned.
•	Ideal for requiring specific permissions, automated deployments and CI/CD pipelines.
•	for creating the linked service.








4- Azure Data Factory Studio
4-1- Create Datasets for Landing, Raw, Cleansed, Structured Zones based on File formats.
4-2- Using Dynamic File Path for Raw and Cleansed to make sure data will be stored by partitions based on daily ingested files.
4-3- Create two dataflows for Raw to Cleansed and Cleansed to Structured zones.
4-4- Create three Pipelines, one for Copy activities, two others for dataflows.
4-5- Create Master Pipeline with dependency and only run by Scheduled Trigger

4- Azure DevOps (CI/CD)
Manual Development Process in Git for Configured Data Factory:
1.	Create a Feature Branch: Start by creating a new feature branch.
2.	Make Changes and Create a Pull Request: Implement your changes and create a pull request.
3.	Approve and Merge: Once the pull request is approved, merge it into the main branch.
4.	Publish: Click the Publish button to generate ARM templates and deploy changes to the live Data Factory.
Automated Mode:
1.	Use NPM for Automation: Utilize the NPM package for automating the publishing process in Azure DevOps, eliminating the need for manual publishing.
2.	Create a Build Pipeline: Set up a Build Pipeline using YAML to automatically apply changes to the main branch whenever there are updates and a pull request is submitted.
3.	Deploy with ARM Templates: Use ARM templates in the Release Pipeline to apply changes to the live Data Factory.
ARM Template Development Task Limitations:
1.	Trigger Changes: Does not support trigger changes and disables active triggers.
2.	Delete Operations: Does not support delete operations.
Solution:
•	Azure PowerShell Tasks: Add two Azure PowerShell tasks before and after the ARM Template Development Task in the Release Pipeline. These tasks use pre-written scripts that can be edited by replacing resource names and addresses.
Project Phases:
Test and Production:
•	Create Resource Groups and ADF Resources: Set up separate Resource Groups and ADF Resources for each phase.
•	Service Principal Access: Grant Service Principal access to all three Resource Groups in Azure DevOps.
•	Clone Development Phase: In the Release Pipeline, clone the Development phase for testing and make the following changes: 
o	Azure PowerShell Tasks: Change the Resource and Resource Group names.
o	ARM Template Development Task: Override values and change the Resource Group name.
•	No Additional Configuration Needed: No need to configure Azure DevOps with the new phases as they are only for developers.
•	Use Variables: Use variables to make the Release Pipeline dynamic, replacing hard-coded values. Define two variables for each phase (Resource Group and Data Factory) with appropriate values. Do not replace the Location File Path in the script with a variable.
•	External Services: External services outside of Azure Data Factory can also be used. Each service has a parameter in the ARM Template, such as the URL for ADL Gen2, which can be overridden with a variable used in both Test and Production phases. Ensure that access is of type Managed Identity and Data Factory v2.
Note:
•	The Data Flow in the Sink section must be set to Single.
•	The Linked Server should be in Legacy mode.
•	The Data Preview in the Sink should display data.
•	The Azure SQL DB must return records.

