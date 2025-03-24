Logbee.Backend
=================================

Logbee.Backend is the core service responsible for **storing, managing, and centralizing logs and application metrics**.  

The backend is designed for **high availability and scalability**, ensuring efficient log processing even under heavy loads.  

Software applications can send logs to Logbee.Backend through various :doc:`integration </integrations/index>` options.

.. code-block:: c#
    :caption: .NET application sending logs using Serilog

    using Serilog;
    using Serilog.Sinks.LogBee;

    Log.Logger = new LoggerConfiguration()
        .WriteTo.LogBee(new LogBeeApiKey(
          "_OrganizationId_",
          "_ApplicationId_",
          "https://logbee-backend.your_domain.com")
        )
        .CreateLogger();


.. code-block:: js
    :caption: Node.js ExpressJS application sending logs using @logbee/express

    const express = require('express');
    const { logbee } = require('@logbee/express');

    const app = express();

    app.use(logbee.middleware({
        organizationId: '_OrganizationId_',
        applicationId: '_ApplicationId_',
        logbeeApiUri: 'https://logbee-backend.your_domain.com'
    }));

.. code-block:: none
   :caption: Sending a request log via REST API:
    
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
