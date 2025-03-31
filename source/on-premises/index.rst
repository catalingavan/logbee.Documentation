Logbee (on-premises)
=============================

Logbee is a powerful logging solution that can be hosted on-premises, giving you full control over your data.

Some of the advantages of hosting Logbee locally:

.. list-table::
   :header-rows: 0
   :class: table-layout-normal

   * - ✅ **Data Privacy**
     - All logs are stored securely within your in-house servers, ensuring privacy and compliance with organizational policies.
   * - ✅ **Cross-platform hosting**
     - Deploy on Windows, Linux, Azure, or as a containerized application.
   * - ✅ **Easy configuration**
     - Logbee is customizable through simple JSON configuration files.
   * - ✅ **Developer-Friendly**
     - Easy to set up and integrate with your existing applications.

Artifacts
------------------------

Logbee artifacts can be downloaded from the https://github.com/catalingavan/logbee-app/releases page.

Deployment environments
------------------------------

Logbee can be deployed in three different environments.

Local Server
~~~~~~~~~~~~~~~~~~~~~~~~

Ideal for organizations with existing on-premises infrastructure.

**Requirements:**

- IIS Web Server with `ASP.NET Core Runtime 8 <https://dotnet.microsoft.com/en-us/download/dotnet/8.0>`_ installed
- `MongoDB Community Server <https://www.mongodb.com/try/download/community>`_ (version >= 8.0.x)

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

