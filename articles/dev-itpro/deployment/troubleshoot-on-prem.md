---
# required metadata

title: Troubleshoot Dynamics 365 for Finance and Operations, Enterprise edition on-premises 
description: This topic provides troubleshooting information for on-premises deployments of Dynamics 365 for Finance and Operations, Enterprise edition. 
author: sarvanisathish
manager: AnnBe
ms.date: 06/13/2017
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
ms.author: sarvanis
ms.search.validFrom: 2016-02-28
ms.dyn365.ops.version: Platform Update 8

---
# Troubleshoot Dynamics 365 for Finance and Operations, Enterprise edition on-premises

[!include[banner](../includes/banner.md)]


## Service Fabric 
Service Fabric is one of the initial components to install and configure for your on-premises deployment. Service Fabric is used by the Orchestrator, AOS, SSRS and MR nodes.

### Access Service Fabric Explorer
Service Fabric Explorer can be accessed by using a browser and the default address, https://sf.d365ffo.onprem.contoso.com:19080. To verify the address, note what was used in the topic section, [Create DNS zones and add A records](https://docs.microsoft.com/en-us/dynamics365/unified-operations/dev-itpro/deployment/setup-deploy-on-premises-environments#createdns). 
To access the site, the client certificate needs to be located in cert:\CurrentUser\My (Certificates - Current User > Personal > Certificates) of the machine that is accessing the site. When you access the site, select the client certificate when prompted.

### Stateful and Stateless Services, how to identify nodes for log review 
To find out what machine is the primar instance for Stateful Services, like a Local Agent, go to Service Fabric Explorer, expand **Cluster** > **Applications** > **(intended application ex) LocalAgentType** > **fabric:/LocalAgent** > **Fabric:/LocalAgent/ArtifactsManager** > **(guid)**. Note the primary node. 
For Stateless Services, or the rest of the applications, you need to check all of the nodes. 

