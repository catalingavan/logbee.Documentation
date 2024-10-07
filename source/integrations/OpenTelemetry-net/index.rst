OpenTelemetry (.NET)
============================

.NET applications using OpenTelemetry can send traces and logs to logbee.net.

.. contents:: Contents
   :local:

Console applications
----------------------------------------------

Step 1: Install the required NuGet packages

.. code-block:: none
    :caption: Package Manager Console

    PM> Install-Package OpenTelemetry -Version 1.9.0
    PM> Install-Package OpenTelemetry.Exporter.OpenTelemetryProtocol -Version 1.9.0

Step 2: Configure OpenTelemetry

Modify the **Program.cs** file to set up the OpenTelemetry services:

.. code-block:: c#
    :caption: Program.cs
    :linenos:
    :emphasize-lines: 16-17,21-26,33-37,39-45

    class Program
    {
        private static ActivitySource source = new ActivitySource("ConsoleApp.OpenTelemetry", "1.0.0");

        static void Main(string[] args)
        {
            using var loggerFactory = LoggerFactory.Create(builder =>
            {
                builder.AddOpenTelemetry((opt) =>
                {
                    opt.IncludeFormattedMessage = true;
                    opt.IncludeScopes = true;
                    opt.SetResourceBuilder(ResourceBuilder.CreateDefault()
                        .AddAttributes(
                        [
                            new("LogBee.OrganizationId", "_OrganizationId_"),
                            new("LogBee.ApplicationId", "_ApplicationId_")
                        ]));

                    opt
                        .AddOtlpExporter(opt =>
                        {
                            opt.Endpoint = new Uri("https://api.logbee.net/open-telemetry/v1/logs");
                            opt.Protocol = OpenTelemetry.Exporter.OtlpExportProtocol.HttpProtobuf;
                            opt.ExportProcessorType = ExportProcessorType.Batch;
                        });
                });
            });

            using var tracerProvider = Sdk.CreateTracerProviderBuilder()
                .ConfigureResource(resource =>
                    resource
                        .AddAttributes(
                        [
                            new("LogBee.OrganizationId", "_OrganizationId_"),
                            new("LogBee.ApplicationId", "_ApplicationId_")
                        ])
                )
                .AddSource(source.Name) // "ConsoleApp.OpenTelemetry"
                .AddOtlpExporter(opt =>
                {
                    opt.Endpoint = new Uri("https://api.logbee.net/open-telemetry/v1/trace");
                    opt.Protocol = OpenTelemetry.Exporter.OtlpExportProtocol.HttpProtobuf;
                    opt.ExportProcessorType = ExportProcessorType.Batch;
                })
                .Build();
        }
    }

Step 3: Write logs

.. code-block:: c#
    :caption: Program.cs
    :linenos:
    :emphasize-lines: 7

    class Program
    {
        static void Main(string[] args)
        {
            // code removed for simplicity
        
            Foo(loggerFactory);
        }

        private static void Foo(ILoggerFactory loggerFactory)
        {
            var logger = loggerFactory.CreateLogger<Program>();

            using (var parent = source.StartActivity("Foo", ActivityKind.Server))
            {
                parent?.AddTag("server.address", "localhost");
                parent?.AddTag("url.path", "/foo");

                logger.LogInformation("Executing Foo at {DateTime}", DateTime.UtcNow);
            }
        }
    }
   
ASP.NET Core Applications
----------------------------------------------

Step 1: Install the required NuGet packages

.. code-block:: none
    :caption: Package Manager Console

    PM> Install-Package OpenTelemetry.Extensions.Hosting -Version 1.9.0
    PM> Install-Package OpenTelemetry.Instrumentation.AspNetCore -Version 1.9.0
    PM> Install-Package OpenTelemetry.Exporter.OpenTelemetryProtocol -Version 1.9.0

Step 2: Configure OpenTelemetry

Modify the **Program.cs** file to set up the OpenTelemetry services:

.. code-block:: c#
    :caption: Program.cs
    :linenos:
    :emphasize-lines: 14-15,19-23,35-36,38,41

    using OpenTelemetry.Logs;
    using OpenTelemetry.Resources;
    using OpenTelemetry.Trace;

    var builder = WebApplication.CreateBuilder(args);

    builder.Services.AddOpenTelemetry()
        .WithTracing(tracingBuilder =>
        {
            tracingBuilder
                .SetResourceBuilder(ResourceBuilder.CreateDefault()
                    .AddAttributes(
                    [
                        new("LogBee.OrganizationId", "_OrganizationId_"),
                        new("LogBee.ApplicationId", "_ApplicationId_")
                    ])
                )
                .AddAspNetCoreInstrumentation()
                .AddOtlpExporter(opt =>
                {
                    opt.Endpoint = new Uri("https://api.logbee.net/open-telemetry/v1/trace");
                    opt.Protocol = OpenTelemetry.Exporter.OtlpExportProtocol.HttpProtobuf;
                    opt.ExportProcessorType = OpenTelemetry.ExportProcessorType.Batch;
                });
        });

    builder.Logging.AddOpenTelemetry(options =>
    {
        options.IncludeScopes = true;
        options.IncludeFormattedMessage = true;

        options
            .SetResourceBuilder(ResourceBuilder.CreateDefault()
                .AddAttributes(
                [
                    new("LogBee.OrganizationId", "_OrganizationId_"),
                    new("LogBee.ApplicationId", "_ApplicationId_")
                ]))
            .AddOtlpExporter(opt =>
            {
                opt.Endpoint = new Uri("https://api.logbee.net/open-telemetry/v1/logs");
                opt.Protocol = OpenTelemetry.Exporter.OtlpExportProtocol.HttpProtobuf;
                opt.ExportProcessorType = OpenTelemetry.ExportProcessorType.Batch;
            });
    });

    var app = builder.Build();

Step 3: Write logs

.. code-block:: c#
    :caption: Program.cs

    var builder = WebApplication.CreateBuilder(args);
    // code removed for simplicity

    var app = builder.Build();

    app.MapGet("/", (ILogger<Program> logger) =>
    {
        logger.LogInformation("My favourite cartoon is {Name}", "Futurama");
        return "Hello";
    });

    app.Run();
