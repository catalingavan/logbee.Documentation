Web applications
====================

Web applications using `Serilog <https://github.com/serilog/serilog>`_ can integrate the Serilog.Sinks.LogBee.AspNetCore Sink to write events to logBee.net.

.. code-block:: c#
    :caption: Program.cs

    var builder = WebApplication.CreateBuilder(args);

    builder.Services.AddSerilog((services, lc) => lc
        .WriteTo.LogBee(
            new LogBeeApiKey("_OrganizationId_", "_ApplicationId_", "https://api.logbee.net"),
            services
        ));

    // make sure you register the LogBeeMiddleware just before the app.Run()
    app.UseLogBeeMiddleware();

    app.Run();

.. contents:: Contents
   :local:

Saving the logs
----------------------------------------------

The ``Serilog.Sinks.LogBee.AspNetCore`` Sink stores all the events in the current http request (connection).

At the end of the request, the stored events are sent automatically to the logBee endpoint specified in the Sink configuration. 

In addition to the log events, the LogBee Serilog Sink also collects all the HTTP properties of the current execution request.

.. note::
   Make sure you register the LogBee middleware by calling ``app.UseLogBeeMiddleware()`` before the ``app.Run()``


Configuration
----------------------------------------------

Additional LogBee Serilog Sink configuration can be provided using the ``config`` parameter.

.. code-block:: c#
    :caption: Program.cs

    var builder = WebApplication.CreateBuilder(args);

    builder.Services.AddSerilog((services, lc) => lc
        .Enrich.WithCorrelationId()
        .WriteTo.LogBee(
            new LogBeeApiKey("_OrganizationId_", "_ApplicationId_", "https://api.logbee.net"),
            services,
            config =>
            {
                config.ShouldReadRequestHeader = (request, header) =>
                {
                    // we don't want to log sensitive Header value
                    if (string.Equals(header.Key, "X-Api-Key", StringComparison.OrdinalIgnoreCase))
                        return false;

                    return true;
                };

                // handler used to determine if a Request should be saved to logBee endpoint or not
                config.ShouldLogRequest = (context) =>
                {
                    if (string.Equals(context.Request.Path, "/status/healthcheck", StringComparison.OrdinalIgnoreCase)
                            && context.Response.StatusCode < 400)
                    {
                        return false;
                    }

                    return true;
                };

                config.AppendExceptionDetails = (ex) =>
                {
                    if (ex is NullReferenceException nullRefEx)
                        return "Don't forget to check for null references";

                    return null;
                };

                config.RequestKeywords = (context) =>
                {
                    var keywords = new List<string>();
                    if (context.Items.TryGetValue("CorrelationIdEnricher+CorrelationId", out var value)
                            && value is string correlationId)
                    {
                        // add the Serilog CorrelationId as a search keyword
                        keywords.Add(correlationId);
                    }

                    return keywords;
                };
            }
        ));


Logging files
----------------------------------------------

With ``Serilog.Sinks.LogBee.AspNetCore`` you can log string content as files.

In order to do so, you need to access the LoggerContext by using the ``HttpContext.GetLogBeeLoggerContext()`` extension method.

.. code-block:: c#
    :caption: HomeController.cs

    public class HomeController : Controller
    {
        public IActionResult Index()
        {
            var loggerContext = HttpContext.GetLogBeeLoggerContext();
            loggerContext?.LogAsFile(JsonSerializer.Serialize(new
            {
                eventCode = "AUTHORISATION",
                amount = new
                {
                    currency = "USD",
                    value = 12
                }
            }), "Event.json");

            return View();
        }
    }

.. image:: images/request_files_tab.png
    :alt: Request Files tab

.. image:: images/request_file_preview.png
    :alt: Request File preview


:doc:`Install instructions </integrations/Serilog-net/web-apps/install-instructions>`
---------------------------------------------------------------------------------------------------------

.. toctree::
   :hidden:

   install-instructions

