---
layout: default
title: App Service Environment
category: Azure
---
# App Service Environment

The App Service Envionment is an isolation level inside the Azure services. When you create an App Service Environment, you create a private network inside Azure, which you can connect to your local network via VPN or ExpressRoute. Resources created in the App Service Environment will not get public IP-adresses or DNS names by default.

## Creating resources in the App Service Environment
When you create a resource in the App Service Environment, you can do that just like creating resources in the public cloud. When you create the Service Plan for hosting the Resource, you get an extra option under Location if you have App Service Environments in your subscription: 

![ASE Location](/img/ASE-Location.png)

