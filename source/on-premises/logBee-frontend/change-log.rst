Change log
===============

.. contents:: Versions
   :local:
   :depth: 1

logBee.Frontend 1.1.6
--------------------------

Release date: 04-09-2024

https://github.com/catalingavan/logBee-app/releases/tag/logBee.Frontend-v1.1.6

Implemented "Create an alert for this exception" functionality.

.. figure:: images/change-log/create-alert-for-exception-button.png
    :alt: "Create an alert for this exception" button

logBee.Frontend 1.1.2
--------------------------

Release date: 09-07-2024

https://github.com/catalingavan/logBee-app/releases/tag/logBee.Frontend-v1.1.2

Implemented date-time plugin selector for table filters.

.. figure:: images/change-log/table-date-time-picker.png
    :alt: date-time plugin selector

logBee.Frontend 1.1.0
--------------------------

Release date: 06-06-2024

https://github.com/catalingavan/logBee-app/releases/tag/logBee.Frontend-v1.1.0

Upgraded the application to .NET 8.0

logBee.Frontend 1.0.1
--------------------------

Release date: 15-03-2024

https://github.com/catalingavan/logBee-app/releases/tag/logBee.Frontend-v1.0.1


Migrating from the legacy KissLog.Frontend
---------------------------------------------

If you are currently using the legacy KissLog.Frontend application, upgrading to the new logBee.Frontend application will bring breaking changes to both configuration file and to the database schema.

For the configuration file (KissLog.json -> logBee.json) it is best to use the new configuration file and update the properties with your settings.

logBee.Frontend is now using a new database schema incompatible with the previous KissLog.Frontend application.

To migrate the existing data, please follow these steps:

1. In the logBee.Frontend application, update your database connection string to a new database name, different than the database name used in KissLog.Frontend.

2. Run the logBee.Frontend application for the first time, which will create the empty database (with the new schema).

3. Copy the existing data from the old database to the new database, using the following script (MySql language):

.. code-block:: sql

    INSERT INTO `<NEW_DATABASE>`.`application`
    (`Id`,`Organization_Id`,`Name`,`CreatedAt`)
    SELECT `Id`, `Organization_Id`, `Name`, `DateTimeCreated`
    FROM `<OLD_DATABASE>`.`application`
    WHERE `Id` NOT IN (SELECT `Id` from `NEW_DATABASE`.`application`)


    INSERT INTO `<NEW_DATABASE>`.`organization`
    (`Id`,`Name`,`CreatedAt`)
    SELECT `Id`, `Name`, `DateTimeCreated`
    FROM `<OLD_DATABASE>`.`organization`
    WHERE `Id` NOT IN (SELECT `Id` from `<NEW_DATABASE>`.`organization`)


    INSERT INTO `<NEW_DATABASE>`.`organizationalert`
    (`Id`,
    `Organization_Id`,
    `Name`,
    `Description`,
    `JavascriptCode`,
    `ThrottleInSeconds`,
    `CreatedAt`,
    `IsEnabled`,
    `AppliesToAllApplications`,
    `EmailNotificationJson`,
    `SlackNotificationJson`,
    `MicrosoftTeamsNotificationJson`)
    SELECT
    `Id`,
    `Organization_Id`,
    `Name`,
    `Description`,
    `JavascriptCode`,
    `ThrottleInSeconds`,
    `DateTimeCreated`,
    `IsActive`,
    1,
    REPLACE(REPLACE(`EmailNotification`, '"IsActive"', '"IsEnabled"'), '"Emails"', '"SendTo"'),
    REPLACE(`SlackNotification`, '"IsActive"', '"IsEnabled"'),
    REPLACE(`MicrosoftTeamsNotification`, '"IsActive"', '"IsEnabled"')
    FROM `<OLD_DATABASE>`.`alert`
    WHERE `Id` NOT IN (SELECT `Id` from `<NEW_DATABASE>`.`organizationalert`)


If you need assistance, please feel free to reach out and I will happily help with the migration.
