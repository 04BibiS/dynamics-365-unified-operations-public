---
# required metadata

title: Store fulfillment setup
description: This topic provides an overview for store order fulfillment. 
author: rubencdelgado
manager: AnnBe
ms.date: 06/20/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-applications
ms.technology: 

# optional metadata

# ms.search.form:  [Operations AOT form name to tie this topic to]
audience: [Pick one: Application User/Developer/IT Pro]
# ms.devlang: 
# ms.reviewer: [Content Strategist microsoft alias]
ms.search.scope: 
# ms.tgt_pltfrm: 
# ms.custom: [used by loc for topics migrated from the wiki]
ms.search.region: [Global for most topics. Set Country/Region name for localizations]
# ms.search.industry: [leave blank for most, retail, public sector]
ms.author: [author's Microsoft alias]
ms.search.validFrom: [month/year of release that feature was introduced in, in format yyyy-mm-dd]
ms.dyn365.ops.version: 

---

# Order fulfillment point of sale setup

## Overview
Many retailers would like to optimize order fulfillment by enabling stores to fill orders. Order fulfillment at the store level can help
to ease overstock scenarios for a specific store, or may be desirable from a logistical standpoint in cases where a store has extra
capacity or is located within closer shipping distance to the customer. To address this need, a unified order fulfillment operation is
available at the point of sale.

Orders for fulfillment at a specific store will have the store's warehouse designated on the header or lines of the order. 

The order fulfillment operation in the point of sale provides a single work area in the point of sale that can be used to process orders from accepting the order to marking it as shipped or initiating store pickup. 

## Setup the order fulfillment operation

[Operation](https://docs.microsoft.com/en-us/dynamics365/unified-operations/retail/pos-operations) 928, "Order fulfillment", can be used to access the store order fulfillment work area in the point of sale. 

[Add the operation to a button grid](https://docs.microsoft.com/en-us/dynamics365/unified-operations/retail/pos-screen-layouts) specify which parameter to use when invoking order fulfillment at the point of sale. By default, after specifying the order fulfillment operations, the parameter 'All orders' is selected. When configured with this parameter, the operation will list all order lines for fulfillment at the current store. Also available is 'Orders to ship', which can be assigned to a button and utilized when the user only wants to see orders that will ship out of the store. Finally, there is 'Orders for pick up'. When invoked at the point of sale, this lists only orders to be picked up at the store. The different parameters can be assigned to different buttons to give the user a variety of ways to view order fulfillment. 

### Enabling users to access order fulfillment at the point of sale. 

The order fulfillment operation does not have its own permission out of the box, but in the future, users may require the 'Allow retreive order' permission to invoke the operation from the point of sale. 

At the store level, a configuration setting is available that determines whether an order line must be accepted manually from within the point of sale. If that configuration option is not set, order lines will be accepted by default. If that configuration option is turned on, users at the point of sale will need to have the 'Allow accept order' permission to accept orders from within the point of sale. 

Order lines may also be rejected from the point of sale. Rejecting an order line signifies that it will not be fulfilled at that store and sends the order line back for reassignment to another store or warehouse. Order line rejection permission is granted through the 'Allow order reject' permission. The capability to reject order lines will be released as a hotfix shortly after AppUpdate 5 ships.

### Enabling manual order acceptance

Be default, order lines assigned to a store are marked as 'Accepted'. This means that it is assumed they will be fulfilled from the assigned store and will not be subject to further assignment. In certain cases, retailers may want to manually accept orders before they can be fulfilled. For example, if a store is short staffed and is unable to fulfill orders, a store manager may only accept as many orders for processing as they feel can adequately be processed in a given day. Until an order is accepted, it may be reassigned by the back office to a different store. In this way, order acceptance also provides a way to indicate that an order has been acknowledged by a store and will be fulfilled. 

Order lines for store pickup are marked as always marked as 'Pending' and are not subject to acceptance.

To turn on manual acceptance or order lines, navigate to **Retail**>**Channels**>**Retail stores**>**All retail stores**. Select the store and click in the store ID to view the store's details. Click **Edit**. Then in the general fasttab, locate the **ORDER FULFILLMENT** subheader and change **Manual accept** from 'No' to 'Yes'. 

### Synchronizing changes to the channel database

Once the operation has been assigned to a button grid, the proper permissions have been assigned, and the channel is configured, the changes must be synchronized to the channel database. To do so, navigate to **Retail**>**Retail IT**>**Distribution schedule**. Select schedule "1090-Registers" to synch button grid changes and then click **Run now**. Next, synch permission changes by selecting "1060-Staff" and then click **Run now**. Finally, synch channel changes by selecting "1070-Channel configuration" and click **Run now**.

## Using order fulfillment at the point of sale

Open the point of sale and select the order fulfillment operation. Depending on how it is configured, either all lines, order lines for pickup or order lines to ship will be listed. 

### Order fulfillment view 

The order fulfillment view lists order lines for fulfillment at the store and includes the following columns:

  - Order number
  - Product number
  - Description
  - Quantity ordered
  - Requested delivery date
  - Customer name
  - Fulfillment status
  
Additional information for a specific order line can be viewed by selecting the order line and then opening the flyout located just below the logged in user/shift information shown in the point of sale header. This flyout includes 2 tabs- one for line details and another for order details. The line details tab includes the following information:

  - Quantity ordered
  - Quantity remaining to be shipped/picked up
  - Quantity picked
  - Quantity packed
  - Quantity invoiced(alread picked up or shipped)
  - Mode of delivery
  - Delivery address
  
The details flyout also has a tab that provides more order level details including:

  - Customer name
  - Customer ID
  - Order number
  - Order total
  - Order balance
  
At the bottom of the order fulfillment view is an action pane. This contains all of the actions that can be taken against an order line. If an action is not available based on a line's status, that action will be disabled. 

By default, orders will have a status of 'Accepted'. Order status can be viewed as a column in the list of order lines. If 'Manual accept' is configured at the channel level, all lines to be shipped will show as 'Pending' and must be accepted before they can be fulfilled. Orders for store pickup are 'Pending' by default and never need to be accepted.

### Order fulfillment line actions:

**Edit**: If an order status is pending, it may be edited at the point of sale. Orders that have already been partially picked, packed or invoiced may not be edited from the order fulfillment view.

**Accept**: If "Manual accept" is configured at the channel level, lines must be first accepted before they can move through the order fulfillment process.

**Pick**: The pick option supports several actions. First is "Picking" which updates the status of the order line so others in the store do not attempt to pick the same line. Next is "Print picking list" which will print a picking list for the selected line or lines and also update their status to "Picking". Picking list formats are controlled as part of receipt formats. For more information on setting up receipt formats see [Receipt templates and printing](https://docs.microsoft.com/en-us/dynamics365/unified-operations/retail/receipt-templates-printing). Finally is "Mark as picked" which indicates the line has been picked. "Mark as picked" initiates corresponding inventory transactions in the back office. Picking actions can be performed at the same time for multiple order lines across orders and for all modes of delivery.

**Reject**: Lines or partial lines may be rejected. This allows them to be reassigned from the back office to another store or warehouse. Lines may only be rejected if they have not yet been picked or packed. To reject a line that has already been picked or packed, that line must be unpicked or unpacked from the back office. The capability to reject order lines will be released as a hotfix shortly after AppUpdate 5 ships.

**Pack**: The pack option supports two actions. "Print packing slip" will print a packing slip for the selected lines and "Mark as packed" will and mark the lines as packed and mark the lines as delivered in the back office.  Only order lines that belong to the same order and have the same mode of delivery may be packed at the same time.Packing slip formats are controlled as part of receipt formats. For more information on setting up receipt formats see [Receipt templates and printing](https://docs.microsoft.com/en-us/dynamics365/unified-operations/retail/receipt-templates-printing).

**Ship**: The ship action will mark selected lines as 'Delivered' in the back office. Once a line has been fully shipped, it will not appear again in the store fulfillment view.

**Pickup**: The pickup action adds the lines to the transaction view for pickup. If there are other lines on the order that aren't currently being picked up, they will be added to the transaction view with qantity zero. Once a line has been fully picked up, it will no longer appear in the order fulfillment view. 

### Order fulfillment filtering

Order fulfillment at the point of sale includes filtering to help the user easily find what they need. Filters can be changed through the action pane at the bottom of the point of sale screen. By default, a "Delivery type" filter is applied, based on how the operation is set up. If the operation is set up with the parameter "All orders" then that filter is applied when accessing order fulfillment. The same applies for the "Store pickup" and "Ship from store parameters". Other filters that can be applied to the order fulfillment view include:

  - Customer number
  - Customer name
  - Customer email
  - Order number
  - Mode of delivery
  - Receipt number
  - Channel Reference ID
  - Originating store number
  - Line status
  - Created date
  - Delivery date
  - Receipt date













