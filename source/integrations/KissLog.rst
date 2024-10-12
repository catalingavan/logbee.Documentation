KissLog (.NET)
============================

.NET applications can use the ``KissLog`` NuGet package to save the logs to Logbee.

.. container::

   .. image:: https://img.shields.io/nuget/v/KissLog.svg?style=flat-square&label=KissLog
      :target: https://www.nuget.org/packages?q=KissLog
      :alt: KissLog

   .. image:: https://img.shields.io/nuget/v/KissLog.AspNetCore.svg?style=flat-square&label=KissLog.AspNetCore
      :target: https://www.nuget.org/packages?q=KissLog.AspNetCore
      :alt: KissLog.AspNetCore

   .. image:: https://img.shields.io/nuget/v/KissLog.AspNet.Mvc.svg?style=flat-square&label=KissLog.AspNet.Mvc
      :target: https://www.nuget.org/packages?q=KissLog.AspNet.Mvc
      :alt: KissLog.AspNet.Mvc

   .. image:: https://img.shields.io/nuget/v/KissLog.AspNet.WebApi.svg?style=flat-square&label=KissLog.AspNet.WebApi
      :target: https://www.nuget.org/packages?q=KissLog.AspNet.WebApi
      :alt: KissLog.AspNet.WebApi


Working examples and different use cases can be found on the `KissLog integrations page <https://github.com/catalingavan/logbee-integrations-examples/tree/main/KissLog>`_.

.. code-block:: c#
    :caption: Program.cs

    using KissLog.AspNetCore;
    using KissLog.CloudListeners.Auth;
    using KissLog.CloudListeners.RequestLogsListener;
    using KissLog.Formatters;

    var builder = WebApplication.CreateBuilder(args);
    builder.Services.AddLogging(provider =>
    {
        provider
            .AddKissLog(options =>
            {
                options.Formatter = (FormatterArgs args) =>
                {
                    if (args.Exception == null)
                        return args.DefaultValue;

                    string exceptionStr = new ExceptionFormatter().Format(args.Exception, args.Logger);
                    return string.Join(Environment.NewLine, new[] { args.DefaultValue, exceptionStr });
                };
            });
    });

    var app = builder.Build();

    app.MapGet("/", (ILogger<Program> logger) =>
    {
        logger.LogInformation("Hello from minimal API");
        return "Hello World!";
    });

    app.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");

    app.UseKissLogMiddleware(options => {
        options.Listeners.Add(new RequestLogsApiListener(new Application("_OrganizationId_", "_ApplicationId_"))
        {
            ApiUrl = "https://api.logbee.net/"
        });
    });

    app.Run();

