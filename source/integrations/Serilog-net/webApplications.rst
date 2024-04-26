Web applications
====================

.NET web applications can use `Serilog <https://github.com/serilog/serilog>`_ to write events to logBee.net.

For web applications, `Serilog LogBee Sink <https://github.com/logBee-net/serilog-sinks-logbee>`_ stores all the events in the current http request (connection).

At the end of the request, the stored events are submitted to the logBee endpoint specified in the Sink configuration. 

.. contents:: Contents
   :local:

Install Instructions
----------------------------------------------

A full working example can be found `here <https://github.com/logBee-net/serilog-sinks-logbee/tree/main/samples/Serilog.Sinks.LogBee_WebApp>`_.

1. Install NuGet Package

.. code-block:: none
    :caption: Package Manager Console

    PM> Install-Package Serilog.Sinks.LogBee.AspNetCore
   

2. Update **appsettings.json**

.. code-block:: javascript
    :caption: appsettings.json
    :emphasize-lines: 9-11

    {
        "Logging": {
            "LogLevel": {
                "Default": "Trace",
                "Microsoft": "Information"
            }
        },

        "LogBee.OrganizationId": "_OrganizationId_",
        "LogBee.ApplicationId": "_ApplicationId_",
        "LogBee.ApiUrl": "https://api.logbee.net"
    }

3. Update **Program.cs**

.. code-block:: c#
    :caption: Program.cs
    :linenos:
    :emphasize-lines: 1-3,6,10-16,27

    using Serilog;
    using Serilog.Sinks.LogBee;
    using Serilog.Sinks.LogBee.AspNetCore;
    
    var builder = WebApplication.CreateBuilder(args);
    builder.Services.AddHttpContextAccessor();

    builder.Services.AddSerilog((services, lc) => lc
        .ReadFrom.Configuration(builder.Configuration)
        .WriteTo.LogBee(new LogBeeApiKey(
                builder.Configuration["LogBee.OrganizationId"]!,
                builder.Configuration["LogBee.ApplicationId"]!,
                builder.Configuration["LogBee.ApiUrl"]!
            ),
            services
        ));

    var app = builder.Build();

    app.MapGet("/", (ILogger<Program> logger) =>
    {
        logger.LogInformation("My favourite cartoon is {Name}", "Futurama");
        return "Hello";
    });

    // register the LogBeeMiddleware() just before the app.Run()
    app.UseLogBeeMiddleware();

    app.Run();


Configuration
----------------------------------------------

Additional Serilog.Sink.LogBee configuration can be provided using the ``config`` parameter.

.. code-block:: c#
    :caption: Program.cs
    :linenos:

    builder.Services.AddSerilog((services, lc) => lc
        .ReadFrom.Configuration(builder.Configuration)
        .WriteTo.LogBee(new LogBeeApiKey(
                builder.Configuration["LogBee.OrganizationId"]!,
                builder.Configuration["LogBee.ApplicationId"]!,
                builder.Configuration["LogBee.ApiUrl"]!
            ),
            services,
            config =>
            {
                config.ShouldReadRequestHeader = (request, header) =>
                {
                    if (string.Equals(header.Key, "X-Api-Key", StringComparison.OrdinalIgnoreCase))
                        return false;

                    return true;
                };

                config.ShouldLogRequest = (context) =>
                {
                    // we don't want to log successful health check requests
                    if(string.Equals(context.Request.Path, "/status/healthcheck", StringComparison.OrdinalIgnoreCase)
                        && context.Response.StatusCode == 200)
                    {
                        return false;
                    }

                    return true;
                };
            }
        ));

Logging files
----------------------------------------------

With ``Serilog.Sinks.LogBee`` you can log string content as files.

In order to do so, you need to access the LoggerContext by using the ``HttpContext.GetLogBeeLoggerContext()`` extension method.

.. code-block:: c#

    using Microsoft.AspNetCore.Mvc;
    using Serilog.Sinks.LogBee.AspNetCore;
    using System.Text.Json;

    namespace Serilog.Sinks.LogBee_WebApp.Controllers
    {
        public class HomeController : Controller
        {
            public IActionResult Index()
            {
                var loggerContext = HttpContext.GetLogBeeLoggerContext();
                loggerContext?.LogAsFile(JsonSerializer.Serialize(new { Prop = "Value" }), "File.json");

                return View();
            }
        }
    }
