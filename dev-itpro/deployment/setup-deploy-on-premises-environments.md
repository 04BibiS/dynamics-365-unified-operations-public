---
# required metadata

title: Set up and deploy on-premises environments
description: This topic provides information about how to plan, set up, and deploy an on-premises environment.
author: kfend
manager: AnnBe
ms.date: 06/14/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-platform
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Application User, Developer, IT Pro
# ms.devlang: 
ms.reviewer: kfend
ms.search.scope: Core
# ms.tgt_pltfrm: 
ms.custom: 55651
ms.assetid: 
ms.search.region: Global
# ms.search.industry: 
ms.author: sarvanisathish
ms.search.validFrom: 2016-08-30
ms.dyn365.ops.version: Platform update 8

---

# Set up and deploy on-premises environments

This topic describes how to plan your deployment, set up the infrastructure, and deploy Microsoft Dynamics 365 for Finance and Operations, Enterprise edition (on-premises).

## Finance and Operations components

The Finance and Operations application consists of three main components:

- Application Object Server (AOS)
- Business Intelligence (BI)
- Financial Reporting/Management Reporter

These components depend on the following system software:

- Microsoft Windows Server 2016
- Microsoft SQL Server 2016 SP1, which has following features:

    - Full-text index search is enabled.
    - SQL Server Reporting Services (SSRS).
    - SQL Server Integration Services (SSIS).

    > [!NOTE]
    > The application won't run if Full Text Search isn't enabled.

