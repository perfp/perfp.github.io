---
layout: default
title: Tuning the Application Insights telemetry
category: Design
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

Application Insights Telemetry (unconfigured): 
{
    "name":"Microsoft.ApplicationInsights.Dev.Message",
    "time":"2018-04-23T20:33:59.5821404Z",
    "data":{
        "baseType":"MessageData",
        "baseData":{
            "ver":2,
            "message":"Executing action method TelemetryDemo.Controllers.PersonController."
            "GetPerson (TelemetryDemo) with arguments (213) - ModelState is Valid",
            "severityLevel":"Information",
            
            }
        }
}
```


## Telemetry Initializers
Telemetry initializers are