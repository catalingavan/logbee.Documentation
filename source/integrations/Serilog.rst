Serilog (.NET)
============================

Logbee integrates with .NET applications using Serilog by registering the ``Serilog.Sinks.Logbee`` sink.

.. container::

   .. image:: https://img.shields.io/nuget/v/Serilog.Sinks.LogBee.svg?style=flat-square&label=Serilog.Sinks.LogBee
      :target: https://www.nuget.org/packages?q=Serilog.Sinks.LogBee
      :alt: Serilog.Sinks.LogBee

   .. image:: https://img.shields.io/nuget/v/Serilog.Sinks.LogBee.AspNetCore.svg?style=flat-square&label=Serilog.Sinks.LogBee.AspNetCore
      :target: https://www.nuget.org/packages?q=Serilog.Sinks.LogBee.AspNetCore
      :alt: Serilog.Sinks.LogBee.AspNetCore


.. code-block:: c#
    :caption: Program.cs

    using Serilog;
    using Serilog.Sinks.LogBee;

    Log.Logger = new LoggerConfiguration()
        .WriteTo.LogBee(new LogBeeApiKey("_OrganizationId_", "_ApplicationId_", "https://api.logbee.net"))
        .CreateLogger();

    try
    {
        Log.Information("Hello from {Name}!", "Serilog");
    }
    catch(Exception ex)
    {
        Log.Error(ex, "Unhandled exception");
    }
    finally
    {
        Log.CloseAndFlush();
    }

Working examples can be found on the `Serilog integrations page <https://github.com/catalingavan/logbee-integrations-examples/tree/main/Serilog>`_.

