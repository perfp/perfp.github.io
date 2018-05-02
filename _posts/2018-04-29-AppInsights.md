---
layout: default
title: Using Application Insights
category: Azure
---
# Using Application Insights

To use Applicaiton Insights from an asp.net core project, you need to register Application Insights on startup: 
```csharp

public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseApplicationInsights()
                .UseStartup<Startup>()
                .Build();
    }
```

If you're running locally this will start emitting AppInsights events as debug messages in Visual Studio or Visual Studio Code.

To publish events to Application Insights need to register the Applicaton Insights instance you want to use. This can be done using wizards in Visual Studio, but what happens behind the scenes is that it will add a nuget package to your project, and set two settings in the .csproj file. This can be done manually like this.
```
dotnet add package Microsoft.ApplicationInsights.AspNetCore
```

In .csproj:
```
  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
    
    <ApplicationInsightsResourceId>/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/components/<AppInsightsName></ApplicationInsightsResourceId>
    
    <ApplicationInsightsAnnotationResourceId>/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/components/<AppInsightsName></ApplicationInsightsAnnotationResourceId>    

  </PropertyGroup>
```


If you deploy the application to an Azure App Service that has an associated Application Insights, there will be environment variables with the Instrumentation Key predefined for  ApplicationInsight:

```
APPINSIGHTS_INSTRUMENTATIONKEY=31F46B0B-16A1-45E9-B859-1478B458C651
APPSETTING_APPINSIGHTS_INSTRUMENTATIONKEY=31F46B0B-16A1-45E9-B859-1478B458C651
```

When the application insights framework finds this key, it automatically starts reporting to Application Insights in Azure. That means that you can report local events by setting this configuration key either in your appsettings.json or as an environment variable:

appsettings.json:
```
{ 
 "ApplicationInsights" : {
    "InstrumentationKey" : "31F46B0B-16A1-45E9-B859-1478B458C651"
  }
}
``` 

Success:
![AI Live](/img/ai-live.png)
