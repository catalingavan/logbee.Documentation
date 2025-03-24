Configuration
=================================

logbee.Backend behavior can be customized by updating the ``Configuration\logbee.json`` file.

A full example of the ``logbee.json`` configuration file can be found `here <https://github.com/catalingavan/logbee-app/blob/main/logbee.Backend/logbee.json>`_.

.. contents:: Configuration options
   :local:

LogbeeFrontendConfigurationFilePath
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The path to the Logbee.Frontend configuration file. The value can be absolute or relative.

.. code-block:: json
    
    {
        "LogbeeFrontendConfigurationFilePath": "../../logbee.Frontend/Configuration/logbee.json"
    }

LogbeeBackendUrl
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Root url pointing to Logbee.Backend application.

.. code-block:: json
    
    {
        "LogBeeBackendUrl": "http://logbee-backend.myapp.com/"
    }


LogbeeBackend.BasicAuth.Password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Basic HTTP authentication scheme password used to connect to Logbee.Backend application.

.. code-block:: json
    
    {
        "LogbeeBackend.BasicAuth.Password": "_LogbeeBackend_authorization_password_"
    }

Database
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
    
    {
        "Database": {
            "Provider": "MongoDb",
            "MongoDb": {},
            "AzureCosmosDb": {}
        }
    }

.. list-table::
   :header-rows: 1

   * - Database.Provider
     - 
   * - MongoDb
     - Sets the database provider to MongoDB.
   * - AzureCosmosDb
     - Sets the database provider to Azure CosmosDB.

.. list-table::
   :header-rows: 1

   * - Database.MongoDb
   * - Required when "Database.Provider" is "MongoDb".

.. list-table::
   :header-rows: 1

   * - Database.AzureCosmosDb
   * - Required true when "Database.Provider" is "AzureCosmosDb".

Database.MongoDb
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration used to connect to MongoDB server.

.. code-block:: json
    
    {
        "Database": {
            "MongoDb": {
                "ConnectionString": "mongodb://localhost:27017?socketTimeoutMS=5000&connectTimeoutMS=5000",
                "DatabaseName": "LogbeeBackend"
            },
        }
    }

Database.AzureCosmosDb
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration used to connect to Azure Cosmos DB service.

.. code-block:: json
    
    {
        "Database": {
            "AzureCosmosDb": {
                "ApplicationRegion": "West Europe",
                "ConnectionString": "https://cosmos-db-name.documents.azure.com:443/;AccountKey=_accountKeyValue_;",
                "DatabaseName": "LogbeeBackend",
                "AzureStorageAccountConnectionString": "DefaultEndpointsProtocol=https;AccountName=storagename;AccountKey=_accountKeyValue_;EndpointSuffix=core.windows.net"
            },
        }
    }


FileStorage
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
    
    {
        "FileStorage": {
            "Provider": "MongoDb",
            "MaximumFileSizeInBytes": 2097152,
            "MongoDb": {},
            "Azure": {}
        }
    }

.. list-table::
   :header-rows: 1

   * - FileStorage.Provider
     - 
   * - MongoDb
     - Sets the files storage provider to MongoDB.
   * - Azure
     - Sets the files storage provider to Azure Storage container.

.. list-table::
   :header-rows: 1

   * - FileStorage.MaximumFileSizeInBytes
   * - Specifies the maximum file size (in bytes) which can be uploaded.

.. list-table::
   :header-rows: 1

   * - FileStorage.MongoDb
   * - Required  when "FileStorage.Provider" is "MongoDb"

.. list-table::
   :header-rows: 1

   * - FileStorage.Azure
   * - Required  when "FileStorage.Provider" is "Azure"

FileStorage.MongoDb
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration for saving file blobs to MongoDb.

.. code-block:: json
    
    {
        "FileStorage": {
            "MongoDb": {
                "ConnectionString": "mongodb://localhost:27017?socketTimeoutMS=5000&connectTimeoutMS=5000",
                "DatabaseName": "LogbeeBackend"
            }
        }
    }

FileStorage.Azure
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration used to connect to Azure Storage account.

.. code-block:: json
    
    {
        "FileStorage": {
            "Azure": {
                "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=storagename;AccountKey=_accountKeyValue_;EndpointSuffix=core.windows.net"
            }
        }
    }

CreateRequestLog
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
    
    {
        "CreateRequestLog": {
            "ValidateApplicationKeys": true,
            "SaveInputStreamAsFileIfLengthGte": 5000,
            "Ignore": {},
            "Obfuscate": {},
            "Truncate": {},
            "Throttle": {}
        }
    }

.. list-table::
   :header-rows: 1

   * - CreateRequestLog.ValidateApplicationKeys
   * - If true, the ``"ApplicationId"`` and ``"OrganizationId"`` are validated against existing records from the LogBee.Frontend database.
       
       This is useful if you want to prevent processing logs from applications which have been deleted in the LogBee.Frontend user-interface, but are still running.

.. list-table::
   :header-rows: 1

   * - CreateRequestLog.SaveInputStreamAsFileIfLengthGte
   * - If Request.InputStream content exceeds the length defined here, the value will be saved as a blob file.
       
       This helps prevent saving excesive large objects in database.