- SQL Server Management Studio
- Standalone Microsoft Azure Service Fabric
- Microsoft Windows PowerShell 5.0 or later
- Active Directory Federation Services (AD FS) on Windows Server 2016

    The domain controller must be Microsoft Windows Server 2012 R2 or later and must have a domain functional level of 2012 R2 or more.

    For more information about domain functional levels, see the following pages:

    - [What Are Active Directory Functional Levels](https://technet.microsoft.com/en-us/library/cc787290(v=ws.10).aspx)
    - [Understanding Active Directory Domain Services Functional Levels](https://technet.microsoft.com/en-us/library/understanding-active-directory-functional-levels(v=ws.10).aspx)

## Lifecycle Services

Finance and Operations bits are distributed through Microsoft Dynamics Lifecycle Services (LCS). Before you can deploy, you must purchase license keys through the [Enterprise Agreements](https://www.microsoft.com/en-us/Licensing/licensing-programs/enterprise.aspx) channel and set up an on-premises project in LCS. Deployments can be initiated only through LCS. For more information about how to set up on-premises projects in LCS, see [Create an on-premises project in Lifecycle Services](../lifecycle-services/lbd-create-lcs-on-prem-project.md).

## Authentication

The on-premises application works with AD FS. To interact with LCS, you must also configure Azure Active Directory (Azure AD).

## Standalone Service Fabric

Finance and Operations uses standalone Service Fabric. For more information, see the [Service Fabric documentation](/azure/service-fabric/).

## Infrastructure

Finance and Operations is designed to work on a hyper-converged architecture. The hardware configuration includes the following components:

- Standalone Service Fabric cluster that is based on Windows Server 2016 virtual machines (VMs)
- Microsoft SQL Server (Both Clustered SQL and Always-On are supported.)
- AD FS for authentication
- Server Message Block (SMB) version 3 file share for storage
- Optional: Microsoft Office Server 2017

For more information, see [System requirements](../get-started/system-requirements.md) and Sizing guidelines.

### Hardware layout

Plan your infrastructure and Service Fabric cluster, based on the recommended sizing in [Hardware sizing for on-premises environments](../get-started/hardware-sizing-on-premises-environments.md). For more information about how to plan the Service Fabric cluster, see [Plan and prepare your Service Fabric standalone cluster deployment](/azure/service-fabric/service-fabric-cluster-standalone-deployment-preparation).

The following table shows an example of a hardware layout. This example is used throughout this topic to illustrate the setup.

> [!NOTE]
> The Primary node of the Service Fabric cluster must have at least three nodes. In this example, **OrchestratorType** is designated as the Primary node type.

| Machine purpose                                 | Machine name    | IP address    |
|-------------------------------------------------|-----------------|---------------|
| Domain controller                               | DAX7SQLAODC1    | 10.179.108.2  |
| AD FS                                           | DAX7SQLAOADFS1  | 10.179.108.3  |
| File server                                     | DAX7SQLAOFILE1  | 10.179.108.4  |
| SQL Always-On cluster                           | DAX7SQLAOSQLA01 | 10.179.108.5  |
|                                                 | DAX7SQLAOSQLA02 | 10.179.108.6  |
|                                                 | DAX7SQLAOSQLA   | 10.179.108.9  |
| Client                                          | SQLAOCLIENT1    | 10.179.108.11 |
| Service Fabric cluster/AOS 1                    | SQLAOSF1AOS1    | 10.179.108.12 |
| Service Fabric cluster/AOS 2                    | SQLAOSF1AOS2    | 10.179.108.13 |
| Service Fabric cluster/AOS 3                    | SQLAOSF1AOS3    | 10.179.108.14 |
| Service Fabric cluster/Orchestrator 1           | SQLAOSF1ORCH1   | 10.179.108.15 |
| Service Fabric cluster/Orchestrator 2           | SQLAOSF1ORCH2   | 10.179.108.16 |
| Service Fabric cluster/Orchestrator 3           | SQLAOSF1ORCH3   | 10.179.108.17 |
| Service Fabric cluster/Management Reporter node | SQLAOSMR1       | 10.179.108.18 |
| Service Fabric cluster/SSRS node                | SQLAOSFBIN1     | 10.179.108.10 |

## Setup

### Prerequisites

Before you start the setup, the following prerequisites must be in place. The setup of these prerequisites is out of scope for this document.

- Active Directory Domain Services (AD DS) is installed and configured in your network.
- AD FS is deployed.
- SQL Server, SSRS, and Management Studio are installed.

### Overview

The following steps must be completed to set up the infrastructure for Finance and Operations.

1. Plan your domain name and DNS zones.
2. Plan and acquire your certificates.
3. Plan your users and service accounts.
4. Create DNS zones, and add A records.
5. Join VMs to the domain.
6. Add prerequisite software to VMs.
7. Download setup scripts from LCS.
8. Describe your configuration.
9. Install certificates.
10. Set up a standalone Service Fabric cluster.
11. Configure LCS connectivity for the tenant.
12. Set up file storage.
13. Set up SQL Server.
14. Configure the databases.
15. Encrypt credentials.
16. Set up SSRS.
17. Configure AD FS.

### Plan your domain name and DNS zones

We recommend that you use a publicly registered domain name for your production installation of AOS. In that way, the installation can be accessed outside the network, if outside access is required.

For example, if your company's domain is contoso.com, your zone for Finance and Operations (on-premises) might be d365ffo.onprem.contoso.com, and the host names might be as follows:

- ax.d365ffo.onprem.contoso.com for AOS machines
- sf.d365ffo.onprem.contoso.com for the Service Fabric cluster

### Plan and acquire your certificates

Secure Sockets Layer (SSL) certificates are required in order to secure a Service Fabric cluster and all the applications that are deployed. For your production and sandbox workloads, we recommend that you acquire certificates from a certificate authority (CA) such as [DigiCert](https://www.digicert.com/ssl-certificate/), [Comodo](https://ssl.comodo.com/), [Symantec](https://www.websecurity.symantec.com/ssl-certificate), [GoDaddy](https://www.godaddy.com/web-security/ssl-certificate), or [GlobalSign](https://www.globalsign.com/en/ssl/). If your domain is set up with [Active Directory Certificate Services](https://technet.microsoft.com/en-us/library/cc772393(v=ws.10).aspx) (AD CS), you can create the certificates through AD CS. Each certificate must contain a private key that was created for key exchange, and it must be exportable to a Personal Information Exchange (.pfx) file.

Self-signed certificates can be used only for testing purposes. For convenience, the setup scripts include scripts that generate and export self-signed certificates. As we've mentioned, these certificates can be used for testing purposes only.

| Purpose                                      | Explanation | Additional requirements |
|----------------------------------------------|-------------|-------------------------|
| SQL Server SSL certificate                   | This certificate is used to encrypt data that is transmitted across a network between an instance of SQL Server and a client application. | The domain name of the certificate should match the fully qualified domain name (FQDN) of the SQL Server instance or listener. For example, if the SQL listener is hosted on the machine DAX7SQLAOSQLA, the certificate's DNS name is DAX7SQLAOSQLA.onprem.contoso.com. |
| Service Fabric Server certificate            | This certificate is used to help secure the node-to-node communication between the Service Fabric nodes. This certificate is also used as the Server certificate that is presented to the client that connects to the cluster. | <p>The domain name of the certificate must match the DNS zone where AOS and Service Fabric are hosted. For example, the domain name of the certificate might be \*.onprem.contoso.com or \*.contoso.com.</p><p>If you use a wildcard certificate, the wildcard character applies to only one level. A subdomain, or subject alternative name (SAN), must be applied to the certificate if it has more than one level, such as \*.contoso.com in the previous example.</p> |
| Service Fabric Client certificate            | This certificate is used by clients to view and manage the Service Fabric cluster. | |
| Encipherment Certificate                     | This certificate is used to encrypt sensitive information such as the SQL Server password and user account passwords. | <p>The certificate key usage must include Data Encipherment (10) and should not include Server Authentication or Client Authentication.</p><p>For more information, see [Managing secrets in Service Fabric applications](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-secret-management).</p> |
| AOS SSL Certificate                          | <p>This certificate is used as the Server certificate that is presented to the client for the AOS website. It's also used to enable Windows Communication Foundation (WCF)/Simple Object Access Protocol (SOAP) certificates.</p><p>The Service Fabric Server certificate can be used here if it's a wildcard certificate.</p> | <p>The domain name of the certificate must match the DNS zone where AOS and Service Fabric are hosted. For example, the domain name of the certificate might be \*.d365ffo.onprem.contoso.com, \*.onprem.contoso.com, or \*.contoso.com.</p><p>If you use a wildcard certificate, the wildcard applies to only one level. A subdomain, or SAN, must be applied to the certificate if it has more than one level, such as \*.contoso.com in the previous example.</p> |
| Session Authentication certificate           | This certificate is used by AOS to help secure a user's session information. | This certificate is also the File Share certificate that will used at the time of deployment from LCS. |
| Data Encryption and Data Signing certificate | These certificates are used by AOS to encrypt sensitive information. | |
| Financial Reporting client certificate       | This certificate is used to help secure the communication between the Financial Reporting services and AOS. | |
| Reporting certificate                        | This certificate is used to help secure the communication between SSRS and AOS. | |
| On-Premise local agent certificate           | <p>This certificate is used to help secure the communication between a local agent that is hosted on-premises and LCS.</p><p>This certificate enables the local agent to act on behalf of your Azure AD tenant, and to communicate with LCS to orchestrate and monitor deployments.</p> | |

### Plan your users and service accounts

You must create several user or service accounts for Finance and Operations (on-premises) to work. You must create a combination of group managed service accounts (gMSAs), domain accounts, and SQL accounts. The following table shows the user accounts, their purpose, and example names that will be used in this topic.

| User account                                            | Type           | Purpose | User name |
|---------------------------------------------------------|----------------|---------|-----------|
| Financial Reporting Application Service Account         | gMSA           |         | Contoso\\svc-FRAS$ |
| Financial Reporting Process Service Account             | gMSA           |         | Contoso\\svc-FRPS$ |
| Financial Reporting Click Once Designer Service Account | gMSA           |         | Contoso\\svc-FRCO$ |
| AOS Service Account                                     | gMSA           | This user should be created for future-proofing. We plan to enable AOS to work with the gMSA in upcoming releases. By creating this user at the time of setup, you will help guarantee a seamless transition to the gMSA. | Contoso\\svc-AXSF$ |
| AOS Service Account                                     | Domain account | AOS uses this user in the GA release. | Contoso\\AXServiceUser |
| AOS SQL DB Admin user                                   | SQL user       | Finance and Operations uses this user to authenticate with SQL\*. This user will also be replaced by the gMSA user in upcoming releases. | AXDBAdmin |
| Local Deployment Agent Service Account                  | gMSA           | This account is used by the local agent to orchestrate the deployment on various nodes. | Contoso\\Svc-LocalAgent$ |

\* The SQL user name and password for SQL authentication are secured, because they are encrypted and stored in the file share.

### Create DNS zones and add A records

DNS is integrated with AD DS, and lets you organize, manage, and find resources in a network. Create a DNS forward lookup zone and A records for the AOS host name and the Service Fabric cluster. In this example setup, the DNS zone name is d365ffo.onprem.contoso.com, and the A records/host names are as follows:

- **ax**.d365ffo.onprem.contoso.com for AOS machines
- **sf**.d365ffo.onprem.contoso.com for the Service Fabric cluster

#### Add a DNS zone

Use the following procedure to add a DNS zone.

1. Sign in to the domain controller machine, select **Start**, and start DNS Manager by typing **dnsmgmt.msc**.
2. Right-click the domain controller name, and then select **New Zone** \> **Next**.
3. Select **Primary Zone**.
4. Leave the **Store the zone in Active Directory (available only if the DNS Server is a writeable domain controller** check box selected, and then select **Next**.
5. Select **To all DNS Servers running on Domain Controllers in this domain: Contoso.com**, and then select **Next**.
6. Select **Forward Lookup Zone**, and then select **Next**.
7. Enter the zone name for your setup, and then select **Next**. For example, enter **d365ffo.onprem.contoso.com**.
8. Select **Do not allow dynamic updates**, and then select **Next**.

#### Set up an A record for AOS

In the new DNS zone, create one A record that is named **ax.d365ffo.onprem.contoso.com** for **each** Service Fabric cluster node of the **AOSNodeType** type. Don't create A records for the other node types.

1. Right-click the new zone, and then select **New Host**.
2. Enter the name and IP address of the Service Fabric node. (For example, enter **10.179.108.12** as the IP address.) Then select **Add Host**.

#### Set up an A record for the orchestrator

In the new DNS zone, create an A record that is named **sf.d365ffo.onprem.contoso.com** for **each** Service Fabric cluster node of the **OrchestratorType** type. Don't create A records for the other node types.

1. Right-click the new zone, and then select **New Host**.
2. Enter the name and IP address of the Service Fabric node. (For example, enter **10.179.108.15** as the IP address.) Then select **Add Host**.

### Join VMs to the domain

Join each VM to the domain by completing the steps in [How to join Windows Server 2016 to an Active Directory domain](http://www.tomsitpro.com/articles/join-windows-server-2016-to-ad-domain,2-1063.html). Alternatively, use the following Windows PowerShell script.

```
$domainName = Read-Host -Prompt 'Specify domain name (ex: contoso.com)'
Add-Computer -DomainName $domainName -Credential (Get-Credential -Message 'Enter domain credential')
```

> [!IMPORTANT]
> You must restart the VMs after you join them to the domain.

### Add prerequisite software to VMs

The following table lists the prerequisite software that must be applied to the machines of each node type.

> [!NOTE]
> You must restart your computer after you install the components.

| Node type | Component | Command |
|-----------|-----------|---------|
| AOS       | SNAC – ODBC driver | See [Microsoft ODBC Driver 13.1 for SQL Server - Windows + Linux](https://www.microsoft.com/en-us/download/details.aspx?id=53339). |
| AOS       | The Microsoft .NET Framework version 2.0–3.5 (CLR 2.0) | `Add-WindowsFeature -Name NET-Framework-Features, NET-Framework-Core, NET-HTTP-Activation, NET-Non-HTTP-Activ` |
| AOS       | The Microsoft .NET Framework version 4.0–4.6 (CLR 4.0) | `Add-WindowsFeature -Name NET-Framework-45-Features, NET-Framework-45-Core, NET-Framework-45-ASPNET, NET-WCF-Services45, NET-WCF-TCP-PortSharing45` |
| AOS       | Internet Information Services (IIS) | `Add-WindowsFeature WAS, WAS-Process-Model, WAS-NET-Environment, WAS-Config-APIs`<br>`# Windows Process Activation Service`<br>`Add-WindowsFeature Web-Server, Web-WebServer, Web-Security, Web-Filtering, Web-App-Dev, Web-Net-Ext, Web-Mgmt-Tools, Web-Mgmt-Console` |
| AOS       | Microsoft SQL Server Management Studio 2016 | |
| AOS       | Microsoft Visual C++ Redistributable Packages for Microsoft Visual Studio 2013 | See [Microsoft Visual C++ Redistributable Packages for Visual Studio 2013](https://support.microsoft.com/en-us/help/3179560). |
| AOS       | Microsoft Access Database Engine 2010 Redistributable | See [Microsoft Access Database Engine 2010 Redistributable](https://www.microsoft.com/en-us/download/details.aspx?id=13255). |
| BI        | The .NET Framework version 2.0–3.5 (CLR 2.0) | `Add-WindowsFeature -Name NET-Framework-Features, NET-Framework-Core, NET-HTTP-Activation, NET-Non-HTTP-Activ` |
| BI        | The .NET Framework version 4.0–4.6 (CLR 4.0) | `Add-WindowsFeature -Name NET-Framework-45-Features, NET-Framework-45-Core, NET-Framework-45-ASPNET, NET-WCF-Services45, NET-WCF-TCP-PortSharing45` |
| BI        | SQL Server 2016 SP1 | |
| BI        | Management Studio 2016 | |
| BI        | SQL Server Reporting Services 2016 in **Native** mode | |
| MR        | The .NET Framework version 2.0–3.5 (CLR 2.0) | `Add-WindowsFeature -Name NET-Framework-Features, NET-Framework-Core, NET-HTTP-Activation, NET-Non-HTTP-Activ` |
| MR        | The .NET Framework version 4.0–4.6 (CLR 4.0) | `Add-WindowsFeature -Name NET-Framework-45-Features, NET-Framework-45-Core, NET-Framework-45-ASPNET, NET-WCF-Services45, NET-WCF-TCP-PortSharing45` |
| MR        | Visual C++ Redistributable Packages for Visual Studio 2013 | See [Microsoft Visual C++ Redistributable Packages for Visual Studio 2013](https://support.microsoft.com/en-us/help/3179560). |

### Download setup scripts from LCS

We have provided several scripts to help improve the setup experience. Follow these steps to download the setup scripts from LCS.

> [!IMPORTANT]
> The scripts must be downloaded and executed from a computer in the domain that Finance and Operations is installed on.

1. Sign in to [LCS](https://lcs.dynamics.com/v2).
2. On the dashboard, select the **Shared asset library** tile.
3. On the **Model** tab, in the grid, select the **Dynamics 365 for Operations - On-premises Deployment scripts** row.
4. Select **Versions**, and download the latest version of the zip file for the scripts.
5. Right-click the zip file, and select **Properties**. Then, in the dialog box, select the **Unblock** check box.
6. Unzip the files into a folder that is named **infra**.

### Describe your configuration

This section provides information about the scripts that you must run. The scripts are discussed in the order that you must run them in. To run the scripts, fill in the template file at $(downloadPath)\\ConfigTemplate.xml. The ConfigTemplate.xml file describes the Service Fabric clusters, the certificates that are used to configure them, and the accounts that must be granted access to the relevant certificates.

Only the domain accounts that are specified in the **ProtectTo** tag will be able to import the .pfx files for certificates. This limitation is a security enhancement that replaces the certificate passwords in plain text. In the example that is provided, the values are filled in for the example infrastructure that is described in this topic. The template file will be used in the next section.

#### Create gMSA and domain user accounts

1. Navigate to the machine that has the downloaded and unzipped infrastructure scripts in the **infra** folder.
2. Copy the **infra** folder to the domain controller machine.
3. Start Windows PowerShell in elevated mode, change the directory to the **infra** folder, and run the following commands.

    ```
    Import-Module Infrastructure\ D365FO-OP\D365FO-OP.psd1
    New-D365FOGMSAAccounts -ConfigurationFilePath .\ConfigTemplate.xml
    ```

3. Run the following script to verify that the accounts have been created. Replace the wildcard character in the **filter** command with the appropriate account names.

    ```
    #Use this command to validate the gMSA accounts are added to AD.
    #Ensure to replace the Filter parameter with the pattern used to create your accounts.
    Get-ADServiceAccount -Filter { samAccountName -like "*" }
    ```

4. If you must make changes to accounts or machines, update the ConfigTemplate.xml file, and then run the following script. Make sure that you copy any changes back to the original **infra** folder.

    ```
    Update-D365FOGMSAAccounts -ConfigurationFilePath .\ConfigTemplate.xml
    ```

### Install certificates

1. Navigate to the machine that has the **infra** folder.
2. If you must generate self-signed certificates, run the following command. The script will create the certificates, put them in the CurrentUser\My certificate store on the machine, and update the thumbprints in the XML file. 

    ```
    # Create self-signed certs 
    New-SelfSignedCertificates.ps1 -ConfigurationFilePath .\ConfigTemplate.xml
    ```

    If you must reuse any certificates and therefore don't have to generate certificates for them, set the **generateSelfSignedCert** tag to **false**.

3. If you're using SSL certificates, skip certificate generation, and update the ConfigTemplate.xml with the respective thumbprints.
4. Make sure that you enter the value against the **protectTo** tag. Only the Active Directory user or group that is specified in the **protectTo** tag will have permissions to import the certificates into the individual VMs.
5. Export the certificates into .pfx files.

    ```
    # Exports Pfx files into a directory VMs\<VMName>, all the certs will be written to infra\Certs folder.
    .\Export-PfxFiles.ps1 -ConfigurationFilePath .\ConfigTemplate.xml
    ```

    > [!IMPORTANT]
    > The certificates must have been installed and must be present in cert:\\CurrentUser\\My. The certificates must also be exportable.

6. Export the scripts that must be run on each VM.

    ```
    # Exports the script files to be execute on each VM into a directory VMs\<VMName>.
    .\Export-Scripts.ps1 -ConfigurationFilePath .\ConfigTemplate.xml
    ```

7. Download any Microsoft Windows Installers (MSIs) from the list of VM prerequisite software earlier in this topic into a file share that is accessible by all VMs.

    > [!NOTE] 
    > Add the Management Studio MSI to this folder if Management Studio isn't already installed on the VMs.

8. Copy the contents of each infra\VMs\<VMName> folder into the corresponding VM, and then run the following scripts.

    ```
    # Install pre-req software on the VMs.
    .\Configure-PreReqs.ps1 -MSIFilePath <path of the MSIs>
    ```

    > [!IMPORTANT] 
    > Restart the machine if you're prompt to restart it. Make sure that you rerun the .\Configure-PreReqs.ps1 script after restart. Continue to restart the machine and rerun the script until the script succeeds.

9. Run the following scripts to complete the VM setup.  

    ```
    .\Add-GMSAOnVM.ps1
    .\Import-PfxFiles.ps1
    .\Set-CertificateAcls.ps1
    ```

10. Run the following script to validate the VM setup.

    ```
    .\Test-D365FOConfiguration.ps1
    ```

### Set up a standalone Service Fabric cluster

1. Run the following command to generate the ClusterConfig.json file.

    ```
   .\New-SFClusterConfig.ps1 -ConfigurationFilePath .\ConfigTemplate.xml
    ```

    For more information, see, [Step 1B: Create a multi-machine cluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-for-windows-server#create-the-cluster), [Secure a standalone cluster on Windows using X.509 certificates](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security), and [Create a standalone cluster running on Windows Server](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-for-windows-server#create-the-cluster).

2. Download the [Service Fabric standalone installation package](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-for-windows-server#download-the-service-fabric-standalone-package) onto one of your Service Fabric nodes. After the zip file is downloaded, unblock it by right-clicking the zip file and then selecting **Properties**. In the dialog box, select the **Unblock** check box in the lower right.
3. Copy the zip file to one of the nodes in the Service Fabric cluster, and unzip it.
4. Copy your ClusterConfig.json file to your unzipped install location of standalone Service Fabric.
5. Navigate to the unzipped install location in Windows PowerShell by using elevated privileges. Then run the following command to test ClusterConfig.

    ```
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\clusterConfig.json
    ```

6. If the test is successful, run the following command to deploy the cluster.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

7. After the cluster is created, open the Service Fabric explorer on any client machine to validate the installation.

    1. Install the Service Fabric client certificate in CurrentUser\\My if it isn't already installed.
    2. Go to **IE settings** \> **Compatibility Mode**, and clear the **Display Intranet sites in compatibility mode** check box.
    3. Go to `https://sf.d365ffo.onprem.contoso.com:19080`, where sf.d365ffo.onprem.contoso.com is the host name of the Service Fabric cluster that is specified in the zone. If DNS name resolution isn't configured, use the IP address of the machine.
    4. Select the client certificate. The **Service Fabric explorer** page appears.

### Configure LCS connectivity for the tenant

Deployment and servicing of Finance and Operations are orchestrated through LCS by using an on-premises local agent. To establish connectivity from LCS to the Finance and Operations tenant, you must configure a certificate that enables the local agent to act on behalf on your Azure AD tenant (for example, Contoso.Onmicrosoft.com).

Use the on-premises agent certificate that you acquired from a CA or the self-signed certificate that you generated by using scripts.

Only user accounts that have the Global Administrator directory role can add certificates to authorize LCS. By default, the person who signs up for Microsoft Office 365 for your organization is the global administrator for the directory.

> [!NOTE]
> You must configure the certificate exactly one time per tenant. All on-premises environments can use the same certificate to connect with LCS.

1. Download and install the latest version of Azure PowerShell on a client machine. For more information, see [Install and configure Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0).
2. Sign in to the [customer's Azure portal](https://portal.azure.com) to verify that you have the Global Administrator directory role.
3. Run the following script from $(DownloadPath)\\InfrastructureScripts.

    ```
    .\AddCertToServicePrincipal.ps1 -CertificateThumbprint <OnPremLocalAgent Certificate Thumbprint>
    ```

### Set up file storage

You must set up the following two highly available SMB 3.0 file shares:

- A file share that stores user documents that are uploaded to AOS (for example, \\\\DAX7SQLAOFILE1\\aos-storage)
- A file share that stores the latest build and configuration files to orchestrate the deployment (for example, \\\\DAX7SQLAOFILE1\\agent))

    > [!NOTE]
    > Keep this file share path as short as possible to avoid exceeding the maximum path length on the files that will be put in the share.

For information about how to enable SMB 3.0, see [SMB Security Enhancements](https://technet.microsoft.com/en-us/library/dn551363(v=ws.11).aspx#BKMK_disablesmb1).

> [!NOTE]
> - Secure dialect negotiation can't detect or prevent downgrades from SMB 2.0 or 3.0 to SMB 1.0. Therefore, we strongly recommend that you disable the SMB 1.0 server. By disabling the SMB 1.0 server, you can take advantage of the full capabilities of SMB encryption.
> - To help guarantee that your data is protected while it's at rest in your environment, BitLocker Drive Encryption must be enabled on every machine. For information about how to enable BitLocker, see [BitLocker: How to deploy on Windows Server 2012 and later](https://docs.microsoft.com/en-us/windows/device-security/bitlocker/bitlocker-how-to-deploy-on-windows-server).

1. On the file share machine, run the following command.

    ```
    Install-WindowsFeature -Name FS-FileServer -IncludeAllSubFeature -IncludeManagementTools
    ```

2. Follow these steps to set up the \\\\DAX7SQLAOFILE1\\aos-storage file share:

    1. In Server Manager, select **File and Storage Services** \> **Shares**.
    2. Select **Tasks** \> **New Share** to create a new share. Name the share **aos-storage**.
    3. Grant **Modify** permissions for every machine in the Service Fabric cluster except OrchestratorNodeType.
    4. Grant **Modify** permissions for the user AOS domain user (contoso\\AXServiceUser) and the gMSA user (contoso\\svc-AXSF).

3. Follow these steps to set up the \\\\DAX7SQLAOFILE1\\agent file share:

    1. In Server Manager, select **File and Storage Services** \> **Shares**.
    2. Select **Tasks** \> **New Share** to create a new share. Name the share **agent**.
    3. Grant **Full-Control** permissions to the gMSA user for the local deployment agent (contoso\\svc-LocalAgent$).

### Set up SQL Server

1. Install SQL Server 2016 SP1 with high availability, either as SQL clusters that include a Storage Area Network (SAN) or in an Always-On configuration.  Verify that the Database Engine, SSRS, Full-Text Search, and Management Tools are already installed.

    > [!NOTE]
    > Make sure that the Always-On is set up as described in [Select Initial Data Synchronization Page (Always On Availability Group Wizards)](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards), and follow the instructions in [To Prepare Secondary Databases Manually](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards#PrepareSecondaryDbs).

2. Run the SQL service as a domain user.
3. Get an SSL certificate from a CA to configure Finance and Operations. For testing purposes, you can create and use a self-signed certificate.

    **Self-signed certificate for a Clustered SQL instance**

    ```
    New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -DnsName "DAX7SQLAOSQLA.contososqlao.com" -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" -Subject "DAX7SQLAOSQLA.contososqlao.com"
    ```

    **Self-signed certificate for an Always-On SQL instance**

    ```
    #https://www.derekseaman.com/2014/11/sql-2014-alwayson-ag-pt-13-ssl.html

    # Create certificate for each SQL Node (i.e. 2 nodes = 2 certificates)
    # Run script on each node
    $computerName = $env:COMPUTERNAME.ToLower() 
    $domain = $env:USERDNSDOMAIN.ToLower()
    $listenerName = 'dax7sqlaosqla' 
    $cert = New-SelfSignedCertificate -Subject "$computerName.$domain" -DnsName "$listenerName.$domain", $listenerName, $computerName -Provider 'Microsoft Enhanced RSA and AES Cryptographic Provider'
    ```

4. Use the certificate to configure SSL on SQL Server. Follow the steps in [How to enable SSL encryption for an instance of SQL Server by using Microsoft Management Console](https://support.microsoft.com/en-us/help/316898/how-to-enable-ssl-encryption-for-an-instance-of-sql-server-by-using-microsoft-management-console).
5. For each node of the cluster, follow these steps. Make sure that you make the changes on the non-active node, and that you fail over to it after changes are made.

    1. Import the certificate into LocalMachine\\My.
    2. Grant certificate permissions to the service account that is used to run the SQL service. In Microsoft Management Console (MMC), right-click the certificate (**certlm.msc**), and then select **Tasks** \> **Manage Private Keys**.
    3. Add the certificate thumbprint to HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\*MSSQL.x*\\MSSQLServer\\SuperSocketNetLib\\Certificate.
    4. In Microsoft SQL Server Configuration Manager, set **ForceEncryption** to **Yes**.

6. Export the public key of the certificate (the .cer file), and install it in the trusted root of each Service Fabric node.

### Configure the databases

#### Configure the OrchestratorData database

1. Create an empty database, and name it **OrchestratorData**. This database is used by the on-premises local agent to orchestrate deployments.
2. Grant the local agent gMSA (svc-LocalAgent$) **db\_owner** permissions on the database:

    1. In the tree, expand the server name, expand **Security** \> **Logins**, right-click, and then select **New Login**.

        ![New Login command](./media/OPSetup_01_NewLogin.png)

    2. Look up the **svc-LocalAgent$** service account. Select **Locations**, select **Entire Directory**, select **Object Types**, and then select **Service Account**.

        ![Select User, Service Account, or Group dialog box](./media/OPSetup_02_SelectUserServiceAccountOrGroupDialogBox.png)

    3. Set **Assign ServerRole** to **Public**.
    4. Assign the **db\_owner** database role permission to the OrchestratorData database.

#### Configure the Finance and Operations database

1. Sign in to [LCS](https://lcs.dynamics.com/v2).
2. On the dashboard, select the **Shared asset library** tile.
3. On the **Model** tab, in the grid, select the **Dynamics 365 for Operations on-premises, Enterprise edition - Demo data** row to download the zip file.
4. The zip file contains empty and demo data .bak files. Select .bak file, based on your requirements. For example, if you require demo data, restore the AxBootstrapDB_Demodata.bak file. 
5. Restore the AxBootstrapDB_DemoData.bak database.
6. Rename the database **AXDBRAIN**.
7. Run the following queries on the restored database.

    ```
    ALTER DATABASE AXDBRAIN
    SET READ_COMMITTED_SNAPSHOT ON
    GO
    
    ALTER DATABASE AXDBRAIN
    SET ALLOW_SNAPSHOT_ISOLATION ON
    GO
    ```

8. Update the database files:

    1. Right-click the database, and then select **Properties**.
    2. Select **Files** to change the database file properties.
    3. In the **Initial Size (MB)** field, enter a value that is more than 5,120.

        ![Database Properties dialog box](./media/OPSetup_03_DatabasePropertiesDialogBox.png)

    4. For **AXDBBuild\_Data**, set the **Autogrowth/Maximize** field to **200** megabytes (MB).
    5. For **AXDBBuild\_Log**, set the **Autogrowth/Maximize** field to **500** MB.

        ![Change Autogrowth dialog box](./media/OPSetup_04_ChangeAutogrowthDialogBox.png)

9. Create a new user that has SQL authentication enabled (axdbadmin).
10. Use the information in the following table to map the users and database roles.

    | User            | Type    | Database role |
    |-----------------|---------|---------------|
    | svc-AXSF$       | gMSA    | db\_owner     |
    | svc-LocalAgent$ | gMSA    | db\_owner     |
    | svc-FRPS$       | gMSA    | db\_owner     |
    | svc-FRAS$       | gMSA    | db\_owner     |
    | axdbadmin       | SqlUser | db\_owner     |

11. Give the svc-AXSF$ user and the axdbadmin SQL user access to the following roles in the tempdb database:

    - db\_datareader
    - db\_datawriter
    - db\_ddladmin

12. In Management Studio, run the following Transact SQL (T-SQL) commands.

    ```
    GRANT VIEW SERVER STATE TO axdbadmin
    GRANT VIEW SERVER STATE TO [contososqlao\svc-AXSF$]
    ```

13. Run the following command.

    ```
    .\Reset-DatabaseUsers.ps1 -DatabaseServer ‘<FQDN of the SQL server>’ -DatabaseName '<AX database name>'
    ```

#### Configure the Financial Reporting database

1. Create an empty database, and name it **FinancialReporting**.
2. Use the information in the following table to map the users and database roles.

    | User            | Type | Database role |
    |-----------------|------|---------------|
    | svc-LocalAgent$ | gMSA | db\_owner     |
    | svc-FRPS$       | gMSA | db\_owner     |
    | svc-FRAS$       | gMSA | db\_owner     |

### Encrypt credentials

1. On any client machine, install the encipherment certificate in the LocalMachine\\My certificate store.
2. Grant the current user read access to the private key of this certificate.
3. Create the Credentials.json file, as shown here.

    ```
    {
        "AosPrincipal": {
            "AccountPassword": "<encryptedDomainUserPassword>"
        },
        "AosSqlAuth": {
            "SqlUser": "<encryptedSqlUser>",
            "SqlPwd": "<encryptedSqlPassword>"
        }
    }
    ```

    - **AccountPassword** is the encrypted domain user password for the AOS domain user (contoso\\axserviceuser).
    - **SqlUser** is the encrypted SQL user (axdbadmin) that has access to the Finance and Operations database (AXDBRAIN), and **SqlPassword** is the encrypted SQL password.

4. Copy the .json file to the SMB file share, \\\\AX7SQLAOFILE1\\agent\\Credentials\\Credentials.json.
5. Update the Credentials.json file with encrypted values.

    ```
    # Service fabric API to encrypt text and copy it to the clipboard.
    Invoke-ServiceFabricEncryptText -Text '<textToEncrypt>' -CertThumbprint '<DataEncipherment Thumbprint>' -CertStore -StoreLocation LocalMachine -StoreName My | clip
    ```

### Set up SSRS

1. Before you begin, make sure that the prerequisites that are listed at the beginning of this topic are installed.
2. Follow the steps in [Configure SQL Server Reporting Services for an on-premises deployment](../analytics/configure-ssrs-on-premises.md).

### Configure AD FS

Before you can complete this procedure, AD FS must be deployed on Windows Server 2016. For information about how to deploy AD FS, see [Deployment Guide Windows Server 2016 and 2012 R2 AD FS Deployment Guide](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide).

Finance and Operations requires additional configuration beyond the default out-of-box configuration of AD FS. For the following steps, Windows PowerShell runs on a machine where the AD FS role service is installed. The user account must have enough permissions to administer AD FS. For example, the user must have a domain administrator account.

1. Configure the AD FS identifier so that it matches the AD FS token issuer.

    ```
    $adfsProperties = Get-AdfsProperties
    Set-AdfsProperties -Identifier $adfsProperties.IdTokenIssuer
    ```

2. You should disable Windows Integrated Authentication (WIA) for intranet authentication connections, unless you've configured AD FS for mixed environments. For more information about how to configure WIA so that it can be used with AD FS, see [Configure browsers to use Windows Integrated Authentication (WIA) with AD FS](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-ad-fs-browser-wia).

    ```
    Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider FormsAuthentication, MicrosoftPassportAuthentication
    ```

3. For sign-in, the user's email address must be an acceptable authentication input.

    ```
    Add-Type -AssemblyName System.Net
    $fdqn = ([System.Net.Dns]::GetHostEntry('localhost').HostName).ToLower()
    $domainName = $fdqn.Substring($fdqn.IndexOf('.')+1)
    Set-AdfsClaimsProviderTrust -TargetIdentifier 'AD AUTHORITY' -AlternateLoginID mail -LookupForests $domainName
    ```

In order for AD FS to trust Finance and Operations for the exchange of authentication, various application entries must be registered in AD FS under an AD FS application group. To speed up setup process and help reduce errors, you can use the following script for registration. Copy the Publish-ADFSApplicationGroup.ps1 script and D365FO-OP directory to a machine where the AD FS role service is installed. Then run the script by using a user account that has enough permissions to administer AD FS. (For example, use an administrator account.)

For more information about how to use the script, see the documentation that is listed in the script. Make a note of the client IDs that are specified in the output, because you will be asked for this information in LCS in a later step.

```
# Host URL is your DNS record\host name for accessing the AOS
.\Publish-ADFSApplicationGroup.ps1 -HostUrl 'https://ax.d365ffo.onprem.contoso.com'
```

The AD FS management console should resemble the following illustration. To open Server Manager, select **Tools** \> **AD FS Management**, and then, at the bottom of the tree view on the left, select **Application Groups**. Double-click the application group to view more details.

![Application group properties](./media/OPSetup_05_ApplicatioGroupProperties.png)

Finally, make sure that you can access the AD FS OpenID Configuration URL on a Service Fabric node of the **AOSNodeType** type. To perform this sanity check, try to open `https://<adfs-dns-name>/adfs/.well-known/openid-configuration` in a web browser. If you receive a message that states that the site isn't secure, you haven't added your AD FS SSL certificate to the Trusted Root Certification Authorities store. This step is described in the AD FS deployment guide. If you successfully access the URL, a JavaScript Object Notation (JSON) file is returned that contains your AD FS configuration, and you will see that your AD FS URL is trusted.

You've now complete the setup of the infrastructure. The following sections describe how to navigate to [LCS](https://lcs.dynamics.com) to set up your connector and deploy your Finance and Operations environment.

## Configure a connector and install an on-premises local agent

1. Sign in to [LCS](https://lcs.dynamics.com/), and open the on-premises implementation project.
2. On the hamburger menu, select **Project settings**.

    ![Project settings command](./media/OPSetup_06_ProjectSettings.png)

3. Select **On-premises connectors**.
4. Select **Add** to create a new connector.
5. On the **Setup host infrastructure** tab, download the agent installer.

    ![Download agent installer button on the Setup host infrastructure tab](./media/OPSetup_07_DownloadAgentInstaller.png)

6. Verify that the zip file is unblocked. Right-click the file, and then select **Properties**. In the dialog box, select **Unblock**.
7. Unzip the agent installer on one of the Service Fabric nodes of the **OrchestratorType** type.
8. On the **Configure agent** tab, enter the configuration settings.
9. Save the configuration, and then select **Download configurations** to download the localagent-config.json configuration file.

    ![Download configurations button on the Configure agent tab](./media/OPSetup_08_DownloadConfigurations.png)

10. Copy the localagent-config.json file to the machine where the agent installer package is located.
11. In a **Command Prompt** window, run the following command by navigating to the folder that contains the agent installer.

    ```
    LocalAgentCLI.exe Install <path of config.json>
    ```

    > [!NOTE]
    > The user who runs this command must have **db\_owner** permissions on the OrchestratorData database.

12. After the local agent is successfully installed, navigate back to your on-premises connector in LCS.
13. On the **Validate setup** tab, select **Message agent** to test for LCS connectivity to your local agent. When a connection is successfully established, the page will resemble the following illustration.

    ![Validate the agent](./media/ValidateAgent.PNG)

## Deploy your Finance and Operations (on-premises) environment

1. In LCS, navigate to your on-premises project, go to **Environment** \> **Sandbox**, and then select **Configure**.
2. Select your environment topology, and then complete the wizard to initiate your deployment.
3. The local agent will pick up the deployment request, start the deployment, and communicate back to LCS when the environment is ready.
