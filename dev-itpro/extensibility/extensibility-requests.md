---
# required metadata

title: Next steps
description: 
author: dfroslieMSFT
manager: AnnBe
ms.date: 06/20/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-platform
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Developer
# ms.devlang: 
ms.reviewer: robinr
ms.search.scope: Operations, Platform, AX Platform
# ms.tgt_pltfrm: 
ms.custom: 89563
ms.assetid: 
ms.search.region: Global
# ms.search.industry: 
ms.author: dfroslie
ms.search.validFrom: 2017-07-01
ms.dyn365.ops.version: Platform update 9
---

# Extensibility requests

The announcement of customization exclusively by extensions impacts the entire ecosystem. We recommended that you read the resources listed on the [Extensibility home page](extensibility-home-page.md). These will answer many questions and prepare you for building solutions using extensions.

Currently, you will discover that some customizations which were done with overlayering cannot be done through extensions. You will be able to do most of these things as we add more extension capabilities. For some of the overlayerings, you will need to file requests to us to enable the appropriate extension points.

# What we are doing

We've been working toward an extension-based customization model for some time now. This started with enabling extensibility and sealing platform and foundation models. Later focus has shifted to the application. We have worked with a number of ISV partners to collect extensibility requirements. The July 2017 update supports many new extensibility scenarios.

For future releases, we will be focusing on completing ISV requests and solving as many VAR-initiated requests as possible. As we do not expect to be able to handle all requests right away, we will not enforce hard sealing of the Application Suite model with next major product update. The next update will still support overlayering of this model; however, a warning will be shown when compiling overlayered code indicating that this is the last release where the Application Suite model can be overlayered.

# How do I log extensibility requests?

If you discover a customization that you cannot implement as an extension, you can unblock yourself using overlayering. Longer term this must be converted to an extension. You must log a request to Microsoft to ensure appropriate extension support is added to the product for your scenario.

Before logging the request, there are a few things to consider:  
+ Could the requirement be met with existing extensibility features? Building solutions with extensions requires different design and implementation patterns.
+ How important is the requirement to the customer and/or business analyst?  
+ Will the implementation be upgrade friendly for the long term?

Extensibility requests are logged using the extensibility feedback program on the Connect site. A non-disclosure form must be signed to gain access. Becoming a member of the Connect program will also enable access to the extensibility Yammer discussion group where you can ask questions about a specific topic.

Request are not logged through opening requests for hotfixes. We will not release extensibility requests as hotfixes.  

Extensibility requests are exclusive for Dynamics 365 for Finance and Operations only. We are not planning to accommodate extensibility requests for Dynamics AX 2012 or earlier releases.

# When will my extensibility requests be enabled?

Extensibility requests are logged to a backlog. Microsoft engineers are working on the requests in a prioritized way. We prioritize requests that accommodate multiple customers, over a request serving a single customer, at least until the time where overlayering is still available as a workaround. We plan to make the backlog publicly available, enabling you to see progress and find your requests are placed in the backlog.

# How will extensibility requests be made available to deploy?

Between the July 2017 update and the next major product update there are no application releases planned. This means any extension requests not part of this or requests made after this, would only be available with the next update. Plans beyond the next major product update have not yet been determined.

Between the July 2017 update and the next major product update, the plan is to make community preview (CTPs) builds available as downloadable VHDs. This will be current application builds, because the application is gradually maturing toward the next update. You can leverage the CTP builds to minimize the preparation time of your solution when the next major product update is publicly available.

Details on how to obtain access to the CTP builds will be shared with the extensibility feedback Connect group.

# Still having questions?

Read the [Extensibility FAQ](app-sealing-faq.md) and the other resources listed on the [Extensibility home page](extensibility-home-page.md).
