Install Instructions
=====================

1. Install the `@logbee/express <https://www.npmjs.com/package/@logbee/express>`_ package

.. code-block:: none

    npm install @logbee/express
   

2. Register the logbee middleware

.. code-block:: javascript
    :caption: index.cs
    :linenos:
    :emphasize-lines: 2,11-15,27

    const express = require('express');
    const { logbee } = require('@logbee/express');
    
    const app = express();
    
    // these middlewares will populate the "request.body" property
    // if present "request.body" will be logged by logbee
    app.use(express.json());
    app.use(express.urlencoded({ extended: true }));
    
    app.use(logbee.middleware({
        organizationId: '_LogBee.OrganizationId_',
        applicationId: '_LogBee.ApplicationId_',
        logbeeApiUri: 'https://api.logbee.net/'
    }));
    
    app.get("/", (req, res) => {
        // code
    });
    
    app.post("/submit", (req, res) => {
        // code
    });

    // other routes declaration

    app.use(logbee.exceptionMiddleware());
    
    app.listen(3000, () => {
        console.log('Server is running');
    });


3. Access the logger

logbee creates a ``logger`` object that can be accessed using the ``logbee.logger(req)`` function.

Log messages can be created using one of the following logger methods: ``trace``, ``debug``, ``info``, ``warn``, ``error``, ``critical``

.. code-block:: javascript
    :caption: index.cs
    :linenos:
    :emphasize-lines: 2-3

    app.get("/", (req, res) => {
        const logger = logbee.logger(req);
        logger?.info('An info message', 'with', 'multiple', 'args', { 'foo': 'bar' });
    
        res.send('Hello World!');
    });