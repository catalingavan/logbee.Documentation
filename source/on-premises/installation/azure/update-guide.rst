Update guide
========================

.. contents:: Table of contents
   :local:

Because we are using the containerized version of Logbee, the update process is simplified.

Stop the App Services
-------------------------------------------------------

Stop both of the App Services.

Download existing configuration
-------------------------------------------------------

Under the Azure Storage account, go to the **logbee-config** File share and download the existing configuration files.

.. code-block:: none

    /logbee-config
    ├── backend.logbee.json
    └── frontend.logbee.json

Apply configuration changes
-------------------------------------------------------

Update the **backend.logbee.json** file by applying the configuration changes (if any).

The configuration changes will be listed in the :doc:`Logbee.Backend change log </on-premises/logbee-backend/change-log>`.

Update the **frontend.logbee.json** file by applying the configuration changes (if any).

The configuration changes will be listed in the :doc:`Logbee.Frontend change log </on-premises/logbee-frontend/change-log>`.

Regardless if there are any changes for the Logbee.Frontend, you need to change/increment the value of ``"StaticResourcesVersion"`` property from **frontend.logbee.json** file.

.. code-block:: json
    
    {
      "StaticResourcesVersion": "2.0.1"
    }

This change will refresh the browser cache for static resources used by the Logbee.Frontend application.

Upload the configuration files
-------------------------------------------------------

Upload the updated configuration files to the **logbee-config** File share.

Update the containers configuration
-------------------------------------------------------

Navigate to the Logbee.Backend App Service, and go to **Deployment > Deployment Center**.

Update the container **Config** by changing the image tag.

.. figure:: images/logbee-backend-app-service-update.png
    :alt: Logbee Backend App Service update


Navigate to the Logbee.Frontend App Service, and go to **Deployment > Deployment Center**.

Update the container **Config** by changing the image tag.


Start the App Services
-------------------------------------------------------

Start with Logbee.Backend App Service and followed by Logbee.Frontend App Service.