CreateRequestLog.Ignore
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: json
    
    {
        "CreateRequestLog": {
            "Ignore": {
                "UrlPathPatterns": [ "(?si).js$", "(?si).css$", "(?si).map$", "(?si).xml$", "(?si).php$", "(?si).ttf" ],
                "ResponseContentTypePatterns": [ "(?si)^application/javascript", "(?si)^image/", "(?si)^application/font-" ]
            }
        }
    }

.. list-table::
   :header-rows: 1

   * - Ignore.UrlPathPatterns
   * - An array of Regex patterns used to identify requests which should be ignored based on the url path.

.. list-table::
   :header-rows: 1

   * - Ignore.ResponseContentTypePatterns
   * - An array of Regex patterns used to identify requests which should be ignored based on the ``Response.Content-Type`` header.

CreateRequestLog.Obfuscate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: json
    
    {
        "CreateRequestLog": {
            "Obfuscate": {
                "IsEnabled": true,
                "ObfuscateInputStream": false,
                "Placeholder": "<obfuscated>",
                "Patterns": [ "(?si)pass" ]
            }
        }
    }

.. list-table::
   :header-rows: 1

   * - Obfuscate.IsEnabled
     -
   * - true
     - Request parameters are parsed and any matching properties will be obfuscated.
   * - false
     - Obfuscation service is disabled.

.. list-table::
   :header-rows: 1

   * - Obfuscate.ObfuscateInputStream
     -
   * - true
     - ``Request.InputStream`` will be parsed and any matching properties will be obfuscated.
       
       This method is expensive and can affect the latency of the application.
   * - false
     - ``Request.InputStream`` will not be parsed.

.. list-table::
   :header-rows: 1

   * - Obfuscate.Placeholder
   * - Placeholder used to replace the sensitive properties matched by the Regex patterns.

.. list-table::
   :header-rows: 1

   * - Obfuscate.Patterns
   * - An array of Regex patters which are used to identify potential sensitive data.

CreateRequestLog.Truncate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration used to truncate request log payloads.

Before saving to database, the request log will be truncated using the limits provided by this configuration.

.. code-block:: json
    
    {
        "CreateRequestLog": {
            "Truncate": {
                "Files": {
                    "Limit": 5
                },
                "LogMessages": {
                    "Limit": 100,
                    "MessageMaxLength": 10000
                },
                "Exceptions": {
                    "Limit": 6,
                    "ExceptionMessageMaxLength": 500
                },
                "CustomProperties": {
                    "Limit": 10,
                    "KeyMaxLength": 20,
                    "ValueMaxLength": 100
                },
                "Keywords": {
                    "Limit": 6,
                    "KeywordMinLength": 5,
                    "KeywordMaxLength": 30
                },
                "RequestHeaders": {
                    "Limit": 20,
                    "KeyMaxLength": 100,
                    "ValueMaxLength": 1000
                },
                "RequestCookies": {
                    "Limit": 5,
                    "KeyMaxLength": 100,
                    "ValueMaxLength": 100
                },
                "RequestQueryString": {
                    "Limit": 30,
                    "KeyMaxLength": 100,
                    "ValueMaxLength": 1000
                },
                "RequestFormData": {
                    "Limit": 30,
                    "KeyMaxLength": 100,
                    "ValueMaxLength": 1000
                },
                "RequestServerVariables": {
                    "Limit": 30,
                    "KeyMaxLength": 100,
                    "ValueMaxLength": 1000
                },
                "RequestClaims": {
                    "Limit": 30,
                    "KeyMaxLength": 100,
                    "ValueMaxLength": 1000
                },
                "ResponseHeaders": {
                    "Limit": 30,
                    "KeyMaxLength": 100,
                    "ValueMaxLength": 1000
                }
            }
        }
    }

CreateRequestLog.Throttle
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: json
    
    {
        "CreateRequestLog": {
            "Throttle": {
                "Rules": [
                    {
                        "IsEnabled": false,
                        "Organizations": ["a754e353-a0f9-48ae-ad11-66470c70d0bf"],
                        "Applications": ["26e1cf75-5ad7-49cc-b48e-798b49dc41ba"],
                        "RemoteIpAddresses": ["2.127.71.193", "228.137.250.192"],
                        "Limits": [
                            {
                                "RequestLimit": 1,
                                "IntervalInSeconds": 5,
                                "StatusCodeLt": 400
                            }
                        ]
                    }
                ]
            }
        }
    }

.. list-table::
   :header-rows: 1

   * - Throttle.Rules[]
   * - A list of throttle rules to be applied when receiving a request log.
       
       A rule can specify only one of ``Organizations``, ``Applications`` or ``RemoteIpAddresses`` filters.
       
       If a rule has no filters specified, the rule will apply for all the incoming requests.

.. list-table::
   :header-rows: 1

   * - Throttle.Rules[]
     -
   * - IsEnabled
     - Specifies if the rule is enabled.
   * - Organizations
     - An array of organization ids for which the rule will apply.
   * - Applications
     - An array of application ids for which the rule will apply.
   * - RemoteIpAddresses
     - An array of IP addresses for which the rule will apply.
   * - Limits[]
     - A list of throttle limits to be applied for the rule.

