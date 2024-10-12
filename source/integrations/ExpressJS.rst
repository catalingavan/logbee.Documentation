ExpressJS (Node.js)
============================

| Node.js applications with ExpressJS can use the ``@logbee/express`` package to send the requests and logs to Logbee.
| `Source repository <https://github.com/catalingavan/logbee-express>`_

.. container::

   .. image:: https://img.shields.io/npm/v/@logbee/express.svg?style=flat-square&label=@logbee/express
      :target: https://www.npmjs.com/package/@logbee/express
      :alt: @logbee/express

.. code-block:: js
    :caption: index.js

    const express = require('express');
    const { logbee } = require('@logbee/express');

    const app = express();
    const port = 3000;

    app.use(express.json());
    app.use(express.urlencoded({ extended: true }));

    app.use(logbee.middleware({
        organizationId: '_OrganizationId_',
        applicationId: '_ApplicationId_',
        logbeeApiUri: 'https://api.logbee.net'
    }));

    app.get("/", (req, res) => {
        const logger = logbee.logger(req);

        logger?.info('Hello World! from ExpressJS');
        res.send('ok');
    });

    app.use(logbee.exceptionMiddleware());

    app.listen(port, () => {
        console.log(`[server]: Server is running at http://localhost:${port}`);
    });

Working example can be found on the `ExpressJS integration page <https://github.com/catalingavan/logbee-integrations-examples/tree/main/ExpressJS>`_.

