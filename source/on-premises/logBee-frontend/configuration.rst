Configuration
=================================

logBee.Frontend behavior can be customized by updating the ``Configuration\logBee.json`` file.

A full example of the ``logBee.json`` configuration file can be found `here <https://github.com/catalingavan/logBee-app/blob/main/logBee.Frontend/logBee.json>`_.

.. contents:: Configuration options
   :local:

LogBeeFrontendDomain
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The domain of the logBee.Frontend application. This value is used in multiple places:

- when generating the HTML titles
- when sending alert emails, as the sender value (``no-reply@logbee.dev``)
- sets the default email domain for the authenticated users (``user1@logbee.dev``).

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

Root url pointing to logBee.Backend application.

.. code-block:: json
    
    {
        "LogBeeBackendUrl": "http://logBee-backend.myapp.com/"
    }

LogBeeFrontendUrl
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Root url pointing to logBee.Frontend application.

.. code-block:: json
    
    {
        "LogBeeFrontendUrl": "http://logBee.myapp.com/"
    }

LogBeeBackend.BasicAuth.Password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Basic HTTP authentication scheme password used to connect to logBee.Backend application.

This property should have the same value as the same property from ``logBee.Backend\Configuration\logBee.json``.

.. code-block:: json
    
    {
        "LogBeeBackend.BasicAuth.Password": "_LogBeeBackend_authorization_password_"
    }

LogBeeFrontend.BasicAuth.Password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Basic HTTP authentication scheme password used to connect to logBee.Frontend application.

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
            "Host": "smtp.sendgrid.net",
            "Port": 587,
            "UserName": "",
            "Password": "",
            "EnableSsl": false
        }
    }

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

       More details about authentication can be found :ref:`here <on-premises/logBee-frontend/index:authentication>`.

Authorization.AzureActiveDirectory
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Optional configuration used to set up Azure Active Directory authentication.

More details can be found :ref:`here <on-premises/logBee-frontend/index:Azure Active Directory>`.

.. code-block:: json
    
    {
        "Authorization": {
            "AzureActiveDirectory": {
                "ClientId": "<AD Application (client) ID>",
                "ClientSecret": "<secret>",
                "Authority": "https://login.microsoftonline.com/<AD Directory (tenant) ID>/v2.0/"
            }
        }
    }