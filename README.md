![Linked Services](https://github.com/user-attachments/assets/a035ed1c-2833-44c0-bd72-b36af910520a)## Azure Data Factory Project - Customers' Orders:
End to End Project

![Project Blue Print](https://github.com/user-attachments/assets/68d9cc99-a73b-4bf0-a27f-8fc36a46fcdf)

### Project Overview
The goal of this project is to ingest data from various departments with different file formats. The process involves:

1. **Landing Data**: Initial stage where data is ingested.
2. **Raw or Bronze Layer**: The next stage where raw data is stored.
3. **Data Cleansing**: Cleaning the data in the subsequent layer.
4. **Structured Data**: Finally, defining the desired structure and storing the data in Azure SQL DB.

This approach ensures that data can be stored relationally from start to finish, allowing for the creation of BI and a Data Warehouse if needed.

### Dataset:
![Dataset Discovery](https://github.com/user-attachments/assets/e83f539e-1a7a-4067-a5ac-7a964e888265)


### Requirements

1. **Azure Setup**:
![Linked Services](https://github.com/user-attachments/assets/d44f80d2-6da1-4e13-83d4-ee0c4fc1c6eb)
![01-Deploy Resources](https://github.com/user-attachments/assets/422d5f76-4e70-4459-a40f-2eefc0a10598)


    - Create a Subscription account, then create a Resource Group under it, and then create Resources under it.
    - Create Storage accounts, including Blob Storage and Data Lake Gen2 Storage.
    - Create Containers for both Storages: Landing for Blob Storage and Raw, Cleansed, Structured for Data Lake Gen2 Storage.
    - Create Azure Data Factory as a Resource to prepare for the next stage.
    - Create Azure Data Factories for Test & Production.
    - Use Auto Integration Runtime.
    - Create Azure SQL DB.
    - Meet Security Measures for Blob Storage Access Control and Azure SQL DB.
    - Create Linked Services for Storage Accounts, Azure SQL DB.

3. **Azure SQL DB**:
    - Deploy Azure SQL DB which includes creating a SQL Server that must have access to Azure Services and resources via Firewall.
    - Create Schema and Tables’ structures.
    - Alter Database to add Transparent Data Encryption (TDE) to protect data at rest.
    - Use Column-Level Security (CLS) to encrypt sensitive columns to protect data in use, ensuring that data remains encrypted during transactions.
    - Use Row-Level Security (RLS) to control access to rows in a table based on the user's role, ensuring that users can only access the data they are authorized to view.
    - Use Dynamic Data Masking to mask sensitive data in the result set of a query, providing an additional layer of security by obfuscating data.
    - Enable Auditing and Threat Protection to track database activities and detect potential threats, helping to identify and respond to suspicious activities.

4. **Access Control**:
    - **Key Vault**: Keep secrets for Storage Account and SQL Server Password. Give List and Read access to ADF.
    - **System-assigned Managed Identity**:
        - Automatically created and managed by Azure.
        - Tied to the lifecycle of the ADF instance.
        - Ideal for temporary and service-specific access needs.
        - Use for accessing resources like Azure Key Vault and Azure Storage without managing credentials.
    - **User-assigned Managed Identity**:
        - Created and managed by the user.
        - Can be shared across multiple services.
        - Suitable for scenarios requiring persistent and fine-grained access control.
        - Use for scenarios where you need to share identities across multiple ADF instances or other Azure services.
    - **Service Principal**:
        - Manually created in Azure Active Directory.
        - Requires management of client secrets or certificates.
        - Use for scenarios where specific permissions and roles need to be assigned.
        - Ideal for requiring specific permissions, automated deployments, and CI/CD pipelines.

5. **Azure Data Factory Studio**:
    - Create Datasets for Landing, Raw, Cleansed, Structured Zones based on file formats.
    - Use Dynamic File Path for Raw and Cleansed to ensure data will be stored by partitions based on daily ingested files.
    - Create two dataflows for Raw to Cleansed and Cleansed to Structured zones.
![Raw to Cleansed](https://github.com/user-attachments/assets/9369b2c3-fa0f-4a2f-a064-e17d9f2a1362)
![Cleansed to Structured](https://github.com/user-attachments/assets/3c0a5601-5428-43b1-b738-4d43fe1dccdd)

    - Create three Pipelines: one for Copy activities, and two others for dataflows.

![P1-Landing to Raw](https://github.com/user-attachments/assets/408fcf25-b008-4b0c-87c4-2fddcf735480)
![P2- Raw to Cleansed](https://github.com/user-attachments/assets/c33f27bf-93c0-473f-a802-958822958f52)
![P3- Cleansed to Structured](https://github.com/user-attachments/assets/1f5ce872-05bc-4a81-9648-15025d7d84f5)

    - Create a Master Pipeline with dependencies and schedule it to run by Scheduled Trigger.
![Master Pipeline](https://github.com/user-attachments/assets/42f84ad8-2869-4c5b-9320-63498cc6a55b)

### Azure DevOps (CI/CD)
![Dev](https://github.com/user-attachments/assets/fdd26ae5-817e-45c8-9850-729798c6ee83)


![Git Config](https://github.com/user-attachments/assets/806a9796-b8a3-4ae8-af07-c7262886b80d)

#### Manual Development Process in Git for Configured Data Factory:
1. **Create a Feature Branch**: Start by creating a new feature branch.
2. **Make Changes and Create a Pull Request**: Implement your changes and create a pull request.
3. **Approve and Merge**: Once the pull request is approved, merge it into the main branch.
4. **Publish**: Click the Publish button to generate ARM templates and deploy changes to the live Data Factory.

![Setup CI](https://github.com/user-attachments/assets/abff22e2-620b-49c3-86de-ad55d6149f55)


#### Automated Mode:
1. **Use NPM for Automation**: Utilize the NPM package for automating the publishing process in Azure DevOps, eliminating the need for manual publishing.
2. **Create a Build Pipeline**: Set up a Build Pipeline using YAML to automatically apply changes to the main branch whenever there are updates and a pull request is submitted.
3. **Deploy with ARM Templates**: Use ARM templates in the Release Pipeline to apply changes to the live Data Factory.

![DevOps Pipeline](https://github.com/user-attachments/assets/e7266e52-411d-46d7-900d-bc80a3214038)

#### ARM Template Development Task Limitations:
1. **Trigger Changes**: Does not support trigger changes and disables active triggers.
2. **Delete Operations**: Does not support delete operations.

**Solution**:
- **Azure PowerShell Tasks**: Add two Azure PowerShell tasks before and after the ARM Template Development Task in the Release Pipeline. These tasks use pre-written scripts that can be edited by replacing resource names and addresses.

![Override Parameters](https://github.com/user-attachments/assets/29e81507-243b-49d8-9e21-774ad539bb67)


#### Additiona Project Phases:
**Test and Production**:
- **Create Resource Groups and ADF Resources**: Set up separate Resource Groups and ADF Resources for each phase.
- **Service Principal Access**: Grant Service Principal access to all three Resource Groups in Azure DevOps.
- **Clone Development Phase**: In the Release Pipeline, clone the Development phase for testing and make the following changes:
    - **Azure PowerShell Tasks**: Change the Resource and Resource Group names.
    - **ARM Template Development Task**: Override values and change the Resource Group name.
**Dynamic Parameter Overriding**:
- **Global Parameters**: Utilize global parameters in Azure Data Factory to manage values that need to be consistent across multiple pipelines and environments. These parameters can be overridden during the CI/CD process to adapt to different environments (Development, Test, Production) dynamically.
- **Parameter Files**: Create parameter files for each environment (Development, Test, Production) that specify the values for storage accounts, databases, and other resources. These files can be referenced in the ARM templates to ensure the correct values are used during deployment.
- **Pipeline Expressions**: Use pipeline expressions to dynamically set values based on the environment. This can include setting file paths, connection strings, and other configuration settings that vary between environments.
- **Use Variables**: Use variables to make the Release Pipeline dynamic, replacing hard-coded values. Define two variables for each phase (Resource Group and Data Factory) with appropriate values.

![ARM Template Parameters](https://github.com/user-attachments/assets/78eb2247-11da-4d13-a089-b3f5e417e9cb)


### Outcome:
![Result- Raw Zone](https://github.com/user-attachments/assets/bad20fe0-4897-4c08-9cb4-ec76385f9c8c)
![Result- Cleansed Zone](https://github.com/user-attachments/assets/4f025013-7e23-4d0a-838d-0bfa8543dc1c)
![Devops Pipeline Successful](https://github.com/user-attachments/assets/05c15d8e-7315-4347-a3b9-83354c61019b)
![Result- Structured (SQL DB)](https://github.com/user-attachments/assets/ca7946cb-b20e-4c0a-8e37-fcf92d78712c)








### Monitoring and Error Handling

1. **Monitoring**:
    - **Azure Data Factory Studio**: Monitor all pipeline runs natively in Azure Data Factory Studio. Select **Monitor** from the left menu to view pipeline runs, activity runs, and trigger runs.
    - **Azure Monitor**: Use Azure Monitor to collect and analyze metrics and logs. Set up diagnostic settings to route logs to a Log Analytics workspace, Event Hub, or Azure Storage for long-term storage.
    - **Alerts**: Configure alerts in Azure Monitor to notify you of pipeline failures or other critical events. Alerts can be sent via email, SMS, or other notification channels.

2. **Error Handling**:
    - **Conditional Paths**: Use conditional paths in your pipelines to handle errors. Define paths for **Upon Success**, **Upon Failure**, **Upon Completion**, and **Upon Skip** to manage different outcomes.
    - **Try-Catch Blocks**: Implement Try-Catch blocks to catch errors and execute alternative logic or notifications.
    - **Notifications**: Set up notifications to alert you when a pipeline or activity fails. Use webhooks, Logic Apps, or Azure Functions to send notifications.

3. **Troubleshooting**:
    - **Logs**: Check logs in Azure Data Factory Studio or Azure Monitor to identify the cause of failures. Look for error messages and stack traces.
    - **Retry Policies**: Configure retry policies for activities to automatically retry on transient failures.
    - **Manual Intervention**: For persistent issues, manually intervene to fix the problem and re-run the pipeline.

4. **Long-Term Run History Storage**:
    - **Diagnostic Settings**: Set up diagnostic settings to export pipeline run history to Azure Data Lake Storage (ADLS) or Blob Storage for long-term retention.
    - **Automated Export**: Use Azure Logic Apps or Azure Functions to automate the export of run history to a storage account. Schedule regular exports to ensure data is retained beyond the default 45-day retention period.
  
**Note**:
- The Data Flow in the Sink section must be set to Single.
- The Linked Server should be in Legacy mode.
- The Data Preview in the Sink should display data.
- The Azure SQL DB must return records.

