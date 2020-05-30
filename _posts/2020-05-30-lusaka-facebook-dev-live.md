---
title: Availability and Resiliency in Azure
categories:
- Azure
- Facebook
- Livestream
exerpt: "Availability and Resiliency in Azure"
---

Useful docs for understanding Azure's global infrastructure can be found [here](https://azure.microsoft.com/en-us/global-infrastructure/regions/)

**Azure Regions**

A region is a set of datacenters deployed within a latency-defined perimeter and connected through a dedicated regional low-latency network.

With more global regions than any other cloud provider, Azure gives customers the flexibility to deploy applications where they need to. Azure is generally available in 53 regions around the world, with plans announced for 7 additional regions.

**Geographies**

A geography is a discrete market, typically containing two or more regions, that preserves data residency and compliance boundaries.

Geographies allow customers with specific data-residency and compliance needs to keep their data and applications close. Geographies are fault-tolerant to withstand complete region failure through their connection to our dedicated high-capacity networking infrastructure.

**Availability Zones**

Availability Zones are physically separate locations within an Azure region. Each Availability Zone is made up of one or more datacenters equipped with independent power, cooling, and networking.

Availability Zones allow customers to run mission-critical applications with high availability and low-latency replication.

**What is resiliency in Azure?**

Comprehensive set of native business continuity solutions, providing high availability, disaster recovery, and backup to protect your mission critical applications and data.

**High availability**

Maintaining acceptable continuous performance despite temporary failures  in services, hardware, datacenters, or  fluctuations in load

**Disaster recovery**

Protection against loss of an entire region through asynchronous replication for failover of virtual machines and data using services such as Azure Site Recovery and geo-redundant storage (GRS)

**Backup**

Replication of virtual machines and data to  one or more regions  using Azure Backup.

**Blast radius**

The radius of protection for applications and data.  For example, Availability Sets protect applications within a datacenter, and Availability Zones protect applications and data in an Azure region

**Data residency boundary**

Two regions that share the same regulatory requirements  for data replication and storage for the country or region in  which they operate. 

**ISO-22301 Certification**

Azure is certified under the first international standard to demonstrate the ability to prevent, mitigate, respond to  and recover from  disruptive incidents.

More Links for furthur reading

[Azure Architecture Center](aka.ms/architecture) - Guidance for architecting solutions on Azure using established patterns and practices.

[Azure resiliency solutions](azure.com/resiliency) - Build with confidence with high availability, disaster recovery, and backup

[Azure Availability Zones](aka.ms/AvailabilityZones) - High availability for your most demanding mission-critical applications and data

[Azure regions](aka.ms/AzureRegions) - Azure has more global regions than any other cloud provider—offering the scale needed to bring applications closer to users around the world, preserving data residency, and offering comprehensive compliance and resiliency options for customers.
