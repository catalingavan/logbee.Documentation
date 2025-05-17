Logbee.Frontend
=================================

.. contents:: Table of contents
   :local:

About
------------------------------

Logbee.Frontend is a user-interface application that helps developers visualize captured errors, logs, and other metric data.
It dynamically generates the UI by consuming REST endpoints from :doc:`Logbee.Backend </on-premises/logbee-backend/index>`.

Authentication
------------------------------

Logbee.Frontend uses a JSON Web Token (JWT) for authentication.
The JWT must be signed using the secret key specified in the :ref:`$.Authorization.HS256Secret <on-premises/logbee-frontend/configuration:Authorization>` property inside the logbee.json configuration file.

If using the default ``HS256Secret`` value, you can use the following authentication JWT:

.. code-block:: none
    
    # $.Authorization.HS256Secret:
    # 00000000-0000-0000-0000-000000000000-00000000-0000-0000-0000-000000000000
    
    # Authentication token:
    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e30.HP79qro7bvfH7BneUy5jB9Owc_5D2UavFDulRETAl9E

Creating the JWT
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can create an authentication JWT programmatically or using an online tool such as `jwt.io <https://jwt.io/>`_.

To generate a JWT manually:

1. Open `jwt.io <https://jwt.io/>`_ .

2. Select the ``HS256`` algorithm.

3. (Optional) Set the ``name`` claim  (e.g., "name": "user_name").

4. Use the ``HS256Secret`` from your logbee.json configuration file as the secret key.

5. Copy the generated JWT and use it as Token in the authentication page.

.. figure:: images/generating-authentication-jwt.png
    :alt: Generating authentication JWT

.. figure:: images/logbee.Frontend-login.png
    :alt: logbee.Frontend login page


User name
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The authenticated user name is used for display purposes only. It can be specified using one of the following methods:

- Appending it to the JWT as ``@user_name``:

  .. code-block:: none
    
    # Authentication token:
    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e30.HP79qro7bvfH7BneUy5jB9Owc_5D2UavFDulRETAl9E@user_name

- Including it in the JWT payload using one of the following claims:

  "emailAddress", "email", "preferred_username", "name"


Auto-login
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can automate the login process by passing the authentication token directly in the login URL using the ``token=value`` query string parameter.

.. code-block:: none
    
    # Authentication url:
    http://localhost:44080/Auth/Login?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e30.HP79qro7bvfH7BneUy5jB9Owc_5D2UavFDulRETAl9E

You can bookmark this URL to quickly log in without manually entering credentials each time.

.. toctree::
   :hidden: 
   :maxdepth: 2
   :titlesonly:
   :includehidden:

   configuration
   active-directory-auth/index
   change-log