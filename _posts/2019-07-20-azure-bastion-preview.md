---
title: Accessing Azure VMs over RDP and SSH on Private IPs with Azure Bastion Preview
categories:
- Azure
- CDN
- Security
exerpt: "Securely connecting from the outside world to workloads and virtual machines on private networks can be challenging. Here's how Azure Bastion can help!"
---

Securely connecting from the outside world to workloads and virtual machines on private networks can be challenging. Here's how Azure Bastion can help!

***Overview***

Azure Bastion is a new managed PaaS service that provides seamless RDP and SSH connectivity to your virtual machines over the Secure Sockets Layer (SSL). This is completed without any exposure of the public IPs on your virtual machines. Azure Bastion provisions directly in your Azure Virtual Network, providing bastion host or jump server as-a-service and integrated connectivity to all virtual machines in your virtual networking using RDP/SSH directly from and through your browser and the Azure portal experience. 

For more information on Azure Bastion, check the [docs](https://azure.microsoft.com/en-us/services/azure-bastion/)

**Deployment**

Using the [preview portal](https://aka.ms/BastionHost), we can deploy a Bastion host together with a new or existing Virtual Network

Here's a few screenshots of the Bastion and Virtual Network setup I have.

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/main/assets/azure-bastion-preview/bastion-create.png" />

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/main/assets/azure-bastion-preview/bastion-vnet-create.png" />

One thing to note is that, in order to deploy a Bastion host into an existing Virtual Network, that VNET needs to have a subnet called AzureBastionSubnet with atleast the /27 address space size.

Once we have the VNET and Bastion host provisioned, we can now deploy a Virtual Machine into the default subnet of the VNET we just created and connect to it on it's private IP.

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/main/assets/azure-bastion-preview/create-vm-bastion.png" />

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/main/assets/azure-bastion-preview/create-vm-networking.png" />

Ensure that there are no Public IPs, or inbound ports opened on the VM. Access to the VM will be governed by the Bastion host on private IPs.

Once the VM, is deployed, when we click 'Connect", we see a new option for connecting to the Virtual Machine. Provide the necessary details and 'Connect'.

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/main/assets/azure-bastion-preview/portal-connect.png" />

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/main/assets/azure-bastion-preview/ssh-portal-bastion.png" />

And we done!