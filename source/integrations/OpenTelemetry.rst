Open Telemetry
============================

Logbee supports OpenTelemetry (OTEL) integration for both **Traces** and **Logs**.

Traces are stored as individual requests, identified by ``span_id``. Logs sharing the same ``span_id`` are linked to the corresponding requests.

Uncorrelated logs are saved separately and grouped in 5-minute intervals.

Working examples can be found on the `OpenTelemetry integrations page <https://github.com/catalingavan/logbee-integrations-examples/tree/main/OpenTelemetry>`_.

Endpoints
-------------------------------

- ``https://api.logbee.net/open-telemetry/v1/traces``
- ``https://api.logbee.net/open-telemetry/v1/logs``

.Open Telemetry Collector
-------------------------------

.. code-block:: yaml
    :caption: otel-collector-config.yaml

    receivers:
      otlp:
        protocols:
          grpc:
          http:

    processors:
      batch:
        timeout: 5s
        send_batch_size: 100

    exporters:
      otlphttp/trace_exporter:
        endpoint: "https://api.logbee.net/open-telemetry"
        compression: none

      otlphttp/log_exporter:
        endpoint: "https://api.logbee.net/open-telemetry"
        compression: none
        
      debug:
        verbosity: detailed

    service:
      pipelines:
        traces:
          receivers: [otlp]
          processors: [batch]
          exporters: [otlphttp/trace_exporter, debug]

        logs:
          receivers: [otlp]
          processors: [batch]
          exporters: [otlphttp/log_exporter, debug]


.NET
-------------------------------

.NET applications can use the ``OpenTelemetry.Exporter.OpenTelemetryProtocol`` exporter to send the logs and traces to Logbee.

.. code-block:: c#
    :caption: Program.cs

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
        .ConfigureResource(res =>
            res.AddAttributes(
            [
                new("LogBee.OrganizationId", "_OrganizationId_"),
                new("LogBee.ApplicationId", "_ApplicationId_")
            ])
        )
        .AddSource("App")
        .AddOtlpExporter(opt =>
        {
            opt.Endpoint = new Uri("https://api.logbee.net/open-telemetry/v1/traces");
            opt.Protocol = OpenTelemetry.Exporter.OtlpExportProtocol.HttpProtobuf;
            opt.ExportProcessorType = ExportProcessorType.Batch;
        })
        .Build();


