---
# required metadata

title: Data migration tool 
description:  This topic provides information about the Data migration tool (DMT) that you can use to migrate data from Microsoft Dynamics AX 2009 to Dynamics 365 for Finance and Operations.
author: kfend
manager: AnnBe
ms.date: 06/21/2018
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
ms.search.validFrom: 2018-06-21
ms.dyn365.ops.version: Platform update 8
---

# Data migration tool

[!include [banner](../includes/banner.md)]

You can use the Data migration tool (DMT) to migrate your data from Microsoft Dynamics AX 2009 to Dynamics 365 for Finance and Operations successfully. The DMT will also locate and fill any gaps between the schema of tables.

The following graphic shows the technical flow from the source system (Dynamics AX 2009) to the target system (Finance and Operations).


![Data migration technical flow](media/dmt_technical_flow.png)


With the DMT, you can export data from the source environment (Dynamics AX 2009) after the data has been through the pre-processing tasks. These tasks include:
-	Pre-process staging
-	Mapping the table fields between the source and target environments
-	Applying conversions to source data 
-	Default value setup for source data 
-	Query filter application

The following graphic shows the process flow of collecting and preparing the appropriate data in your Dynamics AX 2009 instance and then importing the data into your Finance and Operations environment. 
![Data migration process flow](media/dmt_process_flow.PNG)

Because there can be multiple legal entities in the source system, you will need to select the legal entities that contain the data to be migrated. For the selected legal entities, you can review the source tables and their row counts. You can also view any virtual companies. You can also analyze virtual companies that have legal entities attached and the related tables.

Every source table must be mapped to an equivalent target data entity so that the exported data can be migrated successfully. The mapping is set up using a Micorosft Excel mapping file which allows for automatic mapping of the source and target fields of each table. The mapping file also includes the data from the Finance and Operations schema of the data entities and the default data that is required in some tables. 

Before you can migrate your data from Dynamics AX 2009 to Finance and Operations, there are tasks based on migration requirements that must be completed. These include:

- Select target dimensions to align with the source dimensions that are populated based on selected legal entities.
- Review the inventory dimensions that are included with the selected legal entities.
- Chart of accounts for each legal entity or consolidate multiple chart of accounts into a single chart of accounts.
- Complete the basic ledger setup.          
- Apply data conversions to the souce data based on the extended data type (EDT) of the field.
