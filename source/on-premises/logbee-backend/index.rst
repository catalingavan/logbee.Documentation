Logbee.Backend
=================================

About
------------------------------

Logbee.Backend is the core service responsible for **storing, managing, and centralizing logs and application metrics**.  

The service is designed for **high availability and scalability**, ensuring efficient log processing even under heavy loads.  

Logbee.Backend supports two data storage options:

- :ref:`Mongo DB <on-premises/logbee-backend/configuration:Database.MongoDb>`

- :ref:`Azure CosmosDB <on-premises/logbee-backend/configuration:Database.AzureCosmosDb>`

Integration examples
------------------------------

Software applications can send logs to Logbee.Backend through various :doc:`integration </integrations/index>` options.

**.NET**

.. code-block:: c#

    using Serilog;
    using Serilog.Sinks.LogBee;

    Log.Logger = new LoggerConfiguration()
        .WriteTo.LogBee(new LogBeeApiKey(
          "_OrganizationId_",
          "_ApplicationId_",
          "https://logbee-backend.your_domain.com")
        )
        .CreateLogger();


**Node.js**

.. code-block:: js

    const express = require('express');
    const { logbee } = require('@logbee/express');

    const app = express();

    app.use(logbee.middleware({
        organizationId: '_OrganizationId_',
        applicationId: '_ApplicationId_',
        logbeeApiUri: 'https://logbee-backend.your_domain.com'
    }));


**RESTful API**

.. code-block:: none
    
    POST https://logbee-backend.your_domain.com/request-logs
    {
      "organizationId": "_OrganizationId_",
      "applicationId": "_ApplicationId_",
      "startedAt": "2024-03-11T14:36:31.108Z",
      "durationInMilliseconds": 41,
      "httpProperties": {
        "absoluteUri": "http://localhost/catalog/createProduct?locale=en",
        "method": "POST",
        "response": {
          "statusCode": 200
        }
      },
      "logs": [
        {
          "logLevel": "Information",
          "message": "Creating product..."
        }
      ]
    }

.. toctree::
   :hidden:
   :maxdepth: 2
   :titlesonly:
   :includehidden:

   configuration
   change-log
