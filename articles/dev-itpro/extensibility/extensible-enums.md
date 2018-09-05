---
# required metadata

title: Write extensible enums
description: This topic provides information about how to write extensible enums.
author: smithanataraj
manager: AnnBe
ms.date: 09/09/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-platform
ms.technology: 


# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Developer
# ms.devlang: 
ms.reviewer: kfend
ms.search.scope: Operations
# ms.tgt_pltfrm: 
ms.custom: 268724
ms.assetid: 
ms.search.region: Global
# ms.search.industry: 
ms.author: smithanataraj
ms.search.validFrom: 2018-09-09
ms.dyn365.ops.version: Platform update 20
---

# Write extensible enums

An enumeration (enum) is made extensible by setting the following enum properties:

- Is Extensible = True
- UseEnumValue = No

If you set these properties, downstream implementors can extend the enum with more elements. The values of the elements are determined at deployment time and won't be identical across systems. However, the following behavior is guaranteed:

+ Data upgrade scripts aren't required. Enum values are persisted in the database, regardless of the enum that is extended. Therefore, when an enum is made extensible, the enum values that are used on any system will prevail.
+ The first element in the enum gets a value of 0 (zero). Therefore, an extensible enum can still be used with the **not** operator. The only exception is when the first element of the enum had a non-zero value before the enum was made extensible.
	
## Using extensible enums in code
Because enum values are no longer controlled by the developer, there is no certainty about the enum values. Here are some things that you should look for when you use extensible enums in code:

+ Extensible enums **can't be used in comparisons** (for example, **MyEnum::Value1 \> MyEnum::Value2**).
+ **Conversions between integers and enums**

    - Modeled ranges in views and queries, and queries that are created from code: 

        - By using comparisons, like \<, \>, and so on 
        - By using hardcoded integer values in comparisons 
    
    - When the model and all dependent models are compiled, the preceding will be flagged.
	
+ Make sure that logic where the enum values are used is extracted in **smaller methods**. In that way, an extension that uses Chain of Command (CoC) can handle the enum values that are added.
+ For **construct** methods where the instantiation is based on enum values, replace switch blocks with **SysExtension** wherever such a replacement is possible. In other cases, make sure that the default block is extensible. For an example, see the **PurchRFQCaseCopying** class.
+ If the enum is used in **switch blocks**, avoid having default blocks that either have or don't have throws that aren't extensible. 
+ When there are **long switch case blocks** or **if...else blocks** for the enum values, consider creating a class hierarchy to handle specific logic that is related to the enum. For an example, see the **PriceGroupTypeTradeAgreementMapping** class hierarchy.
+ Use the **in** keyword for query ranges that use the enum values, and make the container that the **in** keyword uses extensible.
