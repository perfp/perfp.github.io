---
layout: default
title: App Service Environment
category: Azure
---
# App Service Environment

The App Service Envionment (ASE) is an isolation level inside the Azure services. When you create an ASE , you create a private virtual network (vnet) inside Azure, which you can connect to your local network via VPN or ExpressRoute. Resources created in the ASE will not get public IP-adresses or DNS names by default. 

## Creating resources in the App Service Environment
When you create a resource in the ASE, you can do that just like creating resources in the public cloud. When you create the Service Plan for hosting the Resource, you get an extra option under Location if you have App Service Environments in your subscription: 

![ASE Location](/img/ASE-Location.png)


## Portal Access to resources in the App Service environment
Some resources in the ASE is not available in the Azure Portal if you're starting it from the public internet. In that case you need to log on to a server inside the private vnet in Azure, typically referred to as a "jumpbox" and start the portal from there.

The error you get in the public portal when trying to access resources that are only available inside the ASE usually looks like this:

![ASE Error](/img/ASE Error message.PNG)