### Timeout error received when creating a Service Fabric cluster 
Run the Test-D365FOConfiguration.ps1 as noted in [Set up a standalone Service Fabric cluster](../setup-deploy-on-premises-environments#setupsfcluster.md) and note any errors. 
Verify that the Service fabric Server certificate, client certificate exists in the LocalMachine store on ALL service fabric nodes. 
Verify that the Service fabric Server certificate has the ACL for Network Service on ALL service fabric nodes.

### Time out while waiting for Installer Service to complete for machine x.x.x.x
You can only have one node type for each IP address (machine). Check to see if the nodes are being reused on same machine, for example AOS and ORCH on same machine, and that the ConfigTemplate.xml is correctly defined. 

### Remove a specific application 
We recommend that you use LCS to remove or cleanup deployments. However, if additional steps are needed, you can use Service Fabric Explorer to remove an application.  
In Service Fabric Explorer, go to **Application node** > **Applications** > **MonitoringAgentAppType-Agent**. Click the ellipses by **fabric:/Agent-Monitoring** and delete the application. Enter the full name of the application to confirm. 
You can also remove the **MonitoringAgentAppType-Agent** by clicking the ellipses and then **Unprovision Type**. Enter the full name to confirm. 

### Remove Service Fabric completely 
To remove the Service Fabric completely remove ServiceFabricCluster.ps1 -ClusterConfigFilePath "<path to working directory>\ClusterConfig.json". If this results in error, remove a specific node on that cluster from the
powershell.exe -File "C:\Program Files\Microsoft Service Fabric\bin\fabric\fabric.code\CleanFabric.ps1" 

Next, remove c:\ProgramData\SF folder, if using the default. Otherwise, remove the specified folder. 
If you receive an access denied error, restart PowerShell or restart the machine. 

## LocalAgent 
LocalAgent is the framework that is responsible for communicating with LCS, downloading components to be installed, installation, and maintaining and removing Dynamics. 

### How to find the LocalAgent values that are used 
Local Agent values can be found in Service Fabric Explorer under **Cluster** > **Applications** > **LocalAgentType** > **fabric:/LocalAgent, Details** section.

### Install, upgrade, or uninstall Local agent 
Local agent installation is discussed in the topic, [Set up and deploy on-premises environments](../setup-deploy-on-premises-environments.md). You can also use the follwoing upgrade and uninstall commands: 

    LocalAgentCLI.exe Install <path of localagent-config.json> 
    LocalAgentCLI.exe Upgrade <path of localagent-config.json> 
    LocalAgentCLI.exe Cleanup <path of localagent-config.json>
    
> [!NOTE]
    > The cleanup command doesn't remove any of the files that were placed in the file share. The file share can be reused.

### Error occurs when starting up Local agent services  
If you receive the error, "Could not load file or assembly 'Lcs.DeploymentAgent.Proxy.Contract, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies", this means that the **Strong name** verification should no longer be enabled. This is done by using Configure-PreReqs.ps1. To validate that the verification is no longer enabled, run Test-D365FOConfiguration.ps1.

### "Validation in progress" In LCS, "Validation in progress" is showing for a period of several minutes
Complete the following steps to troubleshoot general issues with local agent validation.
1. Run the Configure-PreReqs.ps1 on all the orchestrator machines to configure the machines correctly. 
2. Verify that the script Test-D365FOConfiguration.ps1 passes on all of the orchestrator machines.  
3. Verify that the installation of LocalAgentCLI.exe completed successfully. 
4. Go to Service Fabric Explorer and verify that the applications are all healthy. 
5. If the applications are not healthy, locate the primary node for the failing service. 
   Look for events in:
   
   - **Event Viewer > **Custom Views** > **Administrative Events** 
   - **Event Viewer** > **Applications and Services Log** > **Microsoft** > **Dynamics** > **AX-LocalAgent 

**Common errors**
Receiving 'Unable to process commands' and/or 'Unable to get the channel information' messages 
 
"RunAsync failed due to an unhandled exception causing the host process to crash: System.ArgumentNullException: Value cannot be null. Parameter name: certificate 

**Reason**
The certificate specified for OnPremLocalAgent certificate is invalid or not configured correctly for the tenant. 

**Steps** 
1. Run Test-D365FOConfiguration.ps1 on all Orchestrator nodes to ensure all checks pass 
2. Verify that the certificate specified  in local agent configuration is correct.  
Make sure there are no special characters while specifying the thumbprint in LCS and the ConfigTemplate.xml 
The certificate should be the same as what is specified in infrastructure\ConfigTemplate.xml for  
    <Certificate type="Orchestrator" exportable="true" generateSelfSignedCert="true"> 
      <Name>OnPremLocalAgent</Name> 
      <Thumbprint></Thumbprint> 
      <ProtectTo></ProtectTo> 
    </Certificate> 
3. Ensure that the steps in Configure LCS connectivity for the tenant section were completed using the same certificate specified in local agent configuration in LCS. 
4. Uninstall the local agent  
5. Specify the correct certificate in local agent configuration and download the configuration file again 
6. Install local agent again with the new configuration file. 

**Error event:** 
Access to the path '\\...\agent\assets\StandAloneSetup-76308-1.zip' is denied 

**Reason**
The file share specified in local agent configuration is invalid. 

**Steps** 
1. Ensure that the share specified exists 
2. Ensure that the local agent user has Full permission on the specified share. The local agent user is the DNS name specified in ConfigTemplate.xml in the section, 
  <ADServiceAccount type="gMSA" name="svc-LocalAgent$" refName="gmsaLocalAgent"> 
      <DNSHostName>svc-LocalAgent.d365ffo.onprem.contoso.com</DNSHostName> 
  </ADServiceAccount> 
3. Ensure that the Setup file share section in setup document is completed. 
4. Uninstall the local agent  
5. Specify the correct file share in local agent configuration and download the configuration file again 
6. Install local agent again with the new configuration file. 

**Error event:** 
Login failed for user 'D365\svc-LocalAgent$'. Reason: Could not find a login matching the name provided. [CLIENT: 10.0.2.23]  

**Reason**
The local agent user is unable to connect to the orchestration database. Possibly users may have been deleted and recreated in the AD. The SID of hte user has changed. Any access given to the user for the SQL Server or database will no longer work. 

**Steps**
1. Run the script on the SQL server 
   .\Initialize-Database.ps1 -ConfigurationFilePath .\ConfigTemplate.xml -ComponentName Orchestrator 
  This creates an empty orchestrator database if it does not exist and adds the local agent user to the database with db_owner permission 
2. The application should automatically go to healthy state once the right permissions are provided. 
3. If any of the settings such as the SQL FQDN, Database name and local agent user was provided incorrectly in LCS, change the settings and reinstall local agent. 
4. If step 1,2 does not solve the issue, manually remove the local agent user from the SQL server and the database and re-run the Initialize-Database script. 
5. If recreating users in AD, SID changing. Need to remove old user/SID and add new user/SID 

**Additional scenario**
Local agent user is unable to connect to the SQL server or database. 

**Steps** 
1. Deleted the svc-LocalAgent user from the sql server primary node databases and then removed the login from both servers.  
2. Run the following scripts in  
    .\Initialize-Database.ps1 -ConfigurationFilePath .\ConfigTemplate.xml -ComponentName Orchestrator 
    .\Configure-Database.ps1 -ConfigurationFilePath .\ConfigTemplate.xml -ComponentName Orchestrator 
  These scripts does not work with always-on setup. The database needs to created first in the primary node and then replicated.  

**Error event:** 
RunAsync failed due to an unhandled exception causing the host process to crash: System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The remote name could not be resolved: 'lcsapi.lcs.dynamics.com' 

**Reason** 
The local agent machines are unable to connect to lcsapi.lcs.dynamics.com. Review AX-BridgeService eventlog for possible "The remote name could not be resolved: 'lcsapi.lcs.dynamics.com'".  

**Steps** 
1. Run [psping lcsapi.lcs.dynamics.com:80].  
2. If unable to receive reply then contact the IT department at your organization as the firewall is blocking access to lcsapi or proxy issues 
    Lcsapi.lcs.dynamics.com 
    <lcs azure blob storage domain> 
    <lcs azure queue domain> 

**Error event:**
Access to the path '\\...\agent\assets\StandAloneSetup-76308-1.zip' is denied 

**Reason** 
The local agent user is unable to access the file share specified in local agent configuration 

**Steps**  
1. Make sure that the file share is valid and it exists. 
2. Download PsExec https://docs.microsoft.com/en-us/sysinternals/downloads/psexec 
3. Open command prompt and  run PsExec.exe -u contoso\svc-LocalAgent$ cmd.exe 
4. Access the file share in the resulting window. Make sure that this completes without errors 
5. Add the local agent user to the file share and give full permissions 

Check for svc-LocalAgent$ rights to path (NTFS and share permissions) 

### "Unable to load DLL 'FabricClient.dll':" error 
Close PowerShell and reopen. If still receive error, restart machine. 

### What Cluster ID in Agent config should be used 
Cluster ID can be any GUID. It is for tracking purposes 

### After changing the tenant for the project from LCS, Local Agent stops working 
Configure Local Agent with updated tenant.  
1. Remove all the environment(s) already installed with connector by deleting from LCS 
2. Uninstall the local agent  
   .\LocalAgentCLI.exe Cleanup <path of localagent-config.json> 
3. Go through 11. Configure LCS connectivity for the tenant step in documentation again 
4. Create a new LCS connector in the new tenant 
5. Download the local-agent.config file 
6. Install local agent 
   .\LocalAgentCLI.exe Install <path of localagent-config.json>

## Log files to monitor deployment

### How to monitor deployment locally 
Review the Event Viewer logs for the primary orchestrator machine and artifacts manager, bridge service 
AX-LocalAgent (overall installation process) 

- Modules  
    - Common 
    - Reportingservices 
    - AOS 
    - Financialreporting 
- Command 
    - Setup 
    - Dvt - Deployment verification test 
        
AX-SetupModuleEvents (additional details for Module details) 
AX-SetupInfrastructureEvents (additional details when interactions with Service Fabric) 

### AOS machines
**Event Viewer** > **Applications and Services Logs** > **Microsoft** > **Dynamics** > **AX-DatabaseSynchronize** 
**Event Viewer** > **Custom Views** > **Administrative Events** 

### How to find full error
Click on the details tab to get full message 

### Service Fabric
Note service that is failing, go to that application directory ex C:\ProgramData\SF\ORCH1\Fabric\work\Applications\LocalAgentType_App5\log 

## Example errors

### Encryption error occurred
Ex. AXBootStrapperAppType, Bootstrapper, AXDiagnostics, RTGatewayAppType, Gateway potential failure related, Microsoft.D365.Gateways.ClusterGateway.exe 

Data encipherment cert used to encrypt AOS AccountPassword was not installed on the box. This cert would be in the Certificates (Local Computer) or Provider type is wrong. 

Validate credentials.json file. Check if the texts there decrypts correctly by typing the message (on AOS1) 

- Invoke-ServiceFabricDecryptText -CipherText 'longstring' -StoreLocation LocalMachine | clip 


Parameter '' is not defined in the ApplicationManifest file 

- Check for proper layout/structure of credentails.json file Encrypt credentials  
- Check to see if end quote is at end of line or on next line 

Note Event Viewer > Custom Views > Administrative Events 
    Error on Microsoft-Service Fabric source 
    
### Can't find the certificate and private key to use for decryption (0x8009200C) 
Missing cert and/or ACL or wrong thumbprint entry (ex axdataenciphermentcert and AXSF$ AXServiceUser) 

Check for special characters 

- Also may be able to see in C:\ProgramData\SF\AOS1\Fabric\work\Applications\AXBootstrapperAppType_App9\log\ConfigureCertificates….txt 
Note any ? For thumprints  

Test via Invoke-ServiceFabricDecryptText -CipherText 'longstring' -StoreLocation LocalMachine | clip 

- If receive "Cannot find the certificate and private key to use for decryption" then verify  axdataenciphermentcert and svc-AXSF$ AXServiceUser ACL 

If changed credentials.json then delete and redeploy from LCS 

### MR
Additional logging can be done by registering providers. Attached ETWManifest.zip can be downloaded to primary orchestrator machine and following commands ran 

- Copy ETWManifest to primary orch machine and run following  
    .\RegisterETW.ps1 -ManifestsAndDll @{"C:\Files\ETWManifest\Microsoft.Dynamics.Reporting.Instrumentation.man" =    "C:\Files\ETWManifest\Microsoft.Dynamics.Reporting.Instrumentation.dll"} 
- Note if needed to unregister, use following command 
    .\RegisterETW.ps1 -ManifestsAndDll @{"C:\Files\ETWManifest\Microsoft.Dynamics.Reporting.Instrumentation.man" = "C:\Files\ETWManifest\Microsoft.Dynamics.Reporting.Instrumentation.dll"} -Unregister  

After providers are registered, additional details will be logged on new deployment that can be reviewed in Event Viewer > Applications and Services Logs > Microsoft > Dynamics 

- MR-Logger 
- MR-Sql 
    
Error encountered while executing AddAXDatabaseChangeTracking at Microsoft.Dynamics.Performance.Deployment.FinancialReportingDeployer.Utility.InvokeCmdletAndValidateSuccess(DeploymentCmdlet cmdlet) at  
- Make sure full path is correct, ex ax.d365ffo.onprem.contoso.com 

Issue could also be with star/* cert 

- Example, The remote certificate CN=*.d365ffo.onprem.contoso.com has and invalid name or does not match the host ax.d365ffo.onprem.contoso.com  

The remote name could not be resolved: 'x.d365fo.onprem.contoso.com' / There was no endpoint listening at https://x.d365fo.onprem.contoso.com/namespaces/AXSF/services/MetadataService that could accept the message. This is often caused by an incorrect address or SOAP action. See InnerException, if present, for more details. 

- Verify address is reachable by manually browsing in browser 
    
Run initialize database script and validate DBs have correct users 

If only receive AddAXDatabaseChangeTracking event, try to reach the Metadataservice of AX: 
https://ax.d365ffo.contoso.com/namespaces/AXSF/services/MetadataService 
Also check Certificates of the service in file wif.config. You find this file by log on to one of the AOS boxes 
use task manager (ex details tab), locate AxService.exe, right-click and select open file location. In wif.config file, should see 3 thumbprints. Note that: 

- They have to be different. 
- They need to be in this order 
    1. Finacialreporting Thumbprint 
    2. ReportingService Thumbprint 
    3. SessionAuthentication Thumbprint 
  If invalid then need to redeploy from LCS with proper Thumbprints. 

### Unable to resolve the xPath value 
Unable to resolve the xPath value of [TopologyInstance/CustomizationGroup[@name='ServiceConfiguration']/Group[@name='AOSServicePrincipalUser']/Customizations/Customization[@fieldName='PrincipalUserAccountPassword']/@selectedValue. 

This is expected behavior and is not an issue. is looking for AOS runtime user information. Since using integrated security, do not need that info. Outputting unresolvable xpaths to be verbose just in case failure needs to be investigated.  

### ADFS
Login page not redirecting and still prompting for credentials. Example was going to URL and getting "An error occurred. Contact your administrator for more information." 

- Add ADFS link to trusted sites 
- Add D365 link to trusted sites as well as likely will be next issue 
- Try adding trailing slash '/' to see if behavior changes.  
Verify AD FS Manager > AD FS > Application Groups. Double click on Microsoft Dynamics 365 for Operations On-premises. Under Native application, double click on Microsoft Dynamics 365 for Operations On-premises - Native application 

- Note Redirect URI. This should match what DNS forward lookup zone is for AX 

Could not establish trust relationship for the SSL/TLS secure channel 

- Look in Service Fabric > Cluster > Applications > AXSFType > fabric:/AXSF > details tab and Aad_AADMetadataLocationFormat /     - Aad_FederationMetadataLocation. Browse to that URL from AOS.  
- Look in AOS Event Viewer > Applications and Services Logs > Microsoft > Dynamics > AX-SystemRuntime for details 
- Check to see if ADFS cert is trusted 
    - Verify ADFS cert by going to ADFS machine > Server Manager > Tools > AD FS Management 
    - Expand AD FS > Service > Certificates and note certs (ex. dc1.contoso.com) 
    - On AOS machine go to cert manager > Certificates (Local Computer) > Trusted Root Certification Authorities > Certificates  and verify that the ADFS cert is listed (ex. dc1.contoso.com) 

If you receive a message that states that the site isn't secure, you haven't added your AD FS SSL certificate to the Trusted Root Certification Authorities store 

Unable to connect to the remote server 

- at System.Net.HttpWebRequest.GetResponse() 
- at System.Xml.XmlDownloadManager.GetNonFileStream(Uri uri, ICredentials credentials, IWebProxy proxy, RequestCachePolicy cachePolicy) 
- at System.Xml.XmlUrlResolver.GetEntity(Uri absoluteUri, String role, Type ofObjectToReturn) go to C:\ProgramData\SF\AOS_1\Fabric\work\Applications\AXSFType_App35\log folder you get the err and out files. 

The out file contains the info: 

- System.Net.WebException: Unable to connect to the remote server ---> System.Net.Sockets.SocketException: A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond x.x.x.x:443 
- Psping, IP and see if reachable  

Redirect login questions/issues 

- Verify in Service Fabric Explorer that Provisioning_AdminPrincipalName and Provisioning_AdminIdentityProvider is valid. If not then cannot proceed and will need to redeploy from LCS. 

    - Following example will reference https://DC1.contoso.com/adfs	and AXServiceUser@contoso.com 
    - Provisioning_AdminIdentityProvider would be https://DC1.contoso.com/adfs 
    - Provisioning_AdminPrincipalName would be AXServiceUser@contoso.com 

- If used Reset-DatabaseUsers.ps1 note need to restart AX Service to take effect 
- If still not working, then  
    - Note USERINFO table, specifically NETWORKDOMAIN, NETWORKALIAS 
    - NETWORKDOMAIN example https://DC1.contoso.com/adfs 
    - NETWORKALIAS example AXServiceUser@contoso.com 
    - In ADFS machine > Server Manager > Tools > AD FS Management > Service, right click on Service and Edit Federation Service Properties 
    - Note Federation Service identifier. This should match to USERINFO.NETWORKDOMAIN (https://DC1.contoso.com/adfs) 
    - Make sure both are https (ex. not http in ADFS but https in USERINFO) 

In ADFS machine go to **Event Viewer** > **Applications and Services Logs** > **AD FS** > **Admin note any errors**.

### Login issues / Reset user 
Verify in Service Fabric Explorer that Provisioning_AdminPrincipalName and Provisioning_AdminIdentityProvider is valid. If so then on    
           - SQL primary box run .\Reset-DatabaseUsers.ps1 
On each AOS, open Task Manager and end task on AXService.exe 
Can verify user has been set via running following select in SQL AXDB  

            - select SID, NETWORKDOMAIN, NETWORKALIAS, * from AXDB.dbo.USERINFO where id = 'admin' 
            
Note on SIDs 

- SID in AAD environment (online) is a hash of network alias, network domain 
- SID in AD environment (on-premises) is a hash of network alias, identify provider 

If still can't log in due to "You are not authorized to login with your current credentials. You will be redirected to the login page in a few seconds" then see ADFS section 

### System.Data.SqlClient.SqlException (0x80131904) and System.ComponentModel.Win32Exception (0x80004005)

System.Data.SqlClient.SqlException (0x80131904): A connection was successfully established with the server, but then an error occurred during the login process. (provider: SSL Provider, error: 0 - The certificate chain was issued by an authority that is not trusted.)  

System.ComponentModel.Win32Exception (0x80004005): The certificate chain was issued by an authority that is not trusted 


The certificates have not been installed or given access to the correct users 
Add the public key SQL server certificate on all the service fabric nodes 

### Keyset does not exist 
Didn’t run scripts on all machines as per instructions. Go back and review Install Certificates section. Copy the scripts in each folder to the VMs that correspond to the folder name. 

Also check csv file for correct domain being used 

### RunAsync failed due to an unhandled FabricException causing replica to fault
RunAsync failed due to an unhandled FabricException causing replica to fault: System.Fabric.FabricException: The first Fabric upgrade must specify both the code and config versions. Requested value: 0.0.0.0: 

Change ClusterConfig.json diagnosticsStore from network share to local path, ex \\server\path to default of c:\\ProgramData\\SF\\DiagnosticsStore 

### Dbsync (DB sync) issues 
Look at AOS working directory, ex C:\ProgramData\SF\AOS1\Fabric\work\Applications\AXSFType_App8\log 
Also review on all AOS machines  

- Event Viewer > Custom Views > Administrative Events 
- Event Viewer > Applications and Services Logs > Microsoft > Dynamics > AX-DatabaseSynchronize 

### Invalid algorithm specified / Cryptography  
‘Invalid algorithm specified’ error. Need to use Provider of Microsoft Enhanced RSA and AES Cryptographic Provider. Review requirements of certificates  
Also verify Credentials.json file structure is correct 

### Partition is below target replica or instance count 
Not a root error. Means status of each node is not ready. For AXSF/AOS status could still be inBuild 
Proceed to go to Event Viewer to get root error as well as c:\ProgramData\SF… 

### "Unable to find certificate" error when running Test-D365FOConfiguration.ps1 
Check to see if combining certs/thumbprints for multiple purposes. Example if combining client and SessionAuthentication will receive this error. Advised not to combine certificates. See certificate requirements 
Check acl.csv for domain.com\user vs. domain\user (ex. NETBIOS structure) 

### The client and server cannot communicate, because they do not possess a common algorithm 
Verify star certificate requirements, example provider being used 
 
### How to find gMSA / group managed service accounts 
Way to list all groups and/or hosts, [Get-ADServiceAccount -Identity svc-LocalAgent$ -Properties *] 
PrincipalsAllowedToRetrieveManagedPassword 

### AddCertToServicePrincipal script failing on Import-Module 
AddCertToServicePrincipal script failing on Import-Module : Could not load file or assembly 'Commands.Common.Graph.RBAC, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies. Strong name validation failed. (Exception from HRESULT: 0x8013141A) may have multiple versions of the same module installed.  

Run the following command in PowerShell 
        
        Uninstall-Module -Name AzureRM  
        Install-Module AzureRM 
Close the PowerShell window and try again.

### ReportingServicesSetup.exe Error

ReportingServicesSetup.exe Error: 0 : Application fabric:/ReportingService is not OK after 10 minutes 
Application: ReportingServicesBootstrapper.exe 
Framework Version: v4.0.30319 
Description: The process was terminated due to an unhandled exception. 

Strong name validation needs to be disabled in the Reporting server 
Run the config-PreReq script in Reporting server machine to disable strong name validation. 

### The requested operation requires elevation 
AOS users are not in the local administrator group and the UAC has not been disabled correctly. To resolve the issue, complete the following steps. 
1. Add AOS users as local admins as described in the section Join VMs to the domain 
2. Run the Config-PreReq script on all the AOS machines 
3. Make sure Test-Configuration script passes 
4. If UAC was changed, need to restart machine to take effect 

### Files in use errors
Set up exclusion rules advised by Service Fabric. For information, see [Plan and prepare your Service Fabric Standalone cluster deployment](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-standalone-deployment-preparation).



 





 
