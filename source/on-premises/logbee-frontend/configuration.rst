Configuration
=================================

logbee.Frontend behavior can be customized by updating the ``Configuration\logbee.json`` file.

A full example of the configuration file can be found on the `GitHub <https://github.com/catalingavan/logbee-app/blob/main/logbee.Frontend/logbee.json>`_ page.

.. contents:: Configuration options
   :local:

LogbeeBackendConfigurationFilePath
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The path to the Logbee.Backend configuration file. The value can be absolute or relative.

.. code-block:: json
    
    {
        "LogbeeBackendConfigurationFilePath": "../../logbee.Backend/Configuration/logbee.json"
    }

LogbeeFrontendDomain
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The domain of the Logbee.Frontend application. This value is used in multiple places:

- when generating the HTML titles
- sets the default email domain for the authenticated users (``user@{LogbeeFrontendDomain}``).
- setting the sender email address to ``no-reply@`` + :ref:`LogbeeFrontendDomain <on-premises/logbee-frontend/configuration:LogbeeFrontendDomain>` (e.g., ``no-reply@logbee.dev``), if no other value is specified under the :ref:`Smtp.Sender <on-premises/logbee-frontend/configuration:Smtp.Sender>` configuration.

.. code-block:: json
    
    {
        "LogbeeFrontendDomain": "logbee.dev"
    }

StaticResourcesVersion
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Property used as query string parameter when loading static resources (css/javascript).

This property should be changed after an application update in order to invalidate the static files cache.

.. code-block:: json
    
    {
        "StaticResourcesVersion": "1.0.0"
    }

LogbeeFrontendUrl
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Root url pointing to Logbee.Frontend application.

.. code-block:: json
    
    {
        "LogbeeFrontendUrl": "http://logBee.myapp.com/"
    }

LogbeeFrontend.BasicAuth.Password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Basic HTTP authentication scheme password used to connect to Logbee.Frontend application.

.. code-block:: json
    
    {
        "LogbeeFrontend.BasicAuth.Password": "_LogBeeFrontend_authorization_password_"
    }

Logbee.License
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Logbee on-premises license key. Can be null.

.. code-block:: json
    
    {
        "Logbee.License": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJMaWNlbnNlSWQiOiIzNTczMDI1My00NGRhLTRiZmMtOGQ0MS1iMzUzMDRkZWUyMzciLCJMaWNlbnNlVHlwZSI6IkVudGVycHJpc2UifQ.K4htH3YOulrpVrkTJuHza81VrYloYvTsfRYzb4fpUYI"
    }


Database
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
    
    {
        "Database": {
            "Provider": "MongoDb",
            "MongoDb": {},
            "MySql": {},
            "SqlServer": {},
            "AzureCosmosDb": {}
        }
    }

.. list-table::
   :header-rows: 1

   * - Database.Provider
     - 
   * - MongoDb
     - Sets the database provider to MongoDb.
   * - MySql
     - Sets the database provider to MySql.
   * - SqlServer
     - Sets the database provider to MS-SQL.
   * - AzureCosmosDb
     - Sets the database provider to Azure CosmosDB.

.. list-table::
   :header-rows: 1

   * - Database.MongoDb
   * - Required when "Database.MongoDb" is "MongoDb".

.. list-table::
   :header-rows: 1

   * - Database.MySql
   * - Required when "Database.Provider" is "MySql".

.. list-table::
   :header-rows: 1

   * - Database.SqlServer
   * - Required when "Database.Provider" is "SqlServer".

.. list-table::
   :header-rows: 1

   * - Database.AzureCosmosDb
   * - Required when "Database.Provider" is "AzureCosmosDb".

Database.MongoDb
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration used to connect to MongoDb database.

.. code-block:: json
    
    {
        "Database": {
            "MongoDb": {
                "ConnectionString": "mongodb://localhost:27017?socketTimeoutMS=5000&connectTimeoutMS=5000",
                "DatabaseName": "LogbeeFrontend"
            }
        }
    }

Database.MySql
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration used to connect to MySql database.

.. code-block:: json
    
    {
        "Database": {
            "MySql": {
                "ConnectionString": "server=localhost;port=3306;database=LogbeeFrontend;uid=<replace_user>;password=<replace_password>;Charset=utf8;"
            }
        }
    }

Database.SqlServer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration used to connect to MS-SQL database.

.. code-block:: json
    
    {
        "Database": {
            "SqlServer": {
                "ConnectionString": "Server=localhost;Database=LogbeeFrontend;User ID=<replace_user>;Password=<replace_password>;TrustServerCertificate=True;"
            }
        }
    }

Database.AzureCosmosDb
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration used to connect to Azure CosmosDB database.

.. code-block:: json
    
    {
        "Database": {
            "AzureCosmosDb": {
                "ApplicationRegion": "West Europe",
                "ConnectionString": "https://cosmos-db-name.documents.azure.com:443/;AccountKey=_accountKeyValue_;",
                "DatabaseName": "LogbeeFrontend",
                "AzureStorageAccountConnectionString": "DefaultEndpointsProtocol=https;AccountName=storagename;AccountKey=_accountKeyValue_;EndpointSuffix=core.windows.net"
            }
        }
    }

Smtp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

SMTP configuration used for sending automated emails (alert notifications).

.. code-block:: json
    
    {
        "Smtp": {
            "Sender": {},
            "Host": "smtp.sendgrid.net",
            "Port": 587,
            "UserName": "",
            "Password": "",
            "EnableSsl": false
        }
    }

Smtp.Sender
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``Smtp.Sender`` configuration is optional and allows for specifying the sender email address.

