Update guide
========================

.. contents:: Table of contents
   :local:

Artifacts
-------------------------------------------------------

- logbee.Backend-{version}-win-x64.zip
- logbee.Frontend-{version}-win-x64.zip

Artifacts can be downloaded from `https://github.com/catalingavan/logbee-app <https://github.com/catalingavan/logbee-app>`_.


Update steps
-------------------------------------------------------

1) Stop both IIS applications:

   * logbee.Backend
   * logbee.Frontend

Create backup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2) Create a backup of the existing IIS application folders.

   .. code-block:: none

       \Backups
       ├── logbee.Frontend\
       └── logbee.Backend\

Copy new files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3) Delete from the **logbee.Backend** folder everything but:

   .. code-block:: none

       \logbee.Backend
       ├── Configuration\
       ├── appsettings.json
       └── web.config

4) Delete from the **logbee.Frontend** folder everything but:

   .. code-block:: none

       \logbee.Frontend
       ├── Configuration\
       ├── appsettings.json
       └── web.config

5) Extract the contents of ``logbee.Backend.AspNetCore.zip`` to logbee.Backend folder.

   Choose **not to override** the existing files (keep the local files).

6) Extract the contents of ``logbee.Frontend.AspNetCore.zip`` to logbee.Frontend folder.

   Choose **not to override** the existing files (keep the local files).

Update logbee.Backend configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

7) Apply the configuration changes (if any) by updating logbee.json file.

   The configuration changes will be listed in the :doc:`change log </on-premises/logbee-backend/change-log>`.


Update logbee.Frontend configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

8) Apply the configuration changes (if any) by updating logbee.json file.

   The configuration changes will be listed in the :doc:`change log </on-premises/logbee-frontend/change-log>`.

9) Change/increment the value of ``"StaticResourcesVersion"`` property from ``logbee.json`` file.

   This will refresh the browser cache for static resources.

   .. code-block:: json
       :emphasize-lines: 3
       :caption: C:\\inetpub\\wwwroot\\logbee.Frontend\\Configuration\\logbee.json

       {
           "LogBeeFrontendDomain": "logBee.dev",
           "StaticResourcesVersion": "any-new-value"
       }

Start the applications
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

10) Restart the IIS applications:

    * logbee.Backend
    * logbee.Frontend

11) Make a single request to logbee.Backend root URL (http://logbee-backend.your_domain.com).
   
    If everything went successful, you will see the logbee.Backend home page.
   
    .. figure:: images/installation-guide/logbee.Backend-running.png
        :alt: logbee.Backend home page

12) Make a single request to the logbee.Frontend root URL (http://logBee-frontend.your_domain.com).
   
    If the startup process went successful, you will see the home page.
   
    .. figure:: images/installation-guide/logbee.Frontend-running.png
        :alt: logbee.Frontend home page

