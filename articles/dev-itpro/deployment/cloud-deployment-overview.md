---
# required metadata

title: Dynamics 365 for Finance and Operations, Enterprise edition cloud deployment overview 
description: Dynamics 365 for Finance and Operations, Enterprise edition now supports running business processes in the cloud.
author: kfend
manager: AnnBe
ms.date: 11/20/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-platform
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Developer, IT Pro
# ms.devlang: 
ms.reviewer: kfend
ms.search.scope: Operations
# ms.tgt_pltfrm: 
ms.custom: 60373
ms.assetid: 
ms.search.region: Global
# ms.search.industry: 
ms.author: kfend
ms.search.validFrom: 2016-02-28
ms.dyn365.ops.version: Platform Update 8

---
# Dynamics 365 for Finance and Operations, Enterprise edition cloud deployment overview

[!include[banner](../includes/banner.md)]
 
Working with Microsoft to deploy Microsoft Dynamics 365 for Operations in the cloud requires that you understand the environment and subscription that you are deploying to, who can perform which tasks, and the data and customizations that you need to manage. 
We recommend that you sign up for the Full Microsoft FastTrack for Dynamics 365 to help speed your deployment and implementation - it's a program that provides training and consulting to help you realize business value faster. For more information, see [Microsoft FastTrack for Dynamics 365 overview](../../fin-and-ops/get-started/fasttrack-dynamics-365-overview.md). If you choose to use the Essentials FastTrack program instead, you will be using the Implementation Project Methodology in Lifecycle Services (LCS) to help you manage your implementation project. 

## Customer lifecycle, subscriptions, and deployment topologies
Microsoft assumes that all customers will follow a lifecycle similar to the following for all cloud deployments, and therefore need different environment topologies at each phase. 

- Evaluate
- Develop customizations, if needed
- Install and test customizations and partner solutions on a tier-1 sandbox (Development or test environment) 
- Test customizations, partner solutions and data configuration on a tier-2 sandbox environment
- Deploy customizations and data configurations to a production environment with high availability

