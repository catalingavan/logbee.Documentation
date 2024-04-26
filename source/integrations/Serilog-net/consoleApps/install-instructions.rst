Install Instructions
=====================
]
A full working example can be found `here <https://github.com/logBee-net/serilog-sinks-logbee/tree/main/samples/Serilog.Sinks.LogBee_ConsoleApp>`_.

1. Install NuGet Package

.. code-block:: none
    :caption: Package Manager Console

    PM> Install-Package Serilog.Sinks.LogBee
   

2. Update **Program.cs**

.. code-block:: c#
    :caption: Program.cs

    using Serilog.Sinks.LogBee;

    namespace Serilog.Sinks.LogBee_ConsoleApp;

    class Program
    {
        static void Main(string[] args)
        {
            Log.Logger =
                new LoggerConfiguration()
                    .WriteTo.LogBee(
                        new LogBeeApiKey(
                            "_OrganizationId_",
                            "_ApplicationId_",
                            "https://api.logbee.net"
                        )
                    )
                    .CreateLogger();

            try
            {
                string name = "Serilog";
                Log.Information("Hello, {Name}!", name);

                throw new NullReferenceException("Oops...");
            }
            catch (Exception ex)
            {
                Log.Error(ex, "Unhandled exception");
            }
            finally
            {
                // make sure you flush the logger so the events are sent to the logBee endpoint
                Log.CloseAndFlush();
            }
        }
    }
