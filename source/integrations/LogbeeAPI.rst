Logbee API (REST)
============================

Logbee provides API for logging events and requests from your applications.

Hurl samples can be found on the `LogbeeAPI integrations page <https://github.com/catalingavan/logbee-integrations-examples/blob/main/LogbeeAPI>`_.

Create a Request Log (`schema <https://github.com/catalingavan/logbee-integrations-examples/blob/main/LogbeeAPI/openapi/requests/CreateRequestLogPayload.yaml>`_)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------

.. code-block:: none

    POST https://api.logbee.net/request-logs
    Content-Type: application/json
    {
      "organizationId": "_OrganizationId_",
      "applicationId": "_ApplicationId_",
      "startedAt": "2024-10-09T15:54:17.488Z",
      "httpProperties": {
        "absoluteUri": "http://localhost/hello",
        "method": "GET",
        "response": {
          "statusCode": 200
        }
      },
      "logs": [
        {
          "logLevel": "Information",
          "message": "My first log message"
        }
      ]
    }

Response
~~~~~~~~~~~~~~~~~~~~

.. code-block:: none

    {
      "id": "a2f1300a-b786-49e0-a556-3e6a2440ac60",
      "organizationId": "_OrganizationId_",
      "applicationId": "_ApplicationId_"
    }