At some phases of a project, you may have all of the environments live at once. For more information, about the default licenses and tiers that are available, see the [Dynamics 365 Licensing Guide](http://download.microsoft.com/documents/en-us/dynamics/pricing/Dynamics_365_Enterprise_edition_Licensing_Guide.pdf).

You may hear the terms, Customer, Partner, and Microsoft subscriptions. A *customer or partner subscription* means that the customer or partner brings their own Azure subscription and deploys Dynamics 365 for Finance and Operations environments to it, for evaluation and development purposes only. The customer or partner pays for the resources deployed to their Azure subscription based on the Azure price list. 
A *Microsoft subscription* means that the customer purchases Dynamics 365 for Finance and Operations licenses which will then allow them to deploy environments to an Azure subscription which is managed by Microsoft, therefore, the customer has no separate Azure billing. With each Enterprise offer, three environments are included by default. 

- One Tier-1 sandbox which is a development or build environment.
- One Tier-2 sandbox (multi-box environment) for user acceptance testing (UAT).
- One production environment with High Availability (HA). This environment is managed and operated by Microsoft.

Additional environments may be purchased as add-ons. For information about licensing and what is included in Microsoft Dynamics 365, see the [Microsoft Dynamics 365 Enterprise edition licensing guide](http://download.microsoft.com/documents/en-us/dynamics/pricing/Dynamics_365_Enterprise_edition_Licensing_Guide.pdf).

Here's how the lifecycle maps to the available environments.

| Lifecycle phase               | Environment                               | Subscription                                        |
|-------------------------------|-------------------------------------------|-----------------------------------------------------|
| Evaluation and analysis       | Demo, one-box                             | Customer or partner subscription.                                |
| Customize                     | Dev/build, one-box                        | Microsoft subscription, partner/customer subscription, or local VM
| User acceptance testing (UAT) | Tier-2 sandbox, multi-box environment | Microsoft subscription                              |
| Go live                       | Production, High Availability multi-box environment                    | Microsoft subscription                              |

## Features of the Finance and Operations production instance
The Finance and Operations application production instance has the following capabilities.

## Security and compliance
Dynamics 365 for Operations is PA-DSS 3.1 certified which means that all communications between components are secured out-of-the-box. 

All Dynamics 365 for Operations front-end virtual machines in Microsoft Azure are configured during deployment to only accept TLS 1.2. 

> [!IMPORTANT]
> Customers who have administrator access to Microsoft-managed sandboxes, including any add-on sandboxes purchased, must follow these guidelines:
> - By default, automatic Windows update is enabled for all Tier 1 - 5 sandboxes and should NOT be disabled. This ensures that any time that Microsoft pushes security or critical infrastructure updates to your environment, your environment receives the latest set of updates and is updated each month with the operating system fixes that Microsoft releases.  
> -	Admin passwords on these environments should NOT be changed. Environments that have admin passwords changed will be flagged by Microsoft. Microsoft reserves the right to, and will reset the admin password.  
> - Adding new user accounts to any Microsoft managed VM is NOT permitted. Microsoft reserves the right to, and will remove the newly added user accounts without providing notice.

## Remote Desktop (RDP)
### Microsoft Managed environments
Customers are now required to complete additional setup to connect to Dynamics 365 for Finance and Operations virtual machines (VMs) through Microsoft Remote Desktop (RDP). This additional setup applies to all Microsoft-managed environments, including Tier 1 through Tier 5 sandboxes and add-ons. In order to connect to Tier 1 through Tier 5 sandbox environments, you must explicitly enable access from your organization’s IP address space. This can be done by a Lifecycle Services (LCS) user who has access to the **Environment** page (**Maintain** > **Enable Access**) where they can enter the IP address space that will be used to connect to the virtual machines through Remote Desktop. Access rules are either a single IP address (example: 10.10.10.10) or an IP address range (example: 192.168.1.0/24). You may add multiple entries at once as a semi-colon(;) separated list (example: 10.10.10.10;20.20.20.20;192.168.1.0/24). These entries are used to configure the Azure Network Security Group that is associated with your environment’s virtual network. For more information,  see [Filter network traffic with network security groups](/azure/virtual-network/virtual-networks-nsg).

> [!NOTE]
> Ensure that public IP addresses, such as a coffee shop location, are NOT configured.     
 
### Partner/Customer Managed environments 
By default, Remote Desktop is enabled for all non-Microsoft managed environments. We recommend that customers restrict access to any environments that belong to their subscriptions. This can be done by configuring Network Security Group rules on the environments directly in Azure Portal.

## Windows Remoting (WinRM)
Windows Remoting (WinRM) is disabled on all environments. Exceptions to enable WinRM will not be granted for any Microsoft-managed environments. Although you can enable WinRM on environments that belong to your subscriptions through Azure Portal, we strongly recommend that you do not do this.

> [!IMPORTANT]
> Customers need to ensure that RDP and WinRM endpoints are secured through explicit NSG rules as mentioned above. The NSG rules should adhere to the below conditions to ensure the environments are secure and the Intellectual Property is protected.
> - NSG rules should **NOT** using star/zero, opening the environment to internet
> - Wide IP address ranges should not be used.
> - IP address ranges should restrict to the Customer's CORPNET 
> - If computers outside the customer's CORPNET are used to connect to the Sandboxes, only IP addresses of the computers should be added.
> - Azure Datacenter IP Address ranges should not be added

> [!WARNING]
> Microsoft will run periodic tests on the Microsoft Managed environments validating that the environments are sufficiently restrictied.
> Microsoft reserves the right to and will remove any IP Address rules that are deemed not restrictive without providing notice.

## Availability
The guaranteed uptime for Dynamics 365 for Finance and Operations is 99.5%. Planned downtime occurs once a month and lasts no longer than eight hours. Because the work completed during the downtime doesn’t always take eight hours, we will always communicate the estimated amount of time that your environments will be down. [Find support for Microsoft Dynamics 365 for Finance and Operations, Enterprise edition and Dynamics Lifecycle Services](../lifecycle-services/lcs-support.md).

### High-availability features
To ensure service availability, all production environments are protected by using default Azure high availability (HA) features. HA functionality provides ways to avoid downtime caused by the failure of a single node within a datacenter, and DR features protect against outages broadly impacting an entire datacenter. Dynamics 365 for Operations cloud architecture uses Azure availability sets for the compute tier to prevent single-point-of-failure events. For more information about Azure availability sets, see [Azure availability sets guidelines for Windows VMs]/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).
High availability for databases is supported through Azure SQL. For more information, see [Overview of business continuity with Azure SQL Database](/azure/sql-database/sql-database-business-continuity).

### Disaster recovery features
Finance and Operations production environments are configured with Azure disaster recovery support that includes the following:
- Azure SQL active-geo replication for primary databases, with a Recovery Point Estimate (RPO) of < 5 seconds. For more information, see [Overview: Failover groups and active geo-replication](/azure/sql-database/sql-database-geo-replication-overview). 
- Geo-redundant copies of Azure blob storage (containing document attachments) in other Azure regions. For more information, see [Geo-redundant storage](/azure/storage/storage-redundancy#geo-redundant-storage).
- Same secondary region for the Azure SQL and Azure blog storage replication.  

Only primary data stores are supported by replication. This means that some Finance and Operations application components, such as Management Reporter, and Entity store, which use transformed data from the primary database, must be generated after the recovery site has been set up and the service has started. Customer code artifacts and recovered data stores are used to re-deploy the site, with a Recovery Time Objective (RTO) of 10 hours and a Recovery Point Objective of 5 minutes. For more information, see [Azure SQL Database Point in Time Restore](https://azure.microsoft.com/en-us/blog/azure-sql-database-point-in-time-restore/).

## Service availability 
Finance and Operations can be deployed into different Microsoft Azure datacenters using Dynamics Lifecycle Services (LCS). Azure is generally available in datacenters and geographical locations around the world. With Finance and Operations, customers can specify the region or datacenter where their customer data will be stored. Microsoft may replicate data to other regions for data durability, but we will not replicate or move customer data outside the geographical location.

**Important:** Regardless of where customer data is stored, Microsoft does not control or limit the locations from which customers or their end-users may access it.
For more information, see [Where your Finance and Operations data is stored](https://www.microsoft.com/en-us/trustcenter/privacy/dynamics365-operations-location).

## Frequently asked questions

## How do I access a development instance

See [Access instances](../dev-tools/access-instances.md) for information about how to access development instances, configure on-premises development VMs, and find configurations settings for developers and administrators.

### How do I deploy a demo environment
A demo environment includes only Microsoft demo data. You can use a demo environment to explore default features and functionality. For more information, see [Deploy a demo environment](deploy-demo-environment.md).

### How do I move my customizations between environments?
To move customizations from a development to a sandbox or production environment, see [Create a deployable package of your models in order to apply it to a runtime environment](../deployment/create-apply-deployable-package.md)

### Can I request a copy of my production database?
A customer can request a copy of their Finance and Operations production database to be installed on their tier-2 sandbox environment. This request is completed by DSE.

### Can I bring my own domain name?
You can bring your own domain name if it is running Azure Active Directory (AAD), and the administrator of your AAD instance has enabled the Finance and Operations application within their AAD. This is usually done through the office email, after you buy a license. When you click the link to accept the offer, AAD is set up for you.

### Can I add guest AAD accounts as Finance and Operations users?
You can add guest AAD accounts if you have correctly configured them within Azure Active Directory, and enabled the Finance and Operations application within your AAD. 

### Why am I no longer able to see the Private AOS machines in one or more of my Tier 2 through Tier 5 Sandbox environmnents?
The Private AOS VMs were part of your environment configuration as they were needed to secure communication between the AOS and BI machines in the past. With recent updates, all communication between AOS and BI machines are secure directly and no longer need the intermediary Private AOS machines. Therefore, we are in the process of rolling out removing the Private AOS machines. As we are removing the machines in batches, you may notice that only some of your environments have the Private AOS machines removed. This change will not impact functionality or security in any way and will be transparent to you.

### Why am I no longer able to Remote Desktop into one or more of my Tier 1 through Tier 5 Microsoft managed Sandbox environments?
Microsoft managedTier 1 through Tier 5 sandbox environments require Remote Desktop and Windows Remoting management endpoints to be restricted to specific IP Address sets. Microsoft regularly validates the environments are sufficiently restricted. Microsoft reserves the right to and will remove any IP Address ranges that are not deemed restrictive. You may not be able to Remote Desktop into your environment in the event your IP address is not in the access list, has changed from the IP address listed in the access list or if Microsoft deleted the IP address range. You will need to add the IP address of your computer from which you are connecting by following the steps specified in the **Remote Desktop (RDP)** section in this document to regain access to the environment.
