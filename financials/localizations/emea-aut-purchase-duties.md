---
# required metadata

title: Purchase duties | Microsoft Docs
description: A purchase duty is a tax on incoming sales tax and is calculated as a percentage of the paid sales tax.
This article is related to a specific functionality extension applicable for legal entities in Austria.
author: neserovleo
manager: AnnBe
ms.date: 05/25/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-applications
ms.technology: 

# optional metadata

# ms.search.form:  
audience: Application User
# ms.devlang: 
ms.reviewer: shylaw
ms.search.scope: Operations, Core
# ms.tgt_pltfrm: 
# ms.custom: 
ms.search.region: Austria
# ms.search.industry: 
ms.author: v-lenest
ms.search.validFrom: 2017-06-30
ms.dyn365.ops.version: Enterprise edition, July 2017 update
---

# Purchase duties


## Overview
A purchase duty is a tax on incoming sales tax and is calculated as a percentage of the paid sales tax.
This article is related to a specific functionality extension applicable for legal entities in Austria. For more details about a global sales tax functionality, see also [Sales tax overview](indirect-taxes-overview.md).


## Description
Purchase duty calculation is based on the Sales tax code concept. If you want to include a sales tax code in the purchase duty calculation, you should select **Purchase duty** checkbox on the **Saxes tax codes** form. 

Purchase duty settings are identified on the **Purchase duty** form in the Tax Setup menu. You can set the duty identification (i.e. KU), description, Tax authority and accounts which will be used for posting: Duty account is a balance account in which the liability for the purchase duty is registered, Cost account is a Profit and Loss account to which the costs are posted and a Settle account.

To specify the purchase duty value, click the **Purchase duty value** button to open the **Purchase duty value** form. You can specify the purchase duty percent (i.e. 0,30) for required period.

Purchase duties will be generated during the sales tax Settle and post process. Purchase duty report can be generated from the **Purchase duty reporting** menu.


