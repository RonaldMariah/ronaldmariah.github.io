---
title: General Availability of Premium Azure File Shares
categories:
- Azure
- Storage
exerpt: "Azure now supports hosting File Shares on Premium backed storage media (SSDs) and are useful for IO-intensive workloads, hosting databases and high-performance computing (HPC)."
---

Azure now supports hosting File Shares on Premium backed storage media (SSDs) and are useful for IO-intensive workloads, hosting databases and high-performance computing (HPC).

The easiest way to create a Premium File Share is using the Azure CLI

``` 
az group create --name myresourcegroup --location westeurope

az storage account create `
    --name mypremiumfileshare `
    --resource-group myresourcegroup ` 
    --location westeurope `
    --sku Premium_LRS `
    --kind FileStorage
```

**Some limitiations on Premium File Shares:**
* Only supports LRS (Locally redundant storage)
* There is no automatic way to convert a File Share on Standard storage to Premium.

**There are alternate ways to get the content stored in Standard storage to Premium**
* Azure Import/Export
* Azure File Sync
* AzCopy
* Robocopy

For more information on Azure Premium File Shares, visit the Microsoft Docs [here](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-premium-fileshare)