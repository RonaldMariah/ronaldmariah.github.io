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

**Create a Resource Group and an Azure Kubernetes Cluster**

```
$aksClusterGroupName="azure-arc"
$aksName="${aksClusterGroupName}-aks"
$resourceLocation="West Europe"
az group create -g $aksClusterGroupName -l $resourceLocation

az aks create \
    --resource-group $aksClusterGroupName \
    --name $aksName \
    -enable-aad \
    --generate-ssh-keys
```

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arc-kubernetes-app-service/Screenshot 2021-12-01 105418.png" />

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arc-kubernetes-app-service/Screenshot 2021-12-01 141200.png" />

**Create a Public IP and obtain the AKS credentials**

```
$infra_rg=$(az aks show \
    --resource-group $aksClusterGroupName \
    --name $aksName \
    --output tsv \
    --query nodeResourceGroup)

az network public-ip create \
    --resource-group $infra_rg \
    --name MyPublicIP --sku STANDARD

$staticIp=$(az network public-ip show \
    --resource-group $infra_rg \
    --name MyPublicIP \
    --output tsv \
    --query ipAddress)

az aks get-credentials \
    --resource-group $aksClusterGroupName \
    --name $aksName --admin
```

**Display the AKS Namespaces**

```
kubectl get ns
```

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arc-kubernetes-app-service/Screenshot 2021-12-01 111134.png" />

**Create the Log Analytics Resource Group and Log Analytics resource**

```
$logAnalyticsGroupName = "la-rg"
az group create --location $resourceLocation --name $logAnalyticsGroupName
$workspaceName="$logAnalyticsGroupName-workspace"

az monitor log-analytics workspace create \
    --resource-group $logAnalyticsGroupName \
    --workspace-name $workspaceName
```

**Save and Encode the Log Analytics Workspace ID**

```
$logAnalyticsWorkspaceId=$(az monitor log-analytics workspace show \
    --resource-group $logAnalyticsGroupName \
    --workspace-name $workspaceName \
    --query customerId \
    --output tsv)

$logAnalyticsWorkspaceIdEnc=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($logAnalyticsWorkspaceId))
```

**Save and Encode the Log Analytics Key**

```
$logAnalyticsKey=$(az monitor log-analytics workspace get-shared-keys \
    --resource-group $logAnalyticsGroupName \
    --workspace-name $workspaceName \
    --query primarySharedKey \
    --output tsv)

$logAnalyticsKeyEnc=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($logAnalyticsKey))
```

**Connect the AKS Cluster to Azure Arc**

```
$connectedClusterName = "AzureArcTest1"
az connectedk8s connect \
    --name $connectedClusterName \
    --resource-group $aksClusterGroupName
```

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arc-kubernetes-app-service/Screenshot 2021-12-01 141133.png" />

**Create the App Service Kubernetes Extension**

```
$kubeEnvironmentName="K8sAppServEnv"
$extensionName = "appservice-kube"
$namespace="appservice-ns"

az k8s-extension create \
    --resource-group $aksClusterGroupName \
    --name $extensionName \
    --cluster-type connectedClusters \
    --cluster-name $connectedClusterName \
    --extension-type 'Microsoft.Web.Appservice' \
    --release-train stable \
    --auto-upgrade-minor-version true \
    --scope cluster \
    --release-namespace $namespace \
    --configuration-settings "Microsoft.CustomLocation.ServiceAccount=default" \
    --configuration-settings "appsNamespace=${namespace}" \
    --configuration-settings "clusterName=${kubeEnvironmentName}" \
    --configuration-settings "loadBalancerIp=${staticIp}" \
    --configuration-settings "keda.enabled=true" \
    --configuration-settings "buildService.storageClassName=default" \
    --configuration-settings "buildService.storageAccessMode=ReadWriteOnce" \
    --configuration-settings "customConfigMap=${namespace}/kube-environment-config" \
    --configuration-settings "envoy.annotations.service.beta.kubernetes.io/azure-load-balancer-resource-group=${aksClusterGroupName}" \
    --configuration-settings "logProcessor.appLogs.destination=log-analytics" \
    --configuration-protected-settings "logProcessor.appLogs.logAnalyticsConfig.customerId=${logAnalyticsWorkspaceIdEnc}" \
    --configuration-protected-settings "logProcessor.appLogs.logAnalyticsConfig.sharedKey=${logAnalyticsKeyEnc}"
```

*This will take some time for the extension to be installed, check the Azure Portal to ensure that the Extension shows 'Installed'*

**Before**

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arc-kubernetes-app-service/Screenshot 2021-12-01 141112.png" />

**After**

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arc-kubernetes-app-service/Screenshot 2021-12-01 143020.png" />

*Once 'Installed', if we now do a 'kubectl get ns' we should see the namespaces for the App Services*

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arc-kubernetes-app-service/Screenshot 2021-12-02 091832.png" />

**Get the Extension ID of the App Service extension**

```
$extensionId=$(az k8s-extension show \
    --cluster-type connectedClusters \
    --cluster-name $connectedClusterName \
    --resource-group $aksClusterGroupName \
    --name $extensionName \
    --query id \
    --output tsv)
```

**Get the Connected Cluster ID**

```
$connectedClusterId=$(az connectedk8s show \
    --resource-group $aksClusterGroupName \
    --name $connectedClusterName \
    --query id \
    --output tsv)
```

In order for us to deploy Azure App Services or any PaaS Services in future, we will need a custom location. This custom location will be our Kubernetes Cluster connected to Azure Arc.

**Define a name for the Custom Location**

*This location will appear as a custom Region when deploying services like Azure App Service Plans, which we will see later on*

```
$customLocationName="MyAKS-RonaldMariah"
```

**Create the Custom Location**

```
az customlocation create \
    --resource-group $aksClusterGroupName \
    --name $customLocationName \
    --host-resource-id $connectedClusterId \
    --namespace $namespace \
    --cluster-extension-ids $extensionId
```

**Get the ID of the Custom Location**

```
$customLocationId=$(az customlocation show \
    --resource-group $aksClusterGroupName \
    --name $customLocationName \
    --query id --output tsv)
```

**Create the App Service Kubernetes Environment**

```
az appservice kube create \
    --resource-group $aksClusterGroupName \
    --name $kubeEnvironmentName \
    --custom-location $customLocationId \
    --static-ip $staticIp
```

**Create an App Service Plan in the Custom Region (AKS)**

```
az appservice plan create \
    --resource-group $aksClusterGroupName \
    --name appserviceplan \
    --custom-location $customLocationId \
    --is-linux \
    --per-site-scaling
```

**Finally we can now create a Web App on the Preview Azure Portal and use the Custom Region we created earlier**

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arc-kubernetes-app-service/Screenshot 2021-12-01 145136.png" />

Now we are done with setting up Azure App Services on Kubernetes connected to Azure Arc. We can now utilise the benefits of Azure PaaS Services with our existing workloads running in Kubernetes on-premise or any other cloud provider.

Hope you enjoyed the walkthrough of how we can get this set up and using this in production in future!