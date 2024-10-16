Docker
=============================

Logbee can run as a Docker application, thanks to `Marcio <https://github.com/zimbres>`_ valuable contribution.

The official Logbee Docker repositories are the following:

- https://hub.docker.com/r/catalingavan/logbee.backend

- https://hub.docker.com/r/catalingavan/logbee.frontend

Running Logbee in Docker will automatically install all the necessary prerequisites.

.. contents:: Table of contents
   :local:

Docker files
-------------------------------------------------------

To get started running logBee as a Docker container, create the following files:

.. code-block:: none

    /logbee_Docker
    ├── docker-compose.yml
    ├── backend.appsettings.json
    ├── backend.logbee.json
    ├── frontend.appsettings.json
    └── frontend.logbee.json

.. admonition:: Download Docker files
   :class: note

   Full working example of the files above can be found on `https://github.com/catalingavan/logbee-app <https://github.com/catalingavan/logbee-app/tree/main/Docker>`_.

.. code-block:: none
    :caption: docker-compose.yml

    version: "3.7"
    networks:
      default:
        name: logbee-net
        driver_opts:
          com.docker.network.driver.mtu: 1380
    
    services:
      backend:
        image: catalingavan/logbee.backend:1.2.1
        container_name: logbee.backend.dev
        restart: unless-stopped
        volumes:
          - ./backend.appsettings.json:/app/appsettings.json
          - ./backend.logbee.json:/app/Configuration/logbee.json
        ports:
          - "44088:80"
        links:
          - "mongodb"
    
      frontend:
        image: catalingavan/logbee.frontend:1.2.1
        container_name: logbee.frontend.dev
        restart: unless-stopped
        volumes:
          - ./frontend.appsettings.json:/app/appsettings.json
          - ./frontend.logbee.json:/app/Configuration/logbee.json
        ports:
          - "44080:80"
        links:
          - "backend"
    
      mongodb:
        image: mongo:6.0.4
        container_name: logbee.mongodb.dev
        restart: unless-stopped
        volumes:
          - mongo-data:/data/db
          - mongo-config:/data/configdb
    
    volumes:
      mongo-data:
      mongo-config:

Build
-------------------------------------------------------

To start the logBee server and all the necessary prerequisites, use ``docker-compose up`` command.

.. code-block:: none

    C:\logBee_Docker> docker-compose up

After all the services have been created, you can access the applications on the following urls:

- logbee.Frontend: http://localhost:44080/
- logbee.Backend: http://localhost:44088/

To authenticate, use the following token:

.. code-block:: none

   eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e30.HP79qro7bvfH7BneUy5jB9Owc_5D2UavFDulRETAl9E


.. figure:: images/docker-compose-up.png

.. figure:: images/logbee.Frontend-docker.png

.. figure:: images/logbee.Frontend-login.png

.. figure:: images/logbee.Backend-docker.png


Destroy
----------------------------

.. code-block:: none

    C:\logBee_Docker> docker-compose down


.. figure:: images/docker-compose-down.png
