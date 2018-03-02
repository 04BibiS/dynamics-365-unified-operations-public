---
# required metadata

title: Set up number sequences by warehouse
description: This topic walks you through setting up number sequences for purchase product receipts and sales packing slips by warehouse for Poland.
author: ShylaThompson
manager: AnnBe
ms.date: 03/02/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-applications
ms.technology: 

# optional metadata

# ms.search.form:
audience: Application User
# ms.devlang: 
ms.reviewer: shylaw
ms.search.scope: Operations
# ms.tgt_pltfrm: 
# ms.custom
ms.search.region: Poland
# ms.search.industry: 
ms.author: shylaw
ms.search.validFrom: 2016-05-31
ms.dyn365.ops.version: AX 7.0.1

---

# Set up number sequences by warehouse

[!include[banner](../includes/banner.md)]

## Set up a number sequence for purchase product receipts by warehouse

You can set up a number sequence for purchase product receipts by warehouse in the Accounts payable parameters form. You can number delivery documents separately for each warehouse. 

Click Accounts payable > Setup > Accounts payable parameters. 
In the Accounts payable parameters form, in the left pane, click Number sequences. 
In the Number sequence code field, select a number sequence for the Internal product receipt reference type. 
Click Warehouses. 
In the Purchase – delivery note numbering form, in the Warehouse field, select a warehouse. 
In the Number sequence code field, enter a product receipt number sequence code for the selected warehouse. 
Repeat step 6 for each warehouse. 
Press CTRL+S or close the form. 

> [!NOTE]
> You can also set up delivery document numbering by warehouse for a purchase order. To do this, select a specific warehouse in the Posting product receipt form. After you post a product receipt, the order status of the purchase order line will not change to Delivered or Received until all purchase lines for all warehouses are posted for the purchase order. 

## Set up a number sequence for sales packing slips by warehouse

You can set up a number sequence for sales packing slips by warehouse in the Accounts receivable parameters form. You can number delivery documents separately for each warehouse. 

Click Accounts receivable > Setup > Accounts receivable parameters. 
In the left pane, click Updates. Select the Independent delivery note numbering check box. 
In the left pane, click Number sequences. Select a number sequence for the Packing slip reference type. 
Click Warehouses. 
In the Sales - delivery note numbering form, in the Warehouse field, select a warehouse. 
In the Number sequence code field, enter a packing slip number sequence codes for the selected warehouse. 
Repeat step 6 for each warehouse. 
Press CTRL+S or close the form. 

> [!NOTE]
> You can also set up delivery document numbering by warehouse for a sales order. To do this, select a specific warehouse in the Packing slip form. After you post a packing slip, the order status will not change to Delivered or Received in the sales order until all sales lines for all warehouses are posted for the sales order. 

