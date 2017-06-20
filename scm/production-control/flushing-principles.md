---
# required metadata

title: Flushing principles
description: This topic describes ***.
author: BibiSp
manager: AnnBe
ms.date: 04/04/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-applications
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Application User
# ms.devlang: 
# ms.reviewer: 2084
ms.search.scope: Operations, Core
# ms.tgt_pltfrm: 
ms.custom: 221264
ms.assetid: dde49743-1541-4353-a030-63ca3069cd7d
ms.search.region: Global
# ms.search.industry: 
ms.author: johanho
ms.search.validFrom: 2016-11-30
ms.dyn365.ops.version: Version 1611
---

# Controlling raw material consumption with the flushing principle 

[!include[banner](../includes/banner.md)]

The flushing principles reflect different consumption strategies for raw materials used in production processes. Consumption is the process that deducts material from the on-hand inventory and sets the value of the consumed materials to Work in progress (WIP) for production and batch orders. Raw materials are normally consumed from a location that is configured for the process that  consumes the material. This location is called the Production input location. 

Prior to material consumption the materials are moved to the input location:

[![](./media/scenario4a.png)](./media/scenario4a.png)

1.	Material warehouse
2.	Raw material picking
3.	Production input location
4.	Raw material consumption
5.	Production process

Material consumption is controlled by the following four flushing principles:
-   Manual
-  	Start
-   Finish
-   Picked from location

The flushing principles are configured in a defaulting hierarchy starting from the released product with the value Start. On the bill of material or formula line the flushing principle from the product can be overridden. The flushing principle on the production bill of material lines or batch order formula lines is defaulted from the product or overridden value on the bill of material or formulas.

## Description of the flushing principles

### Manual
Using this principle, it is indicated that registration of material consumption is a manual operation. This is, for example, relevant if you want to be able to track time and quantity of consumed batch or serial numbers needs to be accounted for, for tracking purposes. Manual consumption is registered in a production picking list journal and for items enabled for advanced warehouse processes, a hand-held flow can be applied.

### Start
With the use of the Start principle it is indicated that material will be automatically consumed when the production order is started. The amount of material consumed is proportional to the quantity that is started. Together with the manufacturing execution system, the flushing principle Start can also be used to flush materials when an operation or process job is started. The use of this principle is, for example, relevant in cases where the variance in the consumption is low, for low-value materials, no tracking requirements, or short run time on operations. 

### Finish
With the use of the Finish principle it is indicated that material will be automatically consumed when the production order is reported as finished or an operation that is set up to consume the materials is registered as completed. The amount of material consumed is proportional to the quantity that is reported as finished. Together with the manufacturing execution system, the flushing principle Finish can also be used to flush material when an operation or a process job is completed. The use of this principle is relevant in the same situations the Start principle except that the Finish principle is for operations with a longer run time and materials should not be set to Work In Progress (WIP) before the operation is finished. 

### Available at location
With the use of the Available at location principle it is indicated that when the material is registered as picked for production, it is automatically consumed. The material is registered as picked from location when work for the raw material picking is completed or when material is available on the production input location and the material line is released to the warehouse. The picking list generated in the process is posted in a batch job. The use of this principle is, for example, relevant in situations where you have many picking activities against one production order. In this case you need not update the picking list manually and you can get a current view of the WIP balance.