If not specified (null), the sender email address will default to  ``no-reply@`` + :ref:`LogbeeFrontendDomain <on-premises/logbee-frontend/configuration:LogbeeFrontendDomain>` (e.g., ``no-reply@logbee.dev``).

.. code-block:: json
    
    {
        "Smtp": {
            "Sender": {
                "Address": "no-reply@logbee.dev",
                "DisplayName": "Logbee"
            }
        }
    }

.. admonition:: Troubleshooting emails
    :class: note

    Even if an email is successfully sent using the configured SMTP service, the delivery can be affected by several factors.

    The reputation of the sender email address (e.g., ``no-reply@your-logbee-domain.com``) plays a significant role in email delivery.
    Email providers may reject or flag emails from senders with poor reputations.

    **Recommendations:**

    - Use a reputable SMTP service (e.g., SendGrid, Postmark).

    - Ensure your domain is authenticated to improve email delivery.

    - If you have an email address with a good reputation, you can use it under the ``Smtp.Sender.Address`` configuration.

    **Useful links:**

    - `How to Set Up Domain Authentication <https://www.twilio.com/docs/sendgrid/ui/account-and-settings/how-to-set-up-domain-authentication>`_ (SendGrid)

    - `Checking your Sender Score <https://www.senderscore.org/>`_  - Sender Score gives you an idea of how email providers view your IP address, providing insights into how likely your emails are to reach inboxes.

    - `Google Postmaster Tools <https://postmaster.google.com/>`_ - Google's platform to help senders track email performance, including the reputation of your domain and delivery issues.

    - `Google Header Analyzer <https://toolbox.googleapps.com/apps/messageheader/analyzeheader>`_ - can be used to find out how long an email spent in a particular location.

UserInterface
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
    
    {
        "UserInterface": {
            "NumberOfApplicationsToPreloadOnTheDashboardPage": 6
        }
    }

.. list-table::
   :header-rows: 1

   * - UserInterface.NumberOfApplicationsToPreloadOnTheDashboardPage
   * - Specifies how many applications should be preloaded under the ``/Dashboard`` page.

Authorization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
    
    {
        "Authorization": {
            "HS256Secret": "00000000-0000-0000-0000-000000000000-00000000-0000-0000-0000-000000000000",
            "AzureActiveDirectory": {}
        }
    }

.. list-table::
   :header-rows: 1

   * - Authorization.HS256Secret
   * - Represents the signature key of the authentication JSON Web Token (JWT).
       
       The authentication JWT must be signed with the secret provided in this property.

       More details about authentication can be found :ref:`here <on-premises/logbee-frontend/index:authentication>`.

Authorization.AzureActiveDirectory
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Optional configuration used to set up Azure Active Directory authentication.

More details about Azure Active Directory authentication can be found :doc:`here </on-premises/logbee-frontend/active-directory-auth/index>`.

.. code-block:: json
    
    {
        "Authorization": {
            "AzureActiveDirectory": {
                "ClientId": "<AD Application (client) ID>",
                "ClientSecret": "<secret>",
                "Authority": "https://login.microsoftonline.com/<AD Directory (tenant) ID>/v2.0/",
                "AuthorizedGroupIds": ["50679da3-74f4-4592-961c-9423257350dc"]
            }
        }
    }

ApplicationSettings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
    
    {
        "ApplicationSettings": {
            "CacheApplicationsRepositoryInSeconds": 600,
            "ProcessEmailsQueueIntervalInSeconds": 30,
            "DeleteProcessedEmails": {},
            "DeleteLocalLogFiles": {}
        }
    }

.. list-table::
   :header-rows: 1

   * - ApplicationSettings.CacheApplicationsRepositoryInSeconds
   * - Specifies the number of seconds for which the applications should be cached after they have been fetched from the database.
       This setting has no visible impact on the user interface but improves database performance, especially when using Azure CosmosDB.
       If the value is set to `0`, caching will be disabled.

.. list-table::
   :header-rows: 1

   * - ApplicationSettings.ProcessEmailsQueueIntervalInSeconds
   * - Defines the interval (in seconds) at which the system processes emails in the queue. A lower value increases responsiveness but may lead to more frequent database queries.

ApplicationSettings.DeleteProcessedEmails
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: json
    
    {
        "ApplicationSettings": {
            "DeleteProcessedEmails": {
                "SentMoreThanNHoursAgo": 12,
                "TriggerIntervalInMinutes": 60
            }
        }
    }

.. list-table::
   :header-rows: 1

   * - DeleteProcessedEmails.SentMoreThanNHoursAgo
   * - Specifies the number of hours after which sent and processed emails should be deleted. Helps prevent database bloat by automatically removing old emails.

.. list-table::
   :header-rows: 1

   * - DeleteProcessedEmails.TriggerIntervalInMinutes
   * - Defines how often (in minutes) the system checks and removes processed emails that meet the criteria. A lower value ensures frequent cleanup but may increase database load.

ApplicationSettings.DeleteLocalLogFiles
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: json
    
    {
        "ApplicationSettings": {
            "DeleteLocalLogFiles": {
                "CreatedMoreThanNDaysAgo": 3,
                "TriggerIntervalInHours": 6
            }
        }
    }

.. list-table::
   :header-rows: 1

   * - DeleteLocalLogFiles.CreatedMoreThanNDaysAgo
   * - The number of days after which local log files should be deleted.

.. list-table::
   :header-rows: 1

   * - DeleteLocalLogFiles.TriggerIntervalInHours
   * - The interval (in hours) at which the cleanup process runs.
