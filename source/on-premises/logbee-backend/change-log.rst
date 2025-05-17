Change log
===============

.. contents:: Versions
   :local:
   :depth: 1

logbee.Backend 2.1.1
--------------------------

Release date: 17-05-2025

https://github.com/catalingavan/logbee-app/releases/tag/logbee.Backend-v2.1.1

Docker image: catalingavan/logbee.backend:2.1.1

This release is compatible with :ref:`logbee.Frontend-v2.1.1 <on-premises/logbee-frontend/change-log:logbee.Frontend 2.1.1>` or newer versions.

|

**Introduced Interceptors:**

- Interceptors are JavaScript functions that run before each request is logged.
- Useful for filtering out unwanted or repetitive requests (e.g., health checks).
- Interceptors receive a ``context`` object and a ``next()`` function.
- If ``next()`` is called, the request is logged; otherwise, it's skipped.

**Interceptor JavaScript example**

.. code-block:: js
    
    function(context, next) {
      const statusCode = context.requestLog.httpProperties.response.httpStatusCode;
      const localPath = context.requestLog.httpProperties.url.path.toLowerCase();

      if (localPath === "/status/ping" && statusCode === 200) {
         // Skip logging successful health check requests
         return;
      }

      // Continue logging
      next();
   }



logbee.Backend 2.0.0
--------------------------

Release date: 01-04-2025

https://github.com/catalingavan/logbee-app/releases/tag/logbee.Backend-v2.0.0

Docker image: catalingavan/logbee.backend:2.0.0

|

**Improvements:**

- | Refactored the application to fully support a containerized architecture.
  | For more details, see the blog post: https://logbee.net/Blog/1000/migrating-a-web-application-from-windows-to-ubuntu 

- | Completely redesigned the Azure implementation and deployment process, making it more efficient and easier to set up.
  | For more details, see the :doc:`Microsoft Azure installation guide </on-premises/installation/azure/installation-guide>`.

**Configuration breaking changes:**

- Added required :ref:`LogbeeBackendConfigurationFilePath <on-premises/logbee-backend/configuration:LogbeeBackendConfigurationFilePath>` configuration property.

.. code-block:: json
    
    {
        "LogbeeFrontendConfigurationFilePath": "../../logbee.frontend/Configuration/logbee.json"
    }

- ``"Files"`` property has been renamed to ``"FileStorage"``

- Added :ref:`FileStorage.MongoDb <on-premises/logbee-backend/configuration:FileStorage.MongoDb>` configuration property which is required when ``FileStorage.Provider = "MongoDb"``

.. code-block:: json
    
    {
        "FileStorage": {
            "Provider": "MongoDb",
            "MongoDb": {
               "ConnectionString": "mongodb://localhost:27017?socketTimeoutMS=5000&connectTimeoutMS=5000",
               "DatabaseName": "LogbeeBackend"
            }
        }
    }

**Configuration changes:**

- Removed the following properties:

.. code-block:: json
    
    {
        "LogbeeFrontendUrl": "http://logbee-frontend.your_domain.com",
        "LogbeeFrontend.BasicAuth.Password": "_LogBeeFrontend_authorization_password_",
        "LogbeeFrontend": {}
    }


logbee.Backend 1.3.0
--------------------------

Release date: 29-10-2024

https://github.com/catalingavan/logbee-app/releases/tag/logbee.Backend-v1.3.0

**Bug fixes**

Fixes ``405 Method Not Allowed`` when trying to delete captured application logs and data. https://stackoverflow.com/questions/4413455/why-does-http-delete-verb-return-405-error-method-not-allowed-for-my-restful-w

This bug prevented Logbee.Backend from deleting expiring logs and related metrics in certain scenarios, leading to indefinite database growth.

**Improvements**

- Implemented support for OpenTelemetry integration.

- Implemented automated job for deleting old local log files.

**Breaking changes**

Added OpenTelemetry (required) configuration option. `logbee.json <https://github.com/catalingavan/logbee-app/blob/c370ce6c529302bb9121e0fea37c650803e4a850/logbee.Backend/logbee.json#L239>`_

.. code-block:: json
    
   {
      "OpenTelemetry": {
         "Trace": {
            "MaximumNumberOfSpansPerRequest": 100,
            "CreateTraceRateLimit": {
               "IsEnabled": true,
               "Limit": {
                  "RequestLimit": 5,
                  "IntervalInSeconds": 1
               }
            }
         },
         "Logs": {
            "MaximumNumberOfSpansPerRequest": 100,
            "CreateLogsRateLimit": {
               "IsEnabled": true,
               "Limit": {
                  "RequestLimit": 5,
                  "IntervalInSeconds": 1
               }
            }
         }
      }
   }

Added additional (required) ApplicationSettings configuration options. `logbee.json <https://github.com/catalingavan/logbee-app/blob/c370ce6c529302bb9121e0fea37c650803e4a850/logbee.Backend/logbee.json#L276>`_

.. code-block:: json

   {
      "ApplicationSettings": {
         "ProcessAsyncRequestLogs": {
            "MaximumDurationInMilliseconds": 5000,
            "TriggerIntervalInSeconds": 5,
            "Take": 50
         },
         "DeleteLocalLogFiles": {
            "CreatedMoreThanNDaysAgo": 3,
            "TriggerIntervalInHours": 6
         }
      }
   }

logbee.Backend 1.1.0
--------------------------

Release date: 06-06-2024

https://github.com/catalingavan/logbee-app/releases/tag/logBee.Backend-v1.1.0

Upgraded the application to .NET 8.0

logbee.Backend 1.0.1
--------------------------

Release date: 15-03-2024

https://github.com/catalingavan/logbee-app/releases/tag/logBee.Backend-v1.0.1
