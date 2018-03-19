---
# required metadata

title: Upgrade from AX 2012 - Cutover testing (Mock cutover)
description: This topic explains how to test the tasks that occur after you turn off AX 2012 but before you turn on Dynamics 365 for Finance and Operations, Enterprise edition. 
author: tariqbell
manager: AnnBe
ms.date: 01/31/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-platform
ms.technology: 

# optional metadata

# ms.search.form: 
audience: Developer, IT Pro
# ms.devlang: 
ms.reviewer: margoc
ms.search.scope:  Operations
# ms.tgt_pltfrm: 
# ms.custom: 
ms.search.region: Global
# ms.search.industry: 
ms.author: tabell
ms.search.validFrom: 2017-06-16
ms.dyn365.ops.version: Platform update 8
---

# Upgrade from AX 2012 - Cutover testing (Mock cutover)

[!include[banner](../includes/banner.md)]

[!include[upgrade banner](../includes/upgrade-banner.md)]

*Cutover* is the term that we use for the final process of getting a new system live. This cutover process consists of the tasks that occur after Microsoft Dynamics AX 2012 is turned off but before Microsoft Dynamics 365 for Finance and Operations, Enterprise edition, is turned on. The purpose of upgrade cutover testing is to practice the cutover process, to help guarantee a smooth experience for everyone who is involved during the actual cutover to go-live.

There are two main workstreams during a cutover:

- **Technical workstream** – This workstream includes the data upgrade execution process. Your business will enforce a limit on the amount of downtime that is allowed. During this downtime, neither AX 2012 nor Finance and Operations will be available. This workstream might have to tune the data upgrade procedure to meet the business's downtime limit.
- **Functional workstream** – This workstream includes the configuration tasks that are performed after the data upgrade is completed. All these tasks must be documented and quantified, and a resource must be assigned, because both the functional workstream and the technical workstream must fit within the business's downtime limit.

The following illustration shows the overall process for cutover to go-live as it will occur in the production environment.

![Cutover process](./media/cutover_1.png)

This cutover process differs from a basic data upgrade validation in a sandbox environment in the following ways:

- The data upgrade process is performed on the production environment, this approach is faster.
- Because the production environment for Finance and Operations has restricted access, some tasks that were previously performed on the sandbox instance of Application Object Server (AOS) are now performed by the Microsoft DSE team. These tasks include running the data upgrade process.
- We added the following tasks:
    - Perform a smoke test.
    - Complete application setup tasks. This step can be large, depending on the functionality that is used. During this step, the functional team configures new application functionality so that it's ready to be used in the upgraded system.
    - Allow users back in. Notify your user base that the upgrade is completed and that they can use the system again.


> [!NOTE]
> In this article, we use the term *sandbox* to refer to a Standard or Premier Acceptance Testing (Tier 2/3) or higher environment connected to a SQL Azure database.

## Technical workstream

The technical workstream involves various technical team members: the database administrator (DBA), the AX 2012 system administrator, server administrators, and developers who are familiar with AX 2012 and Finance and Operations. This topic will explain which tasks involve which roles.

During cutover testing, the technical team is focused on performance and reliability testing of the data upgrade process, to make sure that it meets the business's downtime limit. Many elements of hardware and software are involved in this process. Some of these elements are on-premises, whereas others are in the Microsoft cloud. In addition, many elements of custom application code and standard code are involved. The result of this testing should be confidence in the cutover process for your environment.

### Overall process

These are the high-level steps og the production environment upgrade process, cutover testing will use the same steps for the technical workstream.

1.	Submit an **Other type** service request through LCS to notify DSE of your intention to upgrade a production environment. Work with your Microsoft solution architect and ensure you do this with plenty of notice. Indicate in the service request that this is a mock cutover.
2.  Turn of AX 2012 AOS instances.
3.	Create a copy of the AX 2012 database. We strongly recommend that you use a copy, because you must delete some objects in the copy that will be exported.
4.	Export the copied database to a bacpac file by using a free SQL Server tool that is named SQLPackage.exe. This tool provides a special type of database backup that can be imported into SQL Database.
5.	Upload the bacpac file to the AOS machine.
6.	Download the bacpac file to the Application Object Server (AOS) machine in the sandbox environment, and then import it by using SQLPackage.exe. 
7.	Notify the Microsoft DSE team that your database is ready for upgrade – they will copy the imported database from sandbox to the production environment.
8.	Microsoft DSE team will run the data upgrade against the imported database.
9.	Microsoft DSE team will notify you once the data upgrade is complete – at this point you can log in and complete any functional configuration tasks required post-upgrade before you allow the end users back into the new system.

The next sections contain more details.

### Turn off the AX 2012 AOS instances

This task involves the AX 2012 system administrator and the server administrators.

The following areas should be validated:

