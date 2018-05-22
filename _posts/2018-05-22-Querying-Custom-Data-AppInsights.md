---
layout: default
title: Querying custom data in Application Insights
category: Azure
---

## Querying custom data in Application Insights
Using Application Insights Analysis is a really powerful way to query the telemetry data from your system. Using a simple, but powerful query language you can get interesting data: 

```
requests
| where duration  > 500ms
``` 

## Logging custom data
Using the ApplicationInsights SDK, you can publish custom telemetry events:

``` 
var client = new TelemetryClient();
Map<String, String> properties = new HashMap<>();
properties.put("updateVersion", updateVersion);

//send the event
telemetry.trackEvent("VersionReceived", properties);
```

## Querying custom data 
You can query custom properties using the customDimensions value.
``` 
customEvents
| where customDimensions.updateVersion == "12.2"
```

## Querying structured data
If you want do trace more complex data, you can serialize objects to json before storing them:
``` 
properties.put("dataReceived", jsonReceived.ToString());

//send the event
telemetry.trackEvent("VersionReceived", properties);
``` 

This can be queried using the extractjson function: 
```
customEvents
|extend productnumber = extractjson("$.Product.ProductNumber", 
   tostring(customDimensions.json))
| project productnumber, timestamp
```
