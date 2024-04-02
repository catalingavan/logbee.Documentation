REST
============================

You can create logs by consuming the REST endpoint provided by logBee (https://api.logbee.net).

.. code-block:: json
    :caption: POST https://api.logbee.net/request-logs

    {
      "organizationId": "__OrganizationId__",
      "applicationId": "__ApplicationId__",
      "startedAt": "2024-04-02T07:07:53.092Z",
      "durationInMilliseconds": 41,
      "httpProperties": {
        "absoluteUri": "http://localhost/catalog/createProduct?tokenId=20420&locale=en",
        "method": "POST",
        "request": {
          "headers": {
            "Content-Type": "application/x-www-form-urlencoded"
          },
          "formData": {
            "category_id": "100",
            "product_name": "DVD player"
          }
        },
        "response": {
          "statusCode": 302,
          "headers": {
            "Location": "/catalog/products?productId=208"
          }
        }
      },
      "logs": [
        {
          "logLevel": "Information",
          "message": "Creating product 'DVD player' ..."
        },
        {
          "logLevel": "Error",
          "message": "Domain.Exceptions.CategoryNotFoundException: Product category (id=100) does not exist"
        },
        {
          "logLevel": "Warning",
          "message": "Will attach the product to the default category"
        },
        {
          "logLevel": "Information",
          "message": "Product has been saved in database. ProductId: 208"
        }
      ],
      "keywords": [
        "product:208",
        "category:100"
      ]
    }

Table of Contents
------------------

.. toctree::
   :maxdepth: 2
   :titlesonly:
   :includehidden:

   openApi-schema