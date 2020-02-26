# A full overview of Azure Application Insights

# Intro
According to [Microsoft](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview#how-do-i-use-application-insights):

  Application Insights, a feature of Azure Monitor, is an extensible Application Performance Management (APM) service for developers and DevOps professionals. Use it to monitor your live applications. It will automatically detect performance anomalies, and includes powerful analytics tools to help you diagnose issues and to understand what users actually do with your app. It's designed to help you continuously improve performance and usability. It works for apps on a wide variety of platforms including .NET, Node.js and Java EE, hosted on-premises, hybrid, or any public cloud. It integrates with your DevOps process, and has connection points to a variety of development tools. It can monitor and analyze telemetry from mobile apps by integrating with Visual Studio App Center.

# AppInsights Services
These are some services that AppInsights provide:
    * HTTP request rates, response times, and success rates
    * Dependency (HTTP & SQL) call rates, response times, and success rates
    * Exception traces from both server and client
    * Diagnostic log traces
    * Page view counts, user and session counts, browser load times, and exceptions
    * AJAX call rates, response times, and success rates
    * Server performance counters
    * Custom client and server telemetry
    * Segmentation by client location, browser version, OS version, server instance, custom dimensions, and more
    * Availability tests

Along with the preceding types, there are associated diagnostic and analytics tools available for alerting and monitoring with various different customizable metrics. With its own query language and customizable dashboards, Application Insights forms a good monitoring solution for .NET microservices.
A brief overview of the ELK stack

# Getting Started
If developing for a .Net project that is supported by one of our platform specific packages, Web or Windows Apps, we strongly recommend to use one of those packages instead of this base library. If your project does not fall into one of those platforms you can use this library for any .Net code. This library should have no dependencies outside of the .Net framework. If you are building a Desktop or any other .Net project type this library will enable you to utilize Application Insights. More on SDK layering and extensibility later.
Get an Instrumentation Key

To use the Application Insights SDK you will need to provide it with an Instrumentation Key which can be obtained from the portal. This Instrumentation Key will identify all the data flowing from your application instances as belonging to your account and specific application.


## Add the SDK library

We recommend consuming the library as a NuGet package. Make sure to look for the Microsoft.ApplicationInsights package. Use the NuGet package manager to add a reference to your application code.

## Initialize a TelemetryClient

The TelemetryClient object is the primary root object for the library. Almost all functionality around telemetry sending is located on this object. You must initialize an instance of this object and populate it with your Instrumentation Key to identify your data.
```cs
using Microsoft.ApplicationInsights;

var tc = new TelemetryClient();
tc.InstrumentationKey = "INSERT YOUR KEY";
```

## Use the TelemetryClient to send telemetry

This "base" library does not provide any automatic telemetry collection or any automatic meta-data properties. You can populate common context on the TelemetryClient.context property which will be automatically attached to each telemetry item sent. You can also attach additional property data to each telemetry item sent. The TelemetryClient also exposes a number of Track...() methods that can be used to send all telemetry types understood by the Application Insights service. Some example use cases are shown below.

```cs
tc.Context.User.Id = Environment.GetUserName(); // This is probably a bad idea from a PII perspective.
tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

tc.TrackPageView("Form1");

tc.TrackEvent("PurchaseOrderSubmitted", new Dictionary<string, string>() { {"CouponCode", "JULY2015" } }, new Dictionary<string, double>() { {"OrderTotal", 68.99 }, {"ItemsOrdered", 5} });
	
try
{
	...
}
catch(Exception e)
{
	tc.TrackException(e);
}

```

## Adding Application Insights to your soluion




## Configuring Application Insights

As soon as you add the NuGet dependencies to your project, NuGet will create a default configuration for you. It's available under <project>\ApplicationInsights.config. By default this is where AppInsights will get its configuratio from. We'll see below how to make some changes.

For more information, check the documentation at: [Configuring the Application Insights SDK with ApplicationInsights.config or .xml](https://docs.microsoft.com/en-us/azure/azure-monitor/app/configuration-with-applicationinsights-config).

For example, this is my default AppInsights.config just after I installed the packages.
```xml
<?xml version="1.0" encoding="utf-8"?>  
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">  
  <InstrumentationKey>{InstrumentKey}</InstrumentationKey>
  <TelemetryInitializers>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer, Microsoft.AI.DependencyCollector"/>
  </TelemetryInitializers>
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.AI.DependencyCollector">
      <ExcludeComponentCorrelationHttpHeadersOnDomains>
        <!-- 
        Requests to the following hostnames will not be modified by adding correlation headers.         
        Add entries here to exclude additional hostnames.
        NOTE: this configuration will be lost upon NuGet upgrade.
        -->
        <Add>core.windows.net</Add>
        <Add>core.chinacloudapi.cn</Add>
        <Add>core.cloudapi.de</Add>
        <Add>core.usgovcloudapi.net</Add>
      </ExcludeComponentCorrelationHttpHeadersOnDomains>
      <IncludeDiagnosticSourceActivities>
        <Add>Microsoft.Azure.EventHubs</Add>
        <Add>Microsoft.Azure.ServiceBus</Add>
      </IncludeDiagnosticSourceActivities>
    </Add>
  </TelemetryModules>
  <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights"/>
</ApplicationInsights>
```

# Configuring from Code
Yes, the above xml is nice but we developers want to t

```cs
ConfigurationItemFactory.Default.Targets.RegisterDefinition("ai", typeof(ApplicationInsightsTarget));
var aiTarget = new ApplicationInsightsTarget
{
    InstrumentationKey = ConfigurationManager.AppSettings["Atis.Services.InstrumentKey"],
    Name = "ai"
};

config.AddTarget("ai", aiTarget);
```

# Validating Telemetry
Once you have everything setup, it's time to run your app and validate that the telemetry is being uploaded to Azure. On my case, this is what I got:

<img>

But there's a lot of internal trace messages there. How do I clean them. 


# Where's my data
If you didn't see your data, it's because the Telemetry is not sent instantly. Telemetry items are batched and sent by the ApplicationInsights SDK. In Console apps, which exits right after calling Track() methods, telemetry may not be sent unless Flush() and Sleep is done before the app exits as shown in full example later in this article.

Check [this article](https://docs.microsoft.com/en-us/azure/azure-monitor/app/console) for more information.

# Fine-tuning the telemetry
```cs
NLog.LogManager.Configuration = BuildConfiguration();
var config = new LoggingConfiguration();
var logLevel = NLog.LogLevel.FromString(ConfigurationManager.AppSettings["MyApp.LogLevel"]);
config.LoggingRules.Add(new LoggingRule("*", logLevel, aiTarget));
config.AddTarget("ai", aiTarget);
```

# Setting LogLevel
Regulating ow much we send to AppInsights is simple. Since AppInsights honours your LoggingRules, it's just a matter of getting what's set from your config file.

For example. In my backend composition root, I have:
```cs
config.LoggingRules.Add(new LoggingRule("*", logLevel, aiTarget));
```

# Logging Global Application Errors
Another good practice is logging global application errros. In ASP.NET, that's done on the Application_Error method. Here's an example:

```cs

    protected void Application_Error(object sender, EventArgs e)
    {
        var ex = Server.GetLastError();

        if (ex is HttpAntiForgeryException)
        {
            Response.Clear();
            Session.Abandon();
            FormsAuthentication.SignOut();

            Response.Redirect("/Account/Login", true);
        }
        else
        {
            var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
            telemetry.TrackException(ex, new Dictionary<string, string> {
                { "AppName", "ATIS.Web" },
                { "ErrorType", "" }
            });
        }
    }

```


# Tracking Custom Events
Another thing that can be done is tracking custom events. For example, on my backend I have custom AppInsights to not only track exceptions from SendGrid but also track the parameters used. That way we can easily reproduce the scenario and fix potential bugs as soon as possible.

This is how to do it:
```cs
// init appInishgts TelemetryClient on the constructor 
appInsights= new TelemetryClient(new TelemetryConfiguration(config.GetSetting("MyApp.InstrumentKey")));

// t
```

# Alerts
Another nice things is to configure alerts hooked to your application insights telemetry. While this is out of scope for this post, I urge you to take a look at the feature. It's very nice indeed.


# Deleting your telemetry
It may happen that after you got your data on AppInsights you realize that there's a lot there that you'd like to delete. How do we do it?

Actually we don't. According to Microsoft's [Data collection, retention, and storage in Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/data-retention-privacy), the data cannot be deleted. And it makes sense. Think of your telemetry as Event Sourcing. It's also better to have a lot than none.

A simple workaround would be to create another instance and use that other instance (remember, you'll have to change your instrumentation key).


# Supressing uneeded traces
The last tip is how to suppress stuff you don't need. For example, we had a lot of 409s emmitted by NServiceBus that we couldn't care less.

To fix that we need to do two things:
1. Create a Filter : ITelemetry
2. Inject it into our initialization logic (global.asax for webapps, your startup method for your backend)








# References
[Microsoft.ApplicationInsights.NLogTarget](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)