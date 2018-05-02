---
layout: default
title: Tuning the Application Insights telemetry
category: Azure
---
## Tuning the Application Insights telemetry
Application Insights is a great tool for monitoring Azure Services and applications. We can monitor our application with great level of detali, and the services themselves, as well as the frameworks also expose a lot of telemetry events. But when we're working with sensitive data, we want to control which data is exposed through the telemetry.

### Telemetry Processor
A telemetry processor is a class that you inject in the telemetry processing chain. It can inspect and change the content of a telemetry item before it is sent from the application.

Given this controller:

```csharp
public class PersonController : Controller
 {
    [HttpGet]
    public Person GetPerson(string nationalIdentityNumber)
    {
        return new Person();
    }
 }
```
We get the following output (modified for clarity):
```javascript
Application Insights Telemetry (unconfigured): 
{
    "name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-04-23T20:33:59.5433669Z",
    "data":{
        "baseType":"MessageData",
        "baseData":{
            "ver":2,
            "message":"Request starting HTTP/1.1 "
            "GET http://localhost:26428/api/Person?nationalIdentityNumber=213",
            "severityLevel":"Information",
        "properties":{
            "Method":"GET",
            "QueryString":"?nationalIdentityNumber=213","Protocol":"HTTP/1.1",
            "Path":"/api/Person",
            "Scheme":"http"
            }
        }
    }
}
```
To change this output, you can create a telemetry processor that you inject into the chain of telemetry processors already registered. Add this code to your Startup.cs:
```csharp
var builder =  TelemetryConfiguration
                .Active
                .TelemetryProcessorChainBuilder;
builder.Use((next) => new UrlFilter(next));
builder.Build();
``` 

The processor class can look like this:
```csharp
 public class UrlFilter : ITelemetryProcessor
    {
        private ITelemetryProcessor Next { get; set; }
        public UrlFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }


        public void Process(ITelemetry item)
        {
            var telemetryItem = item as TraceTelemetry;
            if (telemetryItem != null)
            {
                
                if (telemetryItem.Message.Contains("nationalIdentityNumber=")){
                   // Replace the user identity 
                }
            }
            
            this.Next.Process(item);
        }
    }
```

After running the processor, the output can look like this:
```javascript
Application Insights Telemetry (unconfigured): 
{
    "name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-04-23T20:33:59.5433669Z",
    "data":{
        "baseType":"MessageData",
        "baseData":{
            "ver":2,
            "message":"Request starting HTTP/1.1 "
            "GET http://localhost:26428/api/Person?nationalIdentityNumber=<hidden>",
            "severityLevel":"Information",
        "properties":{
            "Method":"GET",
            "QueryString":"?nationalIdentityNumber=<hidden>","Protocol":"HTTP/1.1",
            "Path":"/api/Person",
            "Scheme":"http"
            }
        }
    }
}
```

