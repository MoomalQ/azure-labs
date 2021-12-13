# Azure Web App Virtual Network Integration and Forced Tunneling

## Overview
Organizations often struggle with the balance of security and agility when moving into the cloud.  This conflict is even more prevelant in heavily regulated industries like public sector, healthcare, and financial services.  PaaS (platform-as-a-service) offerings provide organizations an opportunity to increase agility and reduce costs versus traditional IaaS (infrastructure-as-a-service).  These benefits come with the drawback of less control because of shared responsibilty where the cloud provider fully manages the operating system, hypervisor, and physical hardware and shares responsibilty with the consumer for the layers above.  These layers include network controls and application configuration.  What the cloud provider allows the consumer to configure in those layers can vary significantly across cloud providers and services.

A common requirement for organizations is to mediate traffic leaving their cloud tenant with a security stack (firewall, forward proxy, NIDS, etc).  With PaaS services like [Azure App Services](https://docs.microsoft.com/en-us/azure/app-service/) this was historically a challenge.  This challenge was usually addressed by deploying applications into a dedicated environments called an [ASE (App Service Environment)](https://docs.microsoft.com/en-us/azure/app-service/environment/intro) into customer Virtual Networks which allowed customers to route non-Microsoft-management traffic to a security stack.  Unfortunately, ASEs are signiifcantly complex to configure and manage and come with a signficant cost.  

In early 2020 Microsoft introduced (Regional VNet [Virtual Network) Integration](https://azure.github.io/AppService/2020/02/27/General-Availability-of-VNet-Integration-with-Windows-Web-Apps.html).  This new feature allows organizations to use non-isolated [App Service Plans](https://docs.microsoft.com/en-us/azure/app-service/overview-hosting-plans) such as Standard and Premium plans while still being able to route traffic to a mediation security stack.  

This ARM template deploys a small lab in Azure that demonstrates Regional VNet Integration feature.  It uses a [hub and spoke architecture](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) where Internet-bound traffic generated from an Azure Web App is routed to an instance of Azure Firewall where it is mediated.  The Azure Firewall instance has been configured to write its logs to a Log Analytics Workspace.  [These Kusto queries](https://docs.microsoft.com/en-us/azure/firewall/log-analytics-samples) can be used to analyze the data written to Log Analytics.

The template also deploys a [simple Python Web App](https://github.com/mattfeltonma/python-sample-web-app) which pulls data from two public APIs. One API returns the current time and the other API returns a random quote from the greatest TV show of all time, Breaking Bad.

![lab image](https://github.com/mattfeltonma/azure-labs/blob/master/app-service-force-tunnel/lab_visual.png)

## Installation
1.  Create a resource group.
2.  Deploy the template to the resource group using the New-AzResourceGroupDeployment cmdlet.  Example: **New-AzResourceGroupDeployment -Name "myDeployment" -ResourceGroupName "myResourceGroup" -TemplateUri "https://raw.githubusercontent.com/mattfeltonma/azure-labs/master/app-service-force-tunnel/deploy.json"**  
3.  You will be prompted to provide an administrator user name and password for the local administrator accounts on the virtual machines.
4.  Use a web browser or curl to access the website.




