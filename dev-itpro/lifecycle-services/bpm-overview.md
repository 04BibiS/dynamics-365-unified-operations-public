---
title: Business process modeler
description:
author: kfend
manager: AnnBe
ms.date: 09/17/2017
ms.topic: article
ms.prod: 
ms.service:  dynamics-ax-platform
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Developer, IT Pro
# ms.devlang: 
ms.reviewer: kfend
ms.search.scope: AX 2012, Operations, UnifiedOperations
# ms.tgt_pltfrm: 
ms.custom: 13301
ms.assetid: 
ms.search.region: Global
# ms.search.industry: 
ms.author: ntecklu
ms.search.validFrom: 
ms.dyn365.ops.version: 2012

---

# Business process modeler

[!include[banner](../includes/banner.md)]

Business process modeler (BPM) in Microsoft Dynamics Lifecycle Services (LCS) is a tool that you can use to create, view, and modify repeatable implementations that are based on business process libraries and flowcharts. BPM helps you align your business processes in Microsoft Dynamics 365 for Finance and Operations, Enterprise edition, with industry-standard processes that are described by the [American Productivity &amp; Quality Center (APQC)](http://www.apqc.org/). You can perform fit-gap analysis between your business requirements and the default processes in Finance and Operations. Additionally, you can add new business processes and create flowcharts for processes that aren't already defined in Finance and Operations.

BPM is compatible with the following programs:

- **Microsoft Visual Studio Team Foundation Server (TFS)** – You can generate a consolidated list of gaps and manually import them into TFS as work items that include a reference to the process flow.
- **Microsoft Word** – You can generate documentation for business processes.
- **Microsoft Visio** – You can export business process maps to Visio files.

## Prerequisites

To effectively use BPM, you must have Microsoft Office 2010 and a Microsoft Visual Studio Team Services (VSTS) project.

## Getting started

Follow these steps to access BPM.

1. Go to [LCS](https://lcs.dynamics.com/).
2. Sign in, open a project, and then select the **Business process modeler** tile. The **Business process libraries** page has three sections:

    - **My libraries** – This section contains business processes that the user has created or added.
    - **Corporate libraries** – This section contains custom business processes that someone in your organization has uploaded.
    - **Global libraries** – This section contains cross-industry standard business processes.

3. To copy a standard business process library from the **Global libraries** section to the **My libraries** section, select the upper-right corner of the tile in the **Global libraries** section, and then select **Copy**.
4. After the business process library has been added to the **My libraries** section, select the tile to view the business process library.

## Working in BPM libraries

The following topics provide more information about how to work with BPM libraries:

- [Create, edit, or browse a BPM library](creating-editing-browsing.md)
- [Synchronize a BPM library with Visual Studio Team Services](implementation.md)
- [Complete tasks in BPM](complete-tasks-bpm.md)
- [Use activity diagrams with BPM](using-activity-diagrams.md)

## See also

[Business process libraries in Business process modeler](https://docs.microsoft.com/en-us/dynamics365/unified-operations/dev-itpro/lifecycle-services/business-process-libraries-business-process-modeler)

[Upload custom business processes to BPM](https://docs.microsoft.com/en-us/dynamics365/unified-operations/dev-itpro/lifecycle-services/upload-business-processes-bpm-task-recorder)
