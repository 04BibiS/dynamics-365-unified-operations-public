---
# required metadata

title: Configuration steps for Retail developers working on cloud-hosted development boxes
description: This topic demonstrates the configuration steps for Retail developers working on cloud-hosted development boxes.
author: mugunthanm 
manager: AnnBe
ms.date: 12/08/2017
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

# Configuration steps for Retail developers working on cloud-hosted development boxes

Because the developer cloud VMs do not have admin access to the VMs, Modern POS (MPOS) development is possible in these VMs. You need to use Cloud POS. Before starting the development for any retail application, configure Cloud POS as follows:

1. Open  Visual Studio and click **View** > **Application Explorer**. Wait for IIS Express to start with all the Retail websites deployed. You should see the IIS tray icon in the task bar with all the Retail websites, such as Cloud POS and Retail Server running.
4. To debug CRT/RS extensions, attach the CRT/RS project to the IIS Express process.
5. When you open the Cloud POS project from the Retail SDK, the Configuration IIS Express may fail with the following error. 
    ```
    Filename: redirection.config
    Error: Cannot read configuration file
    ``` 
    To mitigate this issue, follow these steps:
    1. Close Visual Studio.
    2. Rename the **%userprofile%\Documents\IISExpress\config** folder. Do not delete the files, because you will copy the **applicationhost.config** to a new location in step 4.
    3. Start Visual Studio again with the Cloud POS project. The **%userprofile%\Documents\IISExpress\config** folder will be recreated with the default config files.
    4. Replace the **applicationhost.config** file in the recreated folder with the **applicationhost.config** file in the folder you renamed in step 2.

