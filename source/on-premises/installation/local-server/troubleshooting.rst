Troubleshooting
=============================

.. contents:: Table of contents
   :local:

Some of the common reasons for which the logBee server can fail to run include:

- ``Configuration\logbee.json`` configuration errors
- Services connectivity errors (such as SQL or MongoDB)
- SQL permissions errors

Application logs
-------------------------------------------------------

Both IIS applications save their internal logs under the \\logs folder. Here should be the first place to check.

* C:\\inetpub\\wwwroot\\logbee.Backend\\logs
* C:\\inetpub\\wwwroot\\logbee.Frontend\\logs

Need help?
-------------------------------------------------------

Open a `GitHub issue <https://github.com/catalingavan/logbee-app/issues>`_ or send an email to catalingavan@gmail.com.