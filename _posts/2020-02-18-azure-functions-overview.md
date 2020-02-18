---
title: Azure Functions - What is Azure Functions?
categories:
- Azure
exerpt: "In this post I explain what is Azure Functions, why and when would you use it."
---

There are two main concepts when it comes to Azure Functions, and that is "Events" and "Code".
The concept of Azure Functions is that "Events" trigger "Code"
Some examples of events are, new data being available (item being put onto a queue or blob storage), or an HTTP request is received for running some code based off a webhook or building a REST API.

Before we go furthur with Azure Functions, here's a quick recap on the different cloud models when it comes to building applications.

**Infrastructure as a Service (IaaS)**
In this model, the cloud provider, provisions a virtual server for you (the cloud customer), to use however you wish. You get to choose the operating system, and have full control over the OS. You are then also responsible for ensuring that the system is patched regularly with the latest updates, and ensuring that your application is running correctly. In this model, you pay for the virtual machine and storage that is being utilised.

An example of IaaS is Virtual Machines.

**Platform as a Service (PaaS)**
In this model, you choose a hosting plan on which to host your application. You get to choose the specification of the servers that will be running in the background as well as the scaling options that will be available to you. You will not be required to maintain the underlying servers as this will be managed by the Cloud Provider. The prcing model is also pay as you go, and you pay for the duration that the service is provisioned.

An exmaple of PaaS is Azure Web Apps and Azure Web Jobs.


Azure Functions is built on top of the Azure Functions SDK, which provides a simplified programming model for you to develop your code and respond to events. This programming model eliminates the bloilerplate code and allows you to focus on the business requirements.

There are two pricing models for Azure Functions, Consumption Plan or have it run in an App Service Plan

In the Consumption Plan, you pay for what you use and also have automatic scaling based on the needs of your application. This can prvide dramatic cost saving since when there are no events to be processed, there are no servers that are needed, so you don't pay for anything.

- Billing on the Consumption Plan is based on:
    - The number of executions
    - CPU Time (s) x RAM (GB)

Azure also provides 1 million free executions or 400,000GB-s utilisation on the Consumption Plan.

With the Consumption Plan, function execution is limited to 5 minutes. This is a good thing as it prevents functions from running a long period of time and getting build because of a deadlock in your code.

Azure Function can also run on an App Service Plan on the Basic tier or higher.
In this model, it provides pridictable costing, several pricing tiers and no duration constraint (functions can run longer than 5 minutes).

There is also a Azure Functions Premium plan which is in preview and provides Virtual Network integration with Azure Functions as well as improved performance.

For more details on the billing you can check the following [link](https://azure.microsoft.com/en-us/pricing/details/functions/) 

The Azure Functions runtime is also available as a Docker container, which you can use to run Azure Function from on-premise as well as other Cloud Providers that support running docker contianers.

**Benefits of Azure Functions**
Some of the benefits of Azure Functions are:
    - Rapid and simple development
    - Ability to create new function in the Azure Portal directly for rapid prototyping
    - Eliminates boilerplate code
    - Has all of the power of Azure Web Apps (Kudu, CI, Custom Domains, and Settings)
    - Cost effective pricing - pay for what you use (Consumption Plan)
    - No maintenance of servers

**So what are Azure Functions Serverless?**
Azure functions is serverless when you choose the consumption based model since you delegate the maintenance and management of servers to a third party (Cloud Provider)

**Some real world examples of Azure Functions**
Imagine you have a small website, you then decide to add little bits of functionality for different things like, reporting on errors, and having the ability to generate reports nightly. As your website scales, these extra functionality could take up valuable resources when being hosted on the same server that you are using to serve HTTP requests for users.

With Azure Functions, you could move the reporting of errors into an Azure Function that is triggered based on an HTTP event and writes the error to Table Storage. You can also move the functionality for generating reports nightly into a different Azure Function that triggers based off a schedule (nightly at 12am).

This ends an introduction to Azure Functions. In the next post we look at creating an Azure Function.

Thank!