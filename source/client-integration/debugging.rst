Debugging
************

.NET Core
==========

Enabling logging
_________________

The client library has the ability to log useful information that can be used for debug purposes. To enable logging, supply the :code:`Direct` or :code:`Portal` client with an implementation of :code:`Microsoft.Extensions.Logging.ILoggerFactory`. This is Microsoft's own logging API, and allows the user to chose their own logging framework.

Enabling logging on level :code:`DEBUG` will output positive results of requests and worse, :code:`WARN` only failed requests or worse, while :code:`ERROR` will only occur on failed requests to create a signature job. These loggers will be under the :code:`Digipost.Signature.Api.Client` namespace.

Implementing using NLog
^^^^^^^^^^^^^^^^^^^^^^^^

There are numerous ways to implement a logger, but the following examples will be based on `NLog documentation <https://github.com/NLog/NLog.Extensions.Logging/wiki/Getting-started-with-.NET-Core-2---Console-application>`_.

#. Install the Nuget-packages :code:`NLog`, :code:`NLog.Extensions.Logging` and :code:`Microsoft.Extensions.DependencyInjection`.
#. Create a *nlog.config* file. The following is an example that logs to file and to console:

..  code-block:: xml

    <?xml version="1.0" encoding="utf-8"?>

    <!-- XSD manual extracted from package NLog.Schema: https://www.nuget.org/packages/NLog.Schema-->
    <nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          autoReload="true"
          internalLogFile="c:\temp\console-example-internal.log"
          internalLogLevel="Info">
        <!-- the targets to write to -->
        <targets>
            <!-- write logs to file -->
            <target xsi:type="File"
                    name="fileTarget"
                    fileName="/logs/signature-api-client-dotnet/signature-api-client-dotnet.log"
                    layout="${date}|${level:uppercase=true}|${message} ${exception}|${logger}|${all-event-properties}"
                    archiveEvery="Day"
                    archiveNumbering="Date"
                    archiveDateFormat="yyyy-MM-dd"/>
            <!-- write logs to console -->
            <target xsi:type="Console"
                    name="consoleTarget"
                    layout="${date}|${level:uppercase=true}|${message} ${exception}|${logger}|${all-event-properties}" />
        </targets>

        <!-- rules to map from logger name to target -->
        <rules>
            <logger name="*" minlevel="Trace" writeTo="fileTarget,consoleTarget"/>
        </rules>
    </nlog>


3. In your application, do the following to create a logger and supply it to the client:

..  code-block:: c#

    private static IServiceProvider CreateServiceProviderAndSetUpLogging()
    {
        var services = new ServiceCollection();

        services.AddSingleton<ILoggerFactory, LoggerFactory>();
        services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
        services.AddLogging((builder) => builder.SetMinimumLevel(LogLevel.Trace));

        var serviceProvider = services.BuildServiceProvider();
        SetUpLoggingForTesting(serviceProvider);

        return serviceProvider;
    }

    private static void SetUpLoggingForTesting(IServiceProvider serviceProvider)
    {
        var loggerFactory = serviceProvider.GetRequiredService<ILoggerFactory>();

        loggerFactory.AddNLog(new NLogProviderOptions {CaptureMessageTemplates = true, CaptureMessageProperties = true});
        NLog.LogManager.LoadConfiguration("./nlog.config");
    }

    static void Main(string[] args)
    {
        ClientConfiguration clientConfiguration = null;
        var serviceProvider = CreateServiceProviderAndSetUpLogging();
        var client = new PortalClient(clientConfiguration, serviceProvider.GetService<ILoggerFactory>());
    }


Request and response logging
_____________________________

For initial integration and debugging purposes, it can be useful to log the actual request and response going over the wire. This can be enabled by doing the following:

Set the property :code:`ClientConfiguration.LogRequestAndResponse = true`.

..  WARNING::
    Enabling request logging should never be used in a production system. It will severely impact the performance of the client.

Logging of document bundle
____________________________

Logging of document bundle can be enabled via the :code:`ClientConfiguration`:

..  code-block:: c#

    var clientConfiguration = new ClientConfiguration(Environment.DifiTest, "3k 7f 30 dd 05 d3 b7 fc...");
    clientConfiguration.EnableDocumentBundleDiskDump("/directory/path/for/bundle/disk/dump");

..  NOTE::
    Remember to only set the directory to save the disk dump. A new zip file will be placed there for each created signature job.

If you have special needs for the bundle other than just saving it to disk, add your own bundle processor to :code:`ClientConfiguration.DocumentBundleProcessors`.


Java
======

Request and response logging
_____________________________


..  WARNING::
    Enabling request logging should never be used in a production system. It will impact the performance of the client.

You may configure the client library to log HTTP requests and responses by calling :code:`.enableRequestAndResponseLogging()` when creating the client's configuration. You may configure the logger :code:`no.digipost.signature.client.http.requestresponse` in order to customize logging. It must be set to at least :code:`INFO` to write anything to the log.

Writing document bundle to disk
________________________________

You may configure the client library to write a ZIP file with the document bundle by calling :code:`.enableDocumentBundleDiskDump(Path)` when creating the client's configuration.

The `Path parameter <https://docs.oracle.com/javase/7/docs/api/java/nio/file/Path.html>`_ is the directory to where the files will be written. This directory *must* exists as the client library won't try creating it.

If you have needs for the document bundle other than just saving it to disk, add your own document bundle processor by calling :code:`.addDocumentBundleProcessor(…)` with your own :code:`DocumentBundleProcessor` when creating the client's configuration.
