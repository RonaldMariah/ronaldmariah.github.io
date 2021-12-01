---
title: Deploying Azure App Services on Azure Arc enabled Kubernetes Clusters
categories:
- Azure
- Azure Arc
- Kubernetes
- Azure App Services
exerpt: "Deploying Azure App Services on Azure Arc enabled Kubernetes Clusters"
---

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arc-kubernetes-app-service/azure-arc-control-plane.png" />

- Azure Arc enables customers to manage resources outside of Azure. Resources includes
    - Servers - both physical and virtual machines running Windows or Linux.
    - Kubernetes clusters - supporting multiple Kubernetes distributions.
    - Azure data services - Azure SQL Managed Instance and PostgreSQL Hyperscale services.
    - SQL Server - enroll instances from any location with SQL Server on Azure Arc-enabled servers.

This post focuses on the Kubernetes clusters aspect, which allows customers to attach and configure Kubernetes clusters running anywhere. You can connect your clusters running on other public clouds (GCP, AWS) or clusters running on your on-premise data centre (on VMware vSphere, Azure Stack HCI) to Azure Arc.

If you have an Azure Arc-enabled Kubernetes cluster, you can use it to create an App Service enabled custom location and deploy web apps, function apps, and logic apps to it.

Azure Arc-enabled Kubernetes lets you make your on-premises or cloud Kubernetes cluster visible to App Service, Functions, and Logic Apps in Azure. You can create an app and deploy to it just like another Azure region.

If you do not have a Kubernetes cluster, you can create one on Azure (AKS) using the following Azure CLI commands

```
$aksClusterGroupName="<Define your AKS Resource Group name>"
$aksName="${aksClusterGroupName}-aks"
$resourceLocation="West Europe"
az group create -g $aksClusterGroupName -l $resourceLocation
````

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arc-kubernetes-app-service/Screenshot 2021-12-01 105418.png" />

Create a Kuberbetes Cluster with the following

```
az aks create --resource-group $aksClusterGroupName --name $aksName --enable-aad --generate-ssh-keys
```

Create a Public IP and obtain the AKS credentials

```
$infra_rg=$(az aks show --resource-group $aksClusterGroupName --name $aksName --output tsv --query nodeResourceGroup)
az network public-ip create --resource-group $infra_rg --name MyPublicIP --sku STANDARD
$staticIp=$(az network public-ip show --resource-group $infra_rg --name MyPublicIP --output tsv --query ipAddress)
	
az aks get-credentials --resource-group $aksClusterGroupName --name $aksName --admin
```

Display the AKS Namespaces

```
kubectl get ns
```

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arc-kubernetes-app-service/Screenshot 2021-12-01 111134.png" />