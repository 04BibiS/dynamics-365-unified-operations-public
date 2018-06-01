---
# required metadata

title: Validate an application in Dynamics 365 for Finance and Operations
description: This topic provides information about the requirements that are used to verify that custom code meets Microsoft guidelines, and that a solution package can be successfully deployed in a Dynamics 365 for Finance and Operations environment. 
author: kfend
manager: AnnBe
ms.date: 04/13/2018
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
ms.search.scope: Lifecycle Services
# ms.tgt_pltfrm: 
ms.custom: 196913
ms.assetid: 5f9729e3-ff67-4526-b2aa-d7f9f3062a41
ms.search.region: Global
# ms.search.industry: 
ms.author: omarc


---

# Validate an application for Dynamics 365 for Finance and Operations

[!include[banner](../includes/banner.md)]


This topic provides information about the requirements that are used to verify that custom code meets Microsoft guidelines, and that a Lifecycle Services (LCS) solution package can be successfully deployed in a Dynamics 365 for Finance and Operations environment. 

Microsoft requires specific reviews in order to validate the following requirements:

-   A partner's custom code meets Microsoft guidelines.
-   A Microsoft Dynamics LCS solution package can be successfully deployed.
-   Transactions can be completed.

Currently, partners must demonstrate that these requirements have been met by doing test deployments and then sharing the results with Microsoft. No code will be deployed on a customer environment that Microsoft hasn't validated. Partners must complete the following curation artifacts and tests:

-   Code analysis report (CAR)
-   Business process modeler (BPM)/test scripts
-   Business database backup
-   Project name and description
-   Data packages and Process data packages (PDPs)
-   Methodology (optional)
-   Binaries (optional)
-   Deployable packages
-   Models (code and tests)
-   Marketing content

### Curation meeting

The following table describes the steps that must be completed before the validation meeting.

| Phase | Step | Activity                               | Process steps                                                                                                                                                   | Success criteria                                                                                                                                                                                                                                                                                                                        |
|-------|------|----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1     | 1    | Validate code                          | Run all customer model files by using the CAR tool, and then generate the report.                                                                               | Successfully create a CAR without any localization, accessibility, performance, or security issues.                                                                                                                                                                                                                                     |
| 1     | 2    | Verify user experience (UX) guidelines | Follow UX guidelines to implement the workspace correctly.                                                                                                      | Reference best practice information in the Migrate and Create methodology section of LCS.                                                                                                                                                                                                                                               |
| 2     | 3    | Create a project                       | Create an LCS project by using the solution package.                                                                                                           | Successfully package an LCS project. Correctly deploy package artifacts, such as the BPM.                                                                                                                                                                                                                        |
| 2     | 4    | Deploy an environment                  | Deploy a standard Microsoft Dynamics 365 for Finance and Operations environment that has partner code that is based on the package contents (Code, Binaries, and Config).   | Successfully deploy at least one Finance and Operations environments without any errors. The environment configuration is the same as the reference environment of the partner. (The environment configuration includes components and the configuration.) A user can successfully sign in to this environment without any errors. |
| 2     | 5    | Configure and deploy data              | Deploy partner-supplied data in the environment without any errors.                                                                                              | View the supplied data in the package in LCS Config manager without any errors. In the specified legal entity, deploy the data that are supplied to the environment without any errors. Create new data in the selected master, and then push the data to the Finance and Operations environment, without any errors.               |
| 2     | 6    | Do a sanity check                      | After the data have been loaded into the environment, users should be able to sign in and complete typical transactions (as defined in the scope of the solution). | Users can sign in to the data-loaded environment without any errors. Transactions can be completed, as defined in the package scope, without any errors.                                                                                                                                                                                |

### Detailed curation requirements

The following table provides more information about each curation requirement.

| Requirement                  | Description                                                                                                                                                                                                                                                                          |
|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CAR                          | All major issues that the CAR highlights should be addressed after you upgrade. The CAR must be submitted to Microsoft before the validation meeting.                                                                                                                                |
| BPM/test scripts             | All task recordings should be completed for the industry vertical that the solution package is designed for and should include end-to-end scenarios.                                                                                                                                 |
| Business database backup     | A business database of your upgraded Finance and Operations environment and best practice configurations should be loaded into the Asset library in LCS.                                                                                                                        |
| Project name and description | The project name and description should be incorporated into the beginning of the implementation methodology for the solution package.                                                                                                                                               |
| Data packages and PDPs       | All data packages should be loaded into LCS before the validation meeting. You should be able to modify the data packages and load them into an empty environment. By using only data packages and a user, you should be able to run business processes that are in the BPM library. |
| Methodology                  | The methodology should incorporate an overview of the product. Guided experience to Conference Room Pilot 1 (CRP1) and any implementation methodology specifically tailored for the partner's solution are optional. |
| Binaries (optional)          | Incorporate any required binary files.                                                                                                                                                                                                                                               |
| Deployable packages          | Incorporate the deployable packages that are required in order to bring your custom features and functionality into Finance and Operations.                                                                                                                                     |
| Models (code and tests)      | Incorporate any model files that are required for your solution.                                                                                                                                                                                                                     |
| Marketing content            | Add your marketing content, which includes logos, descriptions, and screen shots of your solution package. The logo should be industry-specific and should not include your company name. The description should be aligned with your business processes.                            |


## Update and maintenance requirements
If you have a curated solution that is published on AppSource, you must keep the solution up to date. After each release in the Spring and Fall, you will have six weeks to upgrade your code. You must update and test the following artifacts:

-   CAR
-   Models (code and tests)
-   Deployable packages
-   BPM/test scripts
-   Data packages and PDPs
-   Business database backup

### Maintenance process steps

| Phase | Number | Activity                                            | Process steps                                                                | Success criteria                                                                                                                                                                                                                                                                                     |
|-------|--------|-----------------------------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1     | 1      | Validate Finance and Operations customer code. | Run all customer model files by using the CAR tool, and generate the report. | Successfully create a CAR without any localization, accessibility, performance, or security issues. All major issues that the CAR highlights should be addressed after you upgrade to the latest major release. The CAR must be submitted to Microsoft six weeks after the Spring and Fall releases. |

See also
--------

[AppSource for Dynamics 365 for Finance and Operations](lcs-solutions-app-source.md)
