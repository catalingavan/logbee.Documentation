Configuration
=================================

logbee.Frontend behavior can be customized by updating the ``Configuration\logbee.json`` file.

A full example of the ``logbee.json`` configuration file can be found `here <https://github.com/catalingavan/logbee-app/blob/main/logbee.Frontend/logbee.json>`_.

.. contents:: Configuration options
   :local:

LogBeeFrontendDomain
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The domain of the logbee.Frontend application. This value is used in multiple places:

- when generating the HTML titles
- sets the default email domain for the authenticated users (``user1@logbee.dev``).
- setting the sender email address to ``no-reply@`` + :ref:`LogBeeFrontendDomain <on-premises/logbee-frontend/configuration:LogBeeFrontendDomain>` (e.g., ``no-reply@logbee.dev``), if no other value is specified under the :ref:`Smtp.Sender <on-premises/logbee-frontend/configuration:Smtp.Sender>` configuration.

.. code-block:: json
    
    {
        "LogBeeFrontendDomain": "logbee.dev"
    }

StaticResourcesVersion
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Property used as query string parameter when loading static resources (css/javascript).

This property should be changed after an application update in order to invalidate the static files cache.

.. code-block:: json
    
    {
        "StaticResourcesVersion": "1.0.0"
    }

LogBeeBackendUrl
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Root url pointing to logbee.Backend application.

.. code-block:: json
    
    {
        "LogBeeBackendUrl": "http://logbee-backend.myapp.com/"
    }

LogBeeFrontendUrl
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Root url pointing to logbee.Frontend application.

.. code-block:: json
    
    {
        "LogBeeFrontendUrl": "http://logBee.myapp.com/"
    }

LogBeeBackend.BasicAuth.Password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Basic HTTP authentication scheme password used to connect to logbee.Backend application.

This property should have the same value as the same property from ``logbee.Backend\Configuration\logbee.json``.

.. code-block:: json
    
    {
        "LogBeeBackend.BasicAuth.Password": "_LogBeeBackend_authorization_password_"
    }

LogBeeFrontend.BasicAuth.Password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Basic HTTP authentication scheme password used to connect to logbee.Frontend application.

.. code-block:: json
    
    {
        "LogBeeFrontend.BasicAuth.Password": "_LogBeeFrontend_authorization_password_"
    }

LogBee.License
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The LogBee on-premises license key. Can be null.

.. code-block:: json
    
    {
        "LogBee.License": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJMaWNlbnNlSWQiOiIzNTczMDI1My00NGRhLTRiZmMtOGQ0MS1iMzUzMDRkZWUyMzciLCJMaWNlbnNlVHlwZSI6IkVudGVycHJpc2UifQ.K4htH3YOulrpVrkTJuHza81VrYloYvTsfRYzb4fpUYI"
    }


Database
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
    
    {
        "Database": {
            "Provider": "MongoDb",
            "MongoDb": {},
            "MySql": {},
            "SqlServer": {}
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

Database.MongoDb
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration used to connect to MongoDb database.

.. code-block:: json
    
    {
        "Database": {
            "MongoDb": {
                "ConnectionString": "mongodb://localhost:27017?socketTimeoutMS=5000&connectTimeoutMS=5000",
                "DatabaseName": "LogBeeFrontend"
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
                "ConnectionString": "server=localhost;port=3306;database=LogBeeFrontend;uid=<replace_user>;password=<replace_password>;Charset=utf8;"
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
                "ConnectionString": "Server=localhost;Database=LogBeeFrontend;User ID=<replace_user>;Password=<replace_password>;TrustServerCertificate=True;"
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

If not specified (null), the sender email address will default to  ``no-reply@`` + :ref:`LogBeeFrontendDomain <on-premises/logbee-frontend/configuration:LogBeeFrontendDomain>` (e.g., ``no-reply@logbee.dev``).

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

More details can be found :ref:`here <on-premises/logbee-frontend/index:Azure Active Directory>`.

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

