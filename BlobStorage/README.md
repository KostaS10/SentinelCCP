# Instructions on how to create and deploy the connector:


1. ### Deploy the template in Azure with "Deploy a custom template" option in Azure

2. ### Click edit template and configure the following:
   - Starting from line 171, define the columns of the data stored in BlobStorage
    - Line 203, configure transformKql line if you need any additional transformation with the data
    - Starting from line 220, define the table schema of that custom table where data will be stored
3. ### Define the parameters:
    - Select subscription, RG and region where you want to deploy the connector (where Sentinel instance is)
    - workspace-location parameter defines the location
    - Workspace - provide workspace name
    - Connector title - provide title for your connector
    - Connector Title Max25Char - provide short name for connector (up to 25 characters)
    - Table Name - provide custom table name where the data will be stored (needs to end with _CL)
    - Log format - select either CSV or JSON depending on how data is stored in BlobStorage
    - Gzip compression - select whether data stored in BlobStorage is compressed with gzip or not

# Instructions on how to connect the connector

1. ### Tenant-wide application consent

    - If this is your first time connecting BlobStorage connector, you need **any Entra ID role which can grant tenant-wide application consent** [Entra ID Documentation ](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/grant-admin-consent?pivots=portal#prerequisites). After confirming permissions, clicking on the blue button in the connector (Grant tenant-wide) will create and populate the required **Sentinel azure storage enterprise application - service principal ID.**
    - If you have connected, **Sentinel azure storage enterprise application - service principal ID** value will be populated

2. ### Required Azure permissions:

    - **User Access Administrator** - user connecting the connector needs **User Access Administrator** permissions over the resource group where Sentinel is deployed *and* where the Storage Account containing logs is
    - **Contributor** - user connecting the connector needs **Contributor** permissions over the resource group where Sentinel is deployed *and* where the Storage Account containing logs is

3. ### Storage Account network settings

    - CCF connector needs to communicate with the Storage Account. Currently supported configurations in the **Networking** blade:

      - **Public Network Access > Enabled from all networks**
      - **Public Network Access > Enabled from selected networks** - in this case you need to put Scuba IP addresses in the allowed list [MS Documentation where to find IP ranges](https://learn.microsoft.com/en-us/azure/sentinel/create-codeless-connector#maintain-network-isolation-for-logging-source)

4. ### Provide connector parameters:

    - Blob container URL - URL of the blob container where the logs are (example: https://**StorageAccountName**.blob.core.windows.net/**ContainerName**)
    - Storage Account location - region (westeurope, eastus, etc)
    - Storage Account Resource Group Name - name of RG where Storage Account is
    - Subscription ID - subscription ID where Storage Account is
    - Event Grid System Topic Name - check if the Storage Account has a system topic by navigating to **Events>Event Subscriptions**

        - If one does not exist, leave the parameter empty and the connector will create one automatically
        - If one exists, copy *System Topic* name and put it in the connector parameter: **The event grid system topic name of the blob container's storage account**
