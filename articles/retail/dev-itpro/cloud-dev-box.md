---
# required metadata

title: Configuration steps for Retail developers working on cloud-hosted development machines
description: This topic demonstrates the configuration steps for Retail developers working on cloud-hosted development machines.
author: mugunthanm 
manager: AnnBe
ms.date: 01/16/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-365-retail
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Developer
# ms.devlang: 
ms.reviewer: robinr
ms.search.scope: Retail, Operations 
# ms.tgt_pltfrm: 
ms.custom: 
ms.assetid: 
ms.search.region: global
ms.search.industry: Retail
ms.author: 
ms.search.validFrom: 2017-12-08
ms.dyn365.ops.version: AX 7.0.0, Retail July 2017 update

---

# Configuration steps for Retail developers working on cloud-hosted development machines

Retail developers do not have administsrative access to cloud-hosted development virtual machines (VMs). Modern POS (MPOS) development is possible if you use Cloud POS. Before starting the development for any retail application, configure Cloud POS as follows:

1. Open  Visual Studio and click **View** > **Application Explorer**. 

    Wait for Internet Information Services (IIS) Express to start with all the Retail websites deployed. You should see the IIS icon in the task bar with all the Retail websites running, such as Cloud POS and Retail Server.
    
2. To debug CRT/RS extensions, attach the CRT/RS project to the IIS Express process.

3. When you open the Cloud POS project from the Retail SDK, IIS Express may fail with the following error. 
    ```
    Filename: redirection.config
    Error: Cannot read configuration file
    ``` 
    To resolve this issue, follow these steps:
    1. Close Visual Studio.
    2. Rename the **%userprofile%\Documents\IISExpress\config** folder. Do not delete the files because you will copy the **applicationhost.config** file to a new location in step *iv*.
    3. Start Visual Studio again with the Cloud POS project. The **%userprofile%\Documents\IISExpress\config** folder will be recreated with the default config files.
    4. Copy the **applicationhost.config** file from the folder that you renamed in step *ii*, to the folder created in step *iii*. 

