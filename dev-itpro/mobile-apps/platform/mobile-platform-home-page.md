---
# required metadata

title: Mobile platform home page
description: The mobile platform lets you create mobile apps for your workspaces.
author: RobinARH
manager: AnnBe
ms.date: 07/20/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-platform
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Developer, IT Pro
# ms.devlang: 
ms.reviewer: robinr
ms.search.scope: Operations, Platform, UnifiedOperations
# ms.tgt_pltfrm: 
ms.custom: 255544
ms.assetid: 
ms.search.region: Global
# ms.search.industry: 
ms.author: robinr
ms.search.validFrom: 2017-07-01
ms.dyn365.ops.version: Platform update 9

---

# Mobile platform

Microsoft Dynamics 365 Unified Operations includes support for mobile apps. You can reuse business logic and modeling from the Unified Operations. It also enables rich offline and mobile interactions, and an easy-to-use designer experience. Developers can create simplified forms in Microsoft Visual Studio and then design mobile apps that expose this functionality. The mobile platform makes it easy to change the forms and mobile app definitions to include customizations that are made to the product. 

## Getting started

+ [Getting started](mobile-platform-getting-started.md) 
+ [Architecture](mobile-platform-architecture.md) 
+ [Page design guidelines](page-design-guidelines.md)
+ [Action design guidelines](action-design-guidelines.md)
+ [Form design requirements](form-design-requirements.md)

We also have a video series about creating a mobile app:

+ [Tutorial 1: Building the sales order page](https://youtu.be/PdegfBxifl8)
+ [Tutorial 2: Building the sales order details page](https://youtu.be/mF-vlbnRte0)
+ [Tutorial 3: Building the create new sales order action](https://youtu.be/VYw9oTv9t3o)
+ [Tutorial 4: Adding a lookup to the create new sales order action](https://youtu.be/eNJKd0IYmZk)
+ [Tutorial 5: Adding a lookup and hiding pages using mobile business logic](https://youtu.be/kIJKk9J8FvI)

## Common configurations

+ [Localize mobile workspaces](scenarios/localize-workspaces-on-server.md)
+ [Secure mobile workspaces](scenarios/secure-mobile-workspace.md)
+ [Set up clickable fields](scenarios/make-workspace-field-clickable.md)
+ [Set up mandatory fields through workspace classes](scenarios/make-field-mandatory.md)
+ [Display item counts in a field](scenarios/display-count-workspace.md)

## Client-side development

Client-side APIs are used in the business logic file which provides an extensibility layer to the mobile workspace in order to allow customizing:
1. Metadata
1. Runtime control/page instances
1. Business data
1. Offline-first business behaviors
1. Layout and style

[Download the sample business logic file for Reservation management workspace](https://github.com/Microsoft/Dynamics365-for-Operations-mobile-FleetManagementSamples) (.js file)

+ [Client-side design APIs overview](scenarios/client-api-design-overview.md)
+ [Client APIs](client-apis/client-apis-reference.md)

## Server-side development

Workspace attributes and classes are used to create, configure and publish workspaces on the server. The following two categories of APIs are available for server-side development of mobile app;

+ Workspace attributes; used to create pages, tasks, entities, lookups, relationships in order to build mobile workspaces. Note that a + mobile workspace can be created through designer pane and/or through X++ attribute APIs.

+ Workspace metadata classes; used to inspect and apply server-side business logic to metadata for mobile workspaces.

+ [**Workspace class overview**](scenarios/mobile-workspace-configuration.md)
+ [**Server APIs (X++)**](mobile-workspace-server-apis.md)


