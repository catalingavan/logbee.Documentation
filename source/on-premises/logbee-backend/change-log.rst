Change log
===============

.. contents:: Versions
   :local:
   :depth: 1

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
