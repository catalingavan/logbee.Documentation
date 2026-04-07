Logbee (on-premises)
=============================

Artifacts
------------------------

Logbee artifacts can be downloaded from the project's GitHub releases page: https://github.com/catalingavan/logbee-app/releases

Deployment options
------------------------------

Logbee can be deployed in three different setups.

Local Server
~~~~~~~~~~~~~~~~~~~~~~~~

Ideal for organizations with existing on-premises infrastructure.

**Requirements:**

- IIS Web Server with `ASP.NET Core Runtime 8 <https://dotnet.microsoft.com/en-us/download/dotnet/8.0>`_ installed
- `MongoDB Community Server <https://www.mongodb.com/try/download/community>`_ (version >= 8.0)

For detailed installation instructions, see :doc:`Local server installation guide </on-premises/installation/local-server/index>`.

Microsoft Azure
~~~~~~~~~~~~~~~~~~~~~~~~

Ideal for teams already using Azure for cloud hosting.

**Requirements:**

- Azure Cosmos DB
- Azure Storage Account
- Two App Services (Web Apps)

For detailed installation instructions, see :doc:`Azure installation guide </on-premises/installation/azure/index>`.

Docker
~~~~~~~~~~~~~~~~~~~~~~~~

Ideal for developers who prefer containerized deployments for portability and ease of scaling.

**Requirements:**

- `Docker Engine <https://docs.docker.com/engine/>`_

For detailed installation instructions, see :doc:`Docker installation guide </on-premises/installation/docker/index>`.

.. toctree::
   :hidden: 
   :maxdepth: 2
   :titlesonly:
   :includehidden:

   installation/index
   logbee-frontend/index
   logbee-backend/index

