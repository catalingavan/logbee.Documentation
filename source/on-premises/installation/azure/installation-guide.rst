Installation guide
=============================

.. contents:: Table of contents
   :local:

This tutorial will guide trough the installation steps for Logbee in Microsoft Azure.

Set up prerequisites
-------------------------------------------------------

Azure Cosmos DB
~~~~~~~~~~~~~~~~~~~~~

Create an Azure Cosmos DB account. Select **Azure Cosmos DB for NoSQL**.

.. list-table::
   :header-rows: 1

   * - Properties
     - 
   * - Account Name
     - logbee-cosmosdb *(or any value)*
   * - Availability Zones
     - Disable
   * - Location
     - (Europe) West Europe *<or any appropriate value>*
   * - Capacity mode
     - Provisioned throughput
   * - Apply Free Tier Discount
     - Apply
   * - Limit total account throughput
     - Checked
   * - Geo-Redundancy
     - Disable
   * - Multi-region Writes
     - Disable
   * - Connectivity method
     - All networks
   * - Backup policy
     - Continuous (7 days) *(available for free)*
   * - Data Encryption
     - Service-managed key

Once the Azure Cosmos DB account has been created, navigate to **Settings > Keys** and copy the "PRIMARY CONNECTION STRING" to a text editor.

.. figure:: images/azure-cosmos-db-connection-string.png
    :alt: Azure Cosmos DB Connection String

Storage account
~~~~~~~~~~~~~~~~~~~~~

Logbee uses Azure storage account for saving blob files and for reducing the workload of Azure Cosmos DB.

Create a storage account.

.. list-table::
   :header-rows: 1

   * - Properties
     - 
   * - Storage account name
     - logbeestorage *(or any value)*
   * - Region
     - (Europe) West Europe *<or any appropriate value>*
   * - Primary service
     - Azure Blob Storage or Azure Data Lake Storage Gen 2
   * - Performance
     - Standard
   * - Redundancy
     - Locally-redundant storage (LRS)
   * - Access tier
     - Hot
   * - Public network access
     - Enable
   * - Public network access scope
     - Enable from all networks
   * - Encryption type
     - Microsoft-managed keys (MMK)

Once the Storage account has been created, navigate to **Security & networking > Access keys** and copy the "Connection string" to a text editor.

.. figure:: images/storage-account-access-key.png
    :alt: Storage account Connection string

Navigate to **Data storage > File shares** and create a new File share.

.. list-table::
   :header-rows: 1

   * - Properties
     - 
   * - Name
     - logbee-config
   * - Access tier
     - Transaction optimized
   * - Enable backup
     - Unchecked

.. figure:: images/storage-account-file-share-create.png
    :alt: Storage account Connection string

Prepare the configuration files
-------------------------------------------------------

\1. Download the configuration files for both Logbee.Frontend and Logbee.Backend and save them locally:

- `backend.logbee.json <https://github.com/catalingavan/logbee-app/blob/main/logbee.Backend/logbee.json>`_

- `frontend.logbee.json <https://github.com/catalingavan/logbee-app/blob/main/logbee.Frontend/logbee.json>`_

.. code-block:: none

    /logbee-config
    ├── backend.logbee.json
    └── frontend.logbee.json

\2. Update the **backend.logbee.json** configuration file as following:

.. code-block:: json
    
    {
        "LogbeeFrontendConfigurationFilePath": "configuration/frontend.logbee.json",
        "LogbeeBackendUrl": "https://logbee-backend.azurewebsites.net",
        "Database": {
            "Provider": "AzureCosmosDb",
            "AzureCosmosDb": {
                "ApplicationRegion": "West Europe",
                "ConnectionString": "<Azure Cosmos DB Connection string>",
                "DatabaseName": "logbee-backend",
                "AzureStorageAccountConnectionString": "<Storage account Connection string>"
            }
        },
        "FileStorage": {
            "Provider": "Azure",
            "MaximumFileSizeInBytes": 2097152,
            "Azure": {
                "ConnectionString": "<Storage account Connection string>"
            }
        }
    }

- **LogbeeBackendUrl** must be updated with the Logbee.Backend App Service domain (which has not been yet created).

- **Database.AzureCosmosDb.ApplicationRegion** must be updated with the region name where the Azure Cosmos DB has been created.

