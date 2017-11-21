---
# required metadata

title: Managing Assortments
description: This article explains the basic concepts of assortment management in Dynamics 365 for Retail and provides implementation consideration for your project.
author: jblucher
manager: AnnBe
ms.date: 11/21/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-365-retail
ms.technology: 

# optional metadata

# ms.search.form:  [Operations AOT form name to tie this topic to]
audience: Application user
# ms.devlang: 
# ms.reviewer: josaw
ms.search.scope: Retail, Operations 
# ms.tgt_pltfrm: 
# ms.custom: [used by loc for topics migrated from the wiki]
ms.search.region: Global
# ms.search.industry: retail
ms.author: jeffbl
ms.search.validFrom: 2017-11-21  
ms.dyn365.ops.version: Application update 5 
---

# Managing Assortments
## Insights:
Microsoft Dynamics 365 for Retail provides assortments to allow users to manage product availability across channels.  Assortments determine which products are available at which stores and during which time period.   
In Dynamics 365 an assortment is a mapping of one or channels (or groups of channels, using org hierarchies) to one or more products (or groups of products, using category hierarchies).
A channels overall product mix is determined by all published assortments that are assigned to the channel.  This means that users can configure multiple active assortments per channel.

### Basic assortment setup
In the example below (Figure 1) a unique assortment is configured for each store.  In this case only “Product 1” is available in “Store 1” and only “Product 2” is available at “Store 2”.

![alt text](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/jblucher-manage-assortments/articles/retail/media/Managing-assortments-figure1.png?raw=true "Figure 1")

In order to make “Product 2” available in “Store 1” the user can either add the product to “Assortment 1” (Figure 3) or add “Store 1” to “Assortment 2” (Figure 4).

![alt text](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/jblucher-manage-assortments/articles/retail/media/Managing-assortments-figure2.png?raw=true "Figure 2")

![alt text](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/jblucher-manage-assortments/articles/retail/media/Managing-assortments-figure3.png?raw=true "Figure 3")

### Organization hierarchies
In situations where multiple channels share the same product assortments, users can configure the assortments using the “Retail assortment” organization hierarchy.  Adding nodes from this hierarchy will include all channels in that node and its child nodes (Figure 4).

![alt text](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/jblucher-manage-assortments/articles/retail/media/Managing-assortments-figure4.png?raw=true "Figure 4")

### Product categories
Similarly, on the product side, groups of products can be included by using product category hierarchies.  Users can configure assortments by including one or more category hierarchy nodes (Figure 5).  In this case, the assortment will include all products in that category node and its child nodes.

![alt text](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/jblucher-manage-assortments/articles/retail/media/Managing-assortments-figure5.png?raw=true "Figure 5")

### Excluded products or categories
In addition to including products and categories in assortments, users can also use the exclude option to more easily specify definition.  In the example below, the user intends to have all products in a specified category included, except for “Product 2”.  Rather than defining the assortment product by product or creating additional category nodes, the user can simply include the category, but exclude the product.

*Important:* In a situation where a product is both included and excluded by definition in one or more assortments, the product will always be considered excluded.

![alt text](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/jblucher-manage-assortments/articles/retail/media/Managing-assortments-figure6.png?raw=true "Figure 6")

### Global and released products
Assortments are defined at a global level and can contain channels from multiple legal entities.  The products and categories that are included in assortments are also shared across legal entities.  However, in order to actually sell, order, count, or receive a product, within the channel (for example within POS), the product must first be released.  This means two stores in different legal entities can share an assortment containing the same product, but the products are only available if they have been released to the legal entities.

### Dynamic and static assortments
Assortments can be defined with specific channels and products or by including organization units and categories.  Assortments including references to these groups are considered dynamic assortments.  If the definition or contents of those groups change while the assortment is active, the definition of the assortment also changes.

For example, an assortment is initially defined and published by referencing a category of products.  If additional products are subsequently added to the category, they are automatically included in the existing assortment.  The user does not need to manually these products to the assortment.

Similarly, if a channels organizational unit is added to different node, its assortment would automatically be adjusted based on that definition.

### Stopped products 
Released products can be “stopped” for the sales process by enabling the option in the “Default order settings”.  This setting is most used when a product is end of life and should not be sold at any channel.  Assortments will respect this setting and stopped products will not be assorted, regardless of the assortment configuration.

### Blocked products
In addition to stopping the sales of a product, users can also temporarily block the sales of a product.  This can be configured on the Retail tab of a released product.  Blocked products will still be assorted, but the user will receive a message in POS that the product cannot be sold.

### Date effectivity
In order to allow retailers to configure when products should or should not be available per channel, assortments are date effective.  Users can define and publish their assortments ahead of time and specify the start and end dates.   

### Process assortments batch job
Assortments that are defined in Dynamics 365 need to be processed before taking effect for two main reasons.  The main reason is that assortment definitions need to be de-normalized to make them more easily consumed by the channels.   A given channels product mix can be defined by multiple assortments spanning different date ranges.  Pre-calculating some of this information ahead of time on the server, will help to improve performance at the channel.

Second, the products and channels within the assortment can change outside of the assortment itself.  Dynamic assortments that contain references to categories or organization units, must periodically be processed to include or exclude records based on their current assignment.

## Implementation Considerations
Below are some things to consider as you plan and manage your assortments for your retail implementation.

1.	*Data replication and database size* – While assortments help to serve to serve business need to manage product availability, they are also an important tool for managing channel and offline database size.  Well managed assortments help to reduce the amount of data that needs to processed and replicated to channel and offline database, and reduce the number of records that need to be persisted.  Fewer records in these databases will increase performance for adding items to a transaction, search, and product browsing scenarios.  

2.	*Date effective/expiring assortments* – One of the most effective tools for managing the number of products in channel and offline databases is the date effectivity of assortments.  Leaving open ended (non-expiring) assortments for seasonal or products that are end of life will cause these databases to grow indefinitely.  One approach to help manage this is to keep separate assortments for seasonal products versus products that are always available.  

3.	*Sales and returns outside of assortments* – Initially retailers were unable to effectively manage their assortments since POS was unable to sell or return products outside of the assortment.  The data did not exist in the channel database, so the record could not be found.  Retailers would artificially extend their assortments to account for scenarios where:

    - A product was mistakenly not included in an assortment
    - A product was returned by the customer outside of the assortment effective dates

Now that POS allows users to sell and return outside of the assortments by making a real-time call to headquarters for the data, retailers can manage the assortments for the primary product availability requirements. 








