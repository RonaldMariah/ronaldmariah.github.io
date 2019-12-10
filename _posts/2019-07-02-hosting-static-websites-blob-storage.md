---
title: Hosting static websites from Azure Storage
categories:
- Azure
exerpt: "One of the capabilities of Azure Storage is you can host and serve static web content. In this post, I demonstrate how this can be achieved."
---

One of the capabilities of Azure is you can host and serve static web content directly from Azure Storage. In this post, I demonstrate how this can be achieved.

Hosting static content directly from Azure Storage provides cost saving benefits over hosting the content on Azure Web Apps.

First we will need a Resource Group. To create one, use the following Azure CLI command

```
az resource group create --name <reosurce-group-name> --location <location>
```

Next, we will need an Azure Storage Account, which we can create using the following Azure CLI command

```
az storage account create --sku Standard_LRS --location <location> --kind StorageV2 --access-tier Hot --name <storage-account-name> --resource-group <resource-group-name>
```

Once the storage account is created, note the response from the command which shows the JSON output of the storage account that was created.
There is a part that shows the "primaryEndpoints". Note down the entry for "web"

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/hosting-static-websites/primary-endpoints.png" />

This URL for "web" will be the public endpoint we will use when browsing to the static website. If we browse to it now, we will get a 404.

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/hosting-static-websites/404-static-website.png" />

Our static content will need to be stored in a container in our blob storage account.
This container is a special container called $web, which is the root of the static website.

Use the following Azure CLI command to create one.

```
az storage container create --account-name <storage-account-name> --name $web
```

<h2>Copying a static website to blob storage</h2>

We will use an already existing static website, found on <a href="https://github.com/cloudacademy/static-website-example">Github</a>

Clone the repo using the following command

```
git clone https://github.com/cloudacademy/static-website-example.git
```

Once the repository is clone, next we will need to copy the static content to our Azure Storage. For this we can use a variety of different ways: AzCopy, Azure Storage Explorer, Azure Portal etc.

In this post, I'll use AzCopy, which you can find the latest version at https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10

We will first need a Shared Acess Signature (SAS) token in order to use AzCopy.
We can generate one from the Azure Portal

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/hosting-static-websites/sas-generate-portal.png" />

Once the SAS Token is generated, use it in the following AzCopy commands to copy the contents of the repo into the $web contianer

```
..\azcopy copy "assets" "https://<storage-account-name>.blob.core.windows.net/$web?<sas-token>" --recursive=true
```
```
..\azcopy copy "images" "https://<storage-account-name>.blob.core.windows.net/$web?<sas-token>" --recursive=true
```
```
..\azcopy copy "error" "https://<storage-account-name>.blob.core.windows.net/$web?<sas-token>" --recursive=true
```
```
..\azcopy copy "index.html" "https://<storage-account-name>.blob.core.windows.net/$web?<sas-token>"
```

Once all the content is copied over, we can enable the Static Website feature on Azure Blob Storage.

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/hosting-static-websites/enable-static-website.png" />

Once this is enabled, browse to the public endpoint for the static website.

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/hosting-static-websites/browse-static-website.png" />

Now that we hosted our static website in Azure Storage, we can now add an Azure CDN on top of this storage account so that the users don't experience latency issues when they are visiting our webpage from a location far away from our storage account's location.

And we done!


