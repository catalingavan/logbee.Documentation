Install Instructions
=====================

A full working example can be found `here <https://github.com/logBee-net/serilog-sinks-logbee/tree/main/samples/Serilog.Sinks.LogBee_WebApp>`_.

1. Install NuGet Package

.. code-block:: none
    :caption: Package Manager Console

    PM> Install-Package Serilog.Sinks.LogBee.AspNetCore
   

2. Update **appsettings.json**

.. code-block:: javascript
    :caption: appsettings.json

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

    using Serilog;
    using Serilog.Sinks.LogBee;
    using Serilog.Sinks.LogBee.AspNetCore;
    
    var builder = WebApplication.CreateBuilder(args);

    // register the HttpContextAccessor as it is used by the Serilog LogBee Sink
    builder.Services.AddHttpContextAccessor();
    builder.Services.AddControllersWithViews();

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

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");

    // register the LogBeeMiddleware() just before the app.Run()
    app.UseLogBeeMiddleware();

    app.Run();


4. Write logs:

.. code-block:: c#
    :caption: HomeController.cs

    public class HomeController : Controller
    {
        private readonly ILogger<HomeController> _logger;
        public HomeController(ILogger<HomeController> logger)
        {
            _logger = logger;
        }

        public IActionResult Index()
        {
            _logger.LogInformation("Hello world from {Controller}", "Home");
            return View();
        }
    }
