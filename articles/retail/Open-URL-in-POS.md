---
# required metadata

title: Open URL in POS
description: This topic provides an overview of improvements that have been made to product and customer search functionality in Microsoft Dynamics 365 for Retail. 
author: AamirAllaq
manager: AnnBe
ms.date: 11/13/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-365-retail
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Application user
# ms.devlang: 
ms.reviewer: sericks007
ms.search.scope: Core, Operations, Retail
# ms.tgt_pltfrm: 
ms.custom: 141393
ms.assetid: 
ms.search.region: Global
ms.search.industry: Retail
ms.author: shajain
ms.search.validFrom: 2017-06-30
ms.dyn365.ops.version: 

---
# Overview

This topic describes how you can configure a button in POS to open a URL. This feature does not require a code customization, and the feature can be configured by a non-developer persona.
This feature allows configuration of a button in POS, using button grid designer to open a URL. This feature is currently supported in the following configurations

## Open in new window

This configuration defines whether to open the URL in a new window or within the app. When configured to open a web URL within the app, the side navigation panel and top bar of POS are visible and available for user interaction. When configured top open in a web URL in a new window, the URL will open in a new app window on Modern POS for Windows, and in a new browser tab in all other POS clients. To enable this, you must configure the URL with the Open in new window option checked.
Open within POS
Opening a web URL within POS is currently only supported for Modern POS on Windows. On other clients, this capability is under development and planned for release in future updates. To enable this, you must configure the URL with the Open in new window option unchecked.

## Open a native app
This feature also allows you to specify non-web URLs to open a native app. As an example, you can specify URL protocols such as MailTo, SIP, IM, MSTEAMS, etc., which can then be handled by respective native apps on the host device. To enable this, you must configure the URL with the Open in new window option checked. 
On Windows computers, see Export or Import Default Application Associations to set the default protocol associations if you are setting up your computer using DISM. If you are using MDM, such as Intune to manage your Windows computers, see Application defaults. If you are a developer building a custom website, see Launch the default app for a URI 

## Open a native app seamlessly
Windows, iOS and Android also allow opening of apps more seamlessly, based on app protocol association. If your desired app is not already configured to handle opening from a web browser, you may need a developer persona to configure this first.
For Windows, see Enable apps for websites using app URI handlers, for iOS, see Universal Links and for Android, see Android App Links.  


|   Client                |Open in new window |Open native app | Open within POS	Details | Details                           |
|-------------------------|-------------------|----------------|--------------------------|-----------------------------------|
| Modern POS on Windows   | ✓*                |    ✓          |       ✓                  | *Opens in new Modern POS window   |
| Cloud POS               | ✓*                |    ✓          |       X                   |  *Opens in new browser tab       |
| Modern POS on iOS       | ✓*                |    ✓          |       X                  |  *Opens in new browser tab        |
| Modern POS on Android   | ✓*                |    ✓          |       X                  |  *Opens in new browser tab        |

## Before you begin
Before you begin, review how to configure Screen Layouts for POS. 
Open URL in POS
To configure a URL to be opened in POS, perform the following steps
1.	In head office, go to **Retail > Channel Setup > POS Setup > POS > Screen Layouts**.
2.	Select **Button Grids > Designer**.
3.	Create New Button
4.	Select **Button** properties.
5.	Select **Open URL** as the Action
6.	Enter desired URL.
7.	Configure whether to open the URL in a new window
