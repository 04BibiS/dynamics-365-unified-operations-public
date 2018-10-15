---
# required metadata

title: Sell, dispose, and write-off assets (Russia)
description: This topic explains how to sell, dispose, and write-off assets in Microsoft Dynamics 365 for Finance and Operations in Russia.
author: ShylaThompson
manager: AnnBe
ms.date: 10/15/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-applications
ms.technology: 

# optional metadata

# ms.search.form: 
audience: Application User
# ms.devlang: 
ms.reviewer: shylaw
ms.search.scope: Core, Operations
# ms.tgt_pltfrm: 
# ms.custom: 
ms.search.region: Russia
# ms.search.industry: 
ms.author: shylaw
ms.search.validFrom: 2018-10-28
ms.dyn365.ops.version: 8.1

---

# Sell, dispose, and write-off assets (Russia)
[!include [banner](../includes/banner.md)]

## Set up the automatic creation of a deferrals card 

When you run a fixed asset issue voucher and realize a loss, you can automatically create a deferrals card. The deferrals card shows the calculated loss and the write-off period, which is defined as the difference between the depreciated object's anticipated useful life and the actual length of time that it was held before the loss was incurred.

1.  Click **Fixed assets** \> **Setup** \> **Depreciation** \> **Depreciation group**.
2.  Click the **Deferrals** tab, and then set up the parameters for creating deferrals when a loss is realized.
3.  Select **Disposal** if you want to dispose of the fixed asset entirely, or **Partial dismantlement** to dispose of part of the fixed asset.
4.  In the **Model number** field, select the deferral model number.
5.  In the **Deferrals group** field, select the deferral group.
6.  In the **Expense code** field, select the expense code.

## Create a sales order or free text invoice for a fixed asset 

You can record the sale of fixed assets in Fixed assets, or you can create a sales order or a free text invoice in Accounts receivable. When you sell a fixed asset, you must calculate its depreciation during that period.

If the fixed asset to be sold is registered in the **Inventory management** items list, you must create a sales order. However, you can record the sale of a non-inventoried item in **Fixed assets**, or you can create a free text invoice. The status of an asset that is sold under a sales order or free text invoice is **Sold (waiting for posting)**, until the transaction is posted. After posting, the status of the asset is **Written off (sale)**.

> [!NOTE]
> You must set up posting profiles before you can create transactions for the sale of fixed assets.

If the invoice expenses are included in the sales order header when you sell a fixed asset or inventory asset, the sum of these miscellaneous charges is not included in the profit or loss from the sale.

However, if the invoice expenses are included in the order line that corresponds to the asset, the charges are considered when the profit or loss is calculated from the sale.

## Create a sales order for a fixed asset

1.  Click **Accounts receivable** \> **Common** \> **Sales orders** \> **All sales orders**.
2.  Press CTRL+N to create a new sales order.
3.  Enter information about the sales order.
4.  In the lower pane, press CTRL+N to create new line items.
5.  In the **Item number** field, select a fixed asset inventory item.
6.  In the **Quantity** field, enter the quantity of fixed assets.
7.  In the **Unit** field, enter the measurement unit for fixed assets, such as pieces.
8.  In the **Unit price** field, enter the sale value of the fixed asset.
9.  Click the **General** tab.
10. In the **FA number** field, select the fixed asset number.
11. Click the **Dimension** tab and select the warehouse dimensions.
    > [!NOTE]
    > Click **Inventory management** &gt; **Setup** &gt; **Inventory** &gt; **Item model groups**. If the **Physical negative inventory** check box is selected, then you can sell an item that is a fixed asset type without entering an inventory item. If not, you can sell an inventory asset that has a **Purchased** status.

12. Click **Posting** \> **Facture** to post the sales order.
13. Click **OK** to post the sales invoice. An invoice, facture, ledger, and fixed asset transactions are created, and the **Status** of the sales invoice changes to **Shipped**. The status of fixed asset, changes to **Written off (sale)**. The **Disposal (sale)** and **Gain/Loss** fields are updated on the **FA balances** page. The **Disposal date** and **Disposal cost** fields are updated on the **FA Value models** page.

## Create a free text invoice for a fixed asset

1.  Click **Accounts receivable** \> **Common** \> **Free text invoices** \> **All free text invoices**.
2.  Press CTRL+N to create a new invoice. For more information, see [Create a free text invoice](../financials/accounts-receivable/create-free-text-invoice-template-new.md).
3.  In the **Customer account** field, select a customer account.
4.  In the **Date** field, enter an invoice date.
5.  In the **Currency** field, select an invoice currency.
6.  Create a new invoice line.
7.  In the **Ledger account** field, select the ledger account for posting the revenue to the current invoice line.
8.  In the **Sales tax group** field, select the sales tax group.
9.  In the **Item sales tax group** field, select the item sales tax group.
10. In the **Quantity** field, enter the quantity of fixed assets sold.
11. In the **Unit price** field, enter the sale value of the fixed asset unit.
12. In the **Measurement unit** field, select the unit of measurement unit for the fixed asset.
13. Click the **General** tab.
14. In the **FA number** field, select the fixed asset number.   
    > [!NOTE]
    > When including references to the fixed asset in an invoice line, its status changes to **Sold (waiting for posting)**.

15. Click **Posting** \> **Update facture** to open the **Post facture** page.
16. Click **OK** to post the facture. An invoice, facture, and ledger and fixed asset transactions are created, and the status changes to **Shipped**. The status of the asset is **Written off (sale)**.
17. Click **Fixed assets (Russia)** \> **Common** \> **Fixed assets**. to open **FA Value Models** form. The disposal date and cost of the fixed asset are displayed in the **Disposal date** and **Disposal cost** fields.
18. Click **Balance** to open the **FA balances** form. The details are displayed in the **Leaving (sale)** and **Gain/Loss** fields.

## Fixed asset disposal 

You can dispose of fixed assets for any of the following reasons:

  - An asset is sold to other legal entities or private individuals.
  - An asset is transferred and used as a deposit in joint activities or as a charter capital.
  - An asset is donated or used as another type of non-compensated transfer.
  - An asset is liquidated because of an accident or natural disaster.
  - An asset is exchanged through an exchange agreement.

> [!NOTE]
> Depreciation must be calculated until the month of disposal.

There are three ways to create a disposal transaction:

  - **Leaving (sale)** – This transaction represents a fixed asset sale and can be entered in Accounts receivable or in the fixed assets journal. Several transactions are created in the ledger, based on the posting profile configuration. The status of the asset changes to **Written off (sale)**.

  - **Leaving (dismantlement)** – This transaction is entered in the fixed assets journal. Transactions are created in the ledger, and the remaining value of the asset after it has been dismantled is transferred to the warehouse. The status of the asset changes to **Written off (dismantlement)**.

  - **Fixed Assets write-off** – This transaction is entered in the fixed assets journal, and the write-off transactions for the booked value and booked depreciation for the asset are created. The status of the asset changes to **Written off**


> [!NOTE]
> When you create a transaction for the partial write-off or disposal of a fixed asset, if the transaction accrues a loss, the transaction details are posted to a deferral account. This account contains the values of the calculated loss and the write-off time. The write-off time is calculated by using the fixed asset depreciation factor and the difference between the useful life of the depreciated asset and the actual period that the asset is used before its disposal.
