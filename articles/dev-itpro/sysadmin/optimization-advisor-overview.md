---

# required metadata

title: Optimization advisor overview
description: This topic discusses what tools you have avaialble to ensure optimal cofiguration of your Dynamics 365 Finance and Operations, Enterprise Edition installation. 
author: roxanadiaconu
manager: AnnBe
ms.date: 02/02/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-applications
ms.technology: 

# optional metadata

ms.search.form: SelfHealingWorkspace
# ROBOTS: 
audience: Application User, IT Pro
# ms.devlang: 
ms.reviewer: yuyus
ms.search.scope: Core (Operations, Core)
# ms.tgt_pltfrm: 
ms.custom: 
ms.assetid: 
ms.search.region: global
ms.search.industry: 
ms.author: roxanad
ms.search.validFrom: 2017-12-01
ms.dyn365.ops.version: 7.3 

---

# Optimization advisor overview

[!include[banner](../includes/banner.md)]

This topic explains what tools you have available to ensure optimal configuration and smooth operation of your Dynamics 365 Finance and Operations, Enterpise Edition installation.

## Overview

The availability and performance of the ERP installation and the smooth operation of business processes are negatively affected by incorrect or incomplete module configuration and setup. Another major impacting factor is business data correctness, completeness and cleaness.   
The **Optimization advisor** is a tool that enables power users, business analysts, functional consultants and various IT support functions to identify configuration and business data issues. The **Optimization advisor** suggests configuration best practices and points out what business data is obsolete or incorrect.

The **Optimization advisor** runs a set of best practice rules, on a perodic basis. When a violation of a rule is detected, an optimization opportunity is created and displayed in the **Optimization advisor** page. A user can take corrective action from the **Optimization advisor** page direclty and fix the best practice violation.

Standard security policies apply to optimization opportunties. For example, optimization opportunities related to the **Warehouse management** module configuration are visible only to users that have access and can make changes to **Warehouse managemnet** setup.
Opportunites are displayed in the language of the end user.

Opportunities can be company specific or cross-company, depending on the type of setup and data they validate. Cross-company opportunities can be viewed from all companies. Company specific opportunities can be viewed only by changing company first. 

To complement the set of rules that are shipped with the standard version, you can create rules that are specific to your customizations, ISV solutions and business data. The process of creating a new rule is described [in this article](./optimization-advisor.md).

When you take action on an opportunity, the system can calculate the impact this had, in terms of business process runtime reduction. This feature is unfortunately not available for all the optimization opportunities.

For more information on the **Optimization advisor**, watch the short YouTube video:

> [!Video https://www.youtube.com/embed/MRsAzgFCUSQ]

## Optimization rules

The complete list of optimization advisor rules and their periodicity can be accessed by navigating to: **System administration** > **Periodic tasks** > **Maintain diagnostics validation rule**. For a rule to be validated, it must have status **Active**. The validation frequency can be set to **Daily**, **Weekly**, **Monthly** or **Unscheduled**.

When you want to trigger the validation of unscheduled rules or when you want to re-evaluate periodic rules outside their predefined schedule, navigate to **System administration** > **Periodic tasks** > **Schedule diagnostics validation rule**. In the **Diagnostic rule validation** dialog, choose a frequency. All rules that have the specified frequency will be re-evaluated.

The current set of optimization rules can be divided in the following categories:

**1. Module configuration and set up**

Warehouse management setup is a complicated process. To aid with that, we have introduced a few rules validating the correctness of the setup, such as warehouse location directive setup for fixed product variant locations, for sales orders and transfer orders.
There are a few rules that check whether features that have been enabled are actually being used. For example, there is a rule that checks whether you are using the **Master planning** module. If it identifies that you are not, then it will create an optimization opportunity suggesting you turn off planning processes.  

**2. System configuration**

If certain functionality controlled by a configuration key is not used, an optimization opportunity will be generated, suggesting to disable the configuration key. Examples include Catch Weight, Budget planning, Project, Approved vendor list configuration keys.

**3. Business data consistency and cleanup**

If master data is not correct (e.g. you have unit of measure conversions for units that have not been defined, you have unit of measure conversions that have a division by 0, etc), then the rules controlling this will fire and you will see an optimization opportunity to correct the data. 
If you have too many and too old batch job history entries, obsolete items, closed on hand entries for warehouse enabled items, etc you will also get optimization opportunities suggesting to clean up the data. Keeping your data clean ensures better overall system performance.

**4. Best practices**

If you are not running certain business processes according to best practices (e.g. running inventory pre-closing before the inventory closing, use scheduled batch for subledger journal batch transfer, etc), you will get optimization opportunities informing you what the best practice is and asking you to follow it.



## Optimization opportunities

To view the optimization opportunities that result from the optimization rules evaluation, open the **Optimization advisor** page, available from the default dashboard.
On this page, you can get more information about an opportunity by clicking the **More information** button. If you want the system to take action and correct the setup, clean the data, etc without you having to navigate to the corresponding forms, you can click the **Take action** button. 
There is no workflow for optimization opportunities. Once you have clicked **Take action** or once you click a navigation path available in the **More information** dialog, the optimization opportunity will dissapear from the list. If the corrective action didn't solve the issue completely, next time the rule is evaluated, the opportunity will be generated again. 
If an opportunity doesn't apply to your role, you can chose to **Hide from my list**. Even if the rule behind this opportunity will trigger again, you will not see this opportunity in your list.
If you want to deactivate the evaluation of certain rules, click on the opportunity generated by the rule and then click **Deactivate analysis**.