.. list-table::
   :header-rows: 1

   * - Throttle.Rules[].Limits[]
     -
   * - RequestLimit
     - Specifies how many requests should be accepted in the specified interval of time.
   * - IntervalInSeconds
     - Specifies the interval of time, in seconds, when the request limit is calculated.
   * - StatusCodeLt
     - Specifies the "< Status Code" for which the request limit is applied.


UrlTokenization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json
    
    {
        "UrlTokenization": {
            "IgnoreTokenizationUrlPathPatterns": [ "(?si)^\/[0-9]+$" ],
            "PathComponentTokenization": {
                "Characters": [ "%", " ", ":", ",", ";", "+", "%", "&", "#", "(", ")", "@", "=", "<", ">", "{", "}", "\"", "'" ],
                "Patterns": [ "(?si)(?:\\D*\\d){3}" ]
            },
        }
    }

.. list-table::
   :header-rows: 1

   * - UrlTokenization.IgnoreTokenizationUrlPathPatterns
   * - An array of Regex patterns for which the url tokenization will not be activated.
       
       .. code-block:: none

           Example: [ "(?si)^\/home\/error-(?:[0-9])+$" ]
           Because the url "/Home/Error-404" is matched by the regex, url tokenization will not be activated.

           "/Home/Error-404" ---> "/Home/Error-404"       
       
.. list-table::
   :header-rows: 1

   * - UrlTokenization.PathComponentTokenization.Characters
   * - If an url path contains any of the specified characters in this array, the path will be considered a parameter.

       .. code-block:: none

           Example: [ ":" ]
           Because the url path "/D1:P7:00A" contains ":" character, it will be considered a parameter.

           "/api/reports/generate/D1:P7:00A" ---> "/api/reports/generate/{0}"


.. list-table::
   :header-rows: 1

   * - UrlTokenization.PathComponentTokenization.Patterns
   * - An array of Regex patterns used to identify parameters inside url paths

       .. code-block:: none

           Example: [ "(?si)(?:\\D*\\d){3}" ]
           Because the url path "/APP-002" is matched by the regex (contains 3 digits), it will be considered a parameter.

           "/api/reports/generate/APP-002" ---> "/api/reports/generate/{0}"


TimeToLive
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Specifies for how long the captured logs and other data entities should be kept in database.

The time to live value can be specified in ``Days``, ``Hours`` or ``Minutes``. 

.. code-block:: json
    
    {
        "TimeToLive": {
            "RequestLog": [
                {
                    "StatusCodeLt": 400,
                    "Minutes": 2880
                },
                {
                    "StatusCodeLt": 500,
                    "Hours": 96
                },
                {
                    "StatusCodeLt": 600,
                    "Days": 6
                }
            ],
            "AlertDefinitionInvocation": {
                "Days": 30
            },
            "ApplicationAlert": {
                "Days": 30
            },
            "ApplicationChartData": {
                "Days": 30
            },
            "ApplicationData": {
                "Days": 30
            },
            "ApplicationEndpoint": {
                "Days": 30
            },
            "ApplicationException": {
                "Days": 30
            },
            "ApplicationUsage": {
                "Days": 180
            },
            "ApplicationUser": {
                "Days": 30
            },
            "HttpRefererDestination": {
                "Days": 30
            },
            "HttpRefererSource": {
                "Days": 30
            }
        }
    }

ApplicationSettings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ApplicationSettings.DeleteApplicationDataByExpiryDate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: json
    
    {
        "ApplicationSettings": {
            "DeleteApplicationDataByExpiryDate": {
                "MaximumDurationInMilliseconds": 5000,
                "TriggerIntervalInMinutes": 180
            }
        }
    }

.. list-table::
   :header-rows: 1

   * - DeleteApplicationDataByExpiryDate.TriggerIntervalInMinutes
   * - Specifies the interval of time in which the delete application data service is executed.

ApplicationSettings.ProcessQueues
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: json
    
    {
        "ApplicationSettings": {
            "ProcessQueues": {
                "MaximumDurationInMilliseconds": 5000,
                "TriggerIntervalInSeconds": 10,
                "Take": 100
            }
        }
    }

.. list-table::
   :header-rows: 1

   * - ProcessQueues.TriggerIntervalInSeconds
   * - Specifies the interval in which the entities saved in memory (queue) should be inserted in database.

.. list-table::
   :header-rows: 1

   * - ProcessQueues.Take
   * - Specifies how many items from queue should be processed at the specified interval of time.

ApplicationSettings.ProcessAlerts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: json
    
    {
        "ApplicationSettings": {
            "ProcessAlerts": {
                "MaximumDurationInMilliseconds": 5000,
                "TriggerIntervalInSeconds": 10
            }
        }
    }

.. list-table::
   :header-rows: 1

   * - ProcessAlerts.TriggerIntervalInSeconds
   * - Specifies the interval in which the alerts are evaluated against the received request logs.

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