- **Batch jobs that are running at the time of cutover** – A long-running batch job that has already started to run will prevent the system from stopping. Plan ahead, so that you can stop your AOS instances at the desired moment. You might have to schedule batches so that they're completed some time before you turn off AX 2012.
- **Integrated systems** – You might have other systems that are integrated with the AX 2012 environment. You must factor these systems into your plan to turn off AX 2012. For example, you might have to turn off the integrated systems some time before you turn off AX 2012 itself, so that any remaining in-flight transactions can be completed. The requirements for integrated systems vary widely from business to business. Therefore, your team of experts must plan for this scenario independently.

### Create a backup of the AX 2012 database

This task involves the DBA. The backup will be used if a rollback is required. It will also be used as a reference point that will be kept for a period and show the system state at the moment of cutover.

The following areas should be validated:

- Get concrete timings for the backup process.
- Adjust the backup options that are used (for example, compression versus non-compression), to help guarantee the fastest possible backup.

### Export the database to bacpac

This task involves the DBA. The output of this task is the export file that will be uploaded to Microsoft Azure for the new system.

The following areas should be validated:

- Get concrete timings for the backup process.
- Optimize the export process to help guarantee the fastest possible experience. Optimization might require the following tasks:

    - Measure system resources during export, such as CPU, disk I/O, and memory.
    - If resource bottlenecks are found, create a plan to mitigate them. Typically, you will mitigate these bottlenecks by assigning more of the required resource.

### Upload the bacpac file to Azure storage

This task involves the DBA or the server administrators. During this task, the bacpac file is moved into Azure.

The following areas should be validated:

- Select the machine that will be used for upload, and make sure that the Azure Storage explorer tool is configured and ready to use.
- Get concrete timings for the upload process by measuring it several times. Upload times will vary, based on the speed of your Internet connection and the geographical location of the Azure datacenter in relation to your location.

### Download and import the bacpac file to the Azure SQL database

This task involves your DBA. The outcome of this task is the export file that will be uploaded to Azure for the new system.

The following areas should be validated:

- Get concrete timings for the import process.
- Optimize the export process to help guarantee the fastest possible experience. Optimization might require the following tasks:

    - Measure system resources during export. Here are some examples:

        - **On the AOS machine:** CPU, disk I/O, and memory
        - **On the Azure SQL Database instance:** SQL database throughput (DTU). You can monitor Azure SQL DTU from Microsoft SQL Server Management Studio on the AOS machine by looking at the sys.dm_resource_stats system view.

    - If resource bottlenecks are found, create a plan to mitigate them. Typically, you will mitigate these bottlenecks by assigning more of the required resource. Because this machine is Microsoft-hosted, you must submit a request to Microsoft to increase resources if you identify that they are a bottleneck.

### Execute the data upgrade process

Once you have completed importing the database, notify the DSE team that the database is ready to be upgraded. Include the location (Environment name) that the database has been imported on.
The DSE team will perform this task, the old database structure is transformed to the schema of the new system. The database synchronization process also runs as part of this task. Database synchronization might take a significant amount of time in some situations, such as when a clustered index has changed on a table, because this operation is a costly operation in SQL.

We strongly recommend that you have first performed your analysis and debugging process in a development and sandbox environment. In a sandbox environment, debugging and analysis options are more restricted. The goal is to have few or no issues that must be addressed when you do cutover testing.

The following areas should be validated:

- Get concrete timings for the import process.
- Optimize the process to help guarantee the fastest experience. Optimization might require the following tasks:
    - Monitor the performance of individual upgrade scripts through the ReleaseUpdateScriptsExecution table.
    - Adjust scripts to optimize performance. This task might require that you customize a script’s X++ code to optimize it for your dataset.
    - Monitor Azure SQL DTU by using Microsoft Dynamics Lifecycle Services (LCS) monitoring or the sys.dm_resources_stats system view in Management Studio. If resources are maxed out, you might have to request a higher database level from the Microsoft DSE team.

### Roll back to AX 2012

The goal of this task is to restore the database by using the backup that was made when AX 2012 was turned off, and then turn AX 2012 back on. The state of integrated systems might also have to be restored. However, because integrated systems vary from business to business, you must plan for this scenario independently, based on your specific circumstances. Although it's unlikely that you will have to roll back, it's very important that you have a tested process in case you require it.

## Functional workstream

After data upgrade, several configuration tasks will be required in the new environment. The goal of this workstream is to document and quantify all configuration tasks, and to assign a resource to each task, to help guarantee that these tasks can be done together with the technical workstream during the downtime window.

Typically, functional tasks involve changing the values of specific system parameters or other configuration data. These tasks are identified through the full functional test pass, which is a separate activity from the cutover testing. When a task of this type is identified, it should be reviewed together with the functional resource and your developer.

Larger changes might require that a new custom data upgrade script be written to update the data during the data upgrade process. However, the functional resource can manually run smaller changes through the new system after data upgrade.

Larger changes that have new data upgrade scripts must be tested. Therefore, one or more additional iterations of the MajorVersionDataUpgrade.zip package will have to be run. It's important that you weigh the cost of running the package again against the cost of manual data entry.

For each manual change, a task must be added to the cutover plan document. This task must show the following details:

-	What is the task, and what must be done?
-	Who must do it?
-	How long does it take?
