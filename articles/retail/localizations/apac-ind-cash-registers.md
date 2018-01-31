---
# required metadata

title: GST integration for cash registers for India
description: This topic provides an overview of the cash register functionality that is available for India. It also provides guidelines for setting up the functionality.
author: EvgenyPopovMBS 
manager: annbe
ms.date: 01/31/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-365-retail
ms.technology: 

# optional metadata

ms.search.form: 
audience: IT Pro
# ms.devlang: 
ms.reviewer: shylaw
ms.search.scope: Retail, Operations
# ms.tgt_pltfrm: 
# ms.custom: 
ms.search.region: India
ms.search.industry: Retail
ms.author: v-pakris
ms.search.validFrom: 2018-1-31
ms.dyn365.ops.version: 7.3.1
---
# GST integration for cash registers for India

This topic provides a walkthrough of the features that are related to Goods and Services Tax (GST) in Microsoft Dynamics 365 for Retail industry. This document also highlights the effect of GST on various type of business transactions, and shows the accounting and posting of transactions of various types with the receipt print at the POS.

## Prerequisites 

- Execute **Goods and Services tax configuration in Dynamics 365** script
- Configure Retail channel components
  -To enable India-specific functionality, you must configure extensions for Retail channel components. For more information, see the [deployment guidelines](./apac-ind-loc-deployment-guidelines.md).
  
## India tax entities mapped in Dynamic 365 for Retail
The navigation path for the India tax entities are different in the **Dynamics 365 for Retail** and **Dynamics 365 for Finance and operations**. The below table guides the navigation path for the India tax entities in the **Dynamics 365 for Retail**.

| **India tax entities**              | **Navigation path in Dynamics 365 for Retail**                                |
|-------------------------------------|-------------------------------------------------------------------------------|
| Business verticals                  | Retail \> Channel setup \> Sales taxes \> Business verticals                  |
| Enterprise tax registration numbers | Retail \> Channel setup \> Sales taxes \> Enterprise tax registration numbers |
| GST reference number sequence group | Retail \> Channel setup \> Sales taxes \> GST reference number sequence group |
| HSN codes                           | Retail \> Channel setup \> Sales taxes \> HSN codes                           |
| Service accounting codes            | Retail \> Channel setup \> Sales taxes \> Service accounting codes            |
| Maintain setoff hierarchy profiles  | Retail \> Channel setup \> Sales taxes \> Maintain setoff hierarchy profiles  |
| VAT schedules                       | Retail \> Channel setup \> Sales taxes \> VAT schedules                       |
| Tax setup                           | Retail \> Channel setup \> Sales taxes \> Tax configuration \> Tax setup      |

## Additional setup for Retail solutions

### Validate tax information for the retail store

The tax information on the Retail store, defaults from the selected retail warehouse as defined in the Warehouse master. The configured tax information from the store gets printed on the POS receipt and updates on the retail sales order at the headquarter for the financial postings.

1.  Go to **Retail** \> **Channels** \> **Retail stores** \> **All retail stores**.
2.  Select a retail store.
3.  Click the **Tax information** FastTab.

![Tax information FastTab](media/apac-ind-gst-tax-info-tab.png)

Configure Language text and custom fields 
------------------------------------------

This content guides to configure the **Language text** and **Customs fields** that can be used in the receipt format for the POS receipts.

The **Language ID**, **Text ID**, and **Text **values that are shown in the screenshot are just examples. These values can be change as per the requirement.

The default company of the user creating the receipt setup should be the same as the legal entity in which the Language text setup is created. Alternatively, the same language texts should be created in both the default company of the user and the legal entity of the store for which the setup is being created.

**Language text**

1.  Go to **Retail** \> **Channel** s**etup** \> **POS setup** \> **POS profile \> Language text**.
2.  Click the **POS** tab.
3.  Click **POS language text** fasttab.
4.  Enter the **Language ID** field, which should be same as the language preference of the user.
5.  Enter the **Text ID** field, equal or greater than 900001.
6.  Enter the **Text** field.

[Language text](media/apac-ind-gst-language-text.png)

**Custom fields**

On creating the Customs fields, the **Caption text ID** field values must be corresponded to the **Text ID **field value as defined in the **Language text.**

1.  Go to **Retail** \> **Channel setup** \> **POS setup** \> **POS profile \> Customs fields**.
2.  Enter the **Name** field.
3.  In the **Type** field, select the value.
4.  Enter the **Caption text ID** field.

>   [./media/image7.png](./media/image7.png)

Receipt format
--------------

This section guides to configure the custom fields to the appropriate sections in the **Receipt format designer**, which must be printed on the POS receipts.

1.  Click **Retail** \> **Channel setup** \> **POS setup** \> **POS profile** \> **Receipt formats**.

2.  Select a receipt format for the **Receipt** receipt type and make the required changes.

    ![](media/b659ddac93fafde55ab52bd4dafe250b.png)

Update receipt profiles 
------------------------

**●** Click **Retail** \> **Setup** \> **POS** \> **Receipt profile**.

![](media/3f97d8b8e1765d486000b99b8d7d00a3.png)

Update POS invoice number 
--------------------------

This feature facilitates to reconcile the POS receipt number with the Invoice number for the named customer retail transactions.

On selecting **the Update POS invoice number** checkbox in the **Retail parameter**, the POS receipt number gets populated in the **Transaction id** field for the corresponding Retail sales order.

1.  Click **Retail** \> **Headquarter setup** \> **Parameters** \> **Retail parameters**.

2.  On the **Posting** tab, select the **Update POS invoice number** check box to update the POS receipt number as the invoice number for customer transactions.

![](media/1192e3380ce6d03f925285acbc39edc7.png)

>   **Note:**

-   You can select the **Update POS invoice number** check box only if the existing receipt format includes the store number and terminal number.

![](media/2c91c32afa96e6b4ca882dfddf4745fd.png)

Run a distribution schedule 
----------------------------

To sync the Generic tax engine data from the headquarter to the POS database, a job has been added in the **Distribution schedule** form.

1.  Click **Retail** \> **Periodic** \> **Data distribution** \> **Distribution schedule**.
2.  Verify that a new job, **1180**, has been added for **Generic tax engine**.
3.  Run all the jobs (**9999**).

Scenarios 
----------

### Sales to a registered customer 

Sales to a registered customer are known as B2B sales. If the store location’s state and place of supply state (customer address) are same, then the transaction becomes an intrastate sale and CGST & SGST are payable. If the store location’s state and place of supply (customer address) are in different states, then the transaction become an interstate and IGST is payable.

Customer address with the registration number are required fields for these transactions

**Customer order**

1.  Login to the POS.
2.  Enter the items, and then click **Enter**.

![](media/fe7610d6d760bbc2635d4ab52677df19.png)

Validate GST calculates as per the rate defined in the Tax setup.

**Example**:

| **Item/Service** | **Unit price** | **Tax rates** | **CGST** | **SGST** |
|------------------|----------------|---------------|----------|----------|
| M0001            | 10,000.00      | CGST12%       | 1,200.00 | 1,100.00 |
|                  |                | SGST11%       |          |          |
| M0002            | 5,000.00       | CGST10%       | 500.00   | 500.00   |
|                  |                | SGST10%       |          |          |
| S0001            | 1,200.00       | CGST11%       | 132.00   | 60.00    |
|                  |                | SGST5%        |          |          |
|                  | 16,200.00      |               | 1,832.00 | 1,660.00 |
| Total amount     | 19,692.00      |               |          |          |

1.  Click **Orders \>Create customer order**.
2.  Click on **Add customer** to select the customer account.
3.  Click **Pick up all**.
4.  Select the **Store** and **Pick-up date**.
5.  Click **OK**.

 [./media/image13.png](./media/image13.png)

  >  [!NOTE]
  >  In this example, Store location state is Delhi and Customer address state is Delhi. Both are of same state and hence intrastate GST gets computed.

6.  Click **Exact**, to process the deposit payment.

**Validate the receipt**

1.  Click **Show journal**.
2.  Select the transactions.
3.  Click **Receipt**.

   [./media/image14.png](./media/image14.png)

**Validate the sales order and tax document at Microsoft Dynamics AX
headquarters**

1.  Go to **Retail \> Customers \> All sales orders**.
2.  Select the sales order.
3.  On the Action Pane, on the **Sell** tab, in the **Tax** group, click **Tax
    document**.

  ![](media/40bf33a8a0b99a3266cd66c53c0e3d04.png)

**Recall and process the customer order**

1.  Login to POS.
2.  Click **Recall order**.
3.  Search for the order and Select the order.
4.  Click **Picking and packing** \> **Pickup**
5.  Click **Select all** and then **Pickup.**

   [./media/image16.png](./media/image16.png)

6.  Click **Exact,** to process the payment.

**Validate the receipt**

1.  Click **Show journal**.
2.  Select the transactions.
3.  Click **Receipt**.

![](media/8964112f465b668daa0acb3d68c7d771.png)

**Validate the voucher transactions**

1.  Go to **Retail \> Customers \> All sales orders**.
2.  Select the sales order.
3.  On the Action Pane, on the **Invoice** tab, click **Invoice journals**.
4.  Click **Voucher**.

   [./media/image18.png](./media/image18.png)

5.  Click **Tax document.**
6.  Validate the receipt number is updated as the **Transaction id**.

![](media/a139239bb9768168fa48969193893db5.png)

### Sale of taxable goods to a consumer 

Sales to unregistered customer are known as B2C sales. There is no difference in computation of tax for a B2B and B2C sales.

Customer shall be selected as **Consumer**, if the customer master is maintained with the address in the Dynamics 365.

1.  Login into POS.
2.  Enter an Item, and then click **Enter**.
3.  Click **Add customer**, and select the customer account

![](media/260dd90852026c9eb82f76e16b2ef078.png)

>   *Note: In this example, Store location state is Delhi and Customer address
>   state is Bangalore. Both are of different state and hence interstate GST
>   gets computed.*

1.  Click **Exact,** to process the payment.

**Validate the receipt**

1.  Click **Show journal**.
2.  Select the transactions.
3.  Click **Receipt**.

>   [./media/image21.png](./media/image21.png)

**Validate the retail sales invoice in Microsoft Dynamics AX headquarters**

1.  Go to **Retail** \> **Retail IT** \> **Data distribution**.
2.  Run job **P-0001** (**Channel transactions**).
3.  Close the form.

**Post the statement**

1.  Go to **Retail** \> **Channels \> Retail stores** \> **Open statements**.
2.  Create a statement.
3.  Click **Calculate statement** and then **Post statement**.

**Validate voucher transactions**

1.  Go to **Retail** \> **Customers** \> **All sales orders**.
2.  Select the sales invoice.
3.  Click **Sales order lines** \> **Tax information** button
4.  Verify the Location (Store address) and the Customer address in the
    respective tabs.

>   [./media/image22.png](./media/image22.png)

1.  Click **OK**.
2.  On the Action Pane, on the **Invoice** tab, click **Invoice journals**.
3.  Click **Voucher**.

    ![](media/0a30aaf623dd77e34e845f913fe03a55.png)

4.  Click **Tax document.**
5.  Validate the receipt number is updated as the **Transaction id**.

>   [./media/image24.png](./media/image24.png)

### Sale of taxable goods to an anonymous customer where GST is price-inclusive

**Define price-inclusiveness at the retail store**

1.  Go to **Retail** \> **Channels** \> **Retail stores \> All retail stores.**
2.  Select a retail store
3.  Select **Price include sales tax** checkbox

![](media/f201725af4374e68d900a2f5b0c9c48d.png)

**Run the distribution schedule**

1.  Go to **Retail** \> **Retail IT** \> **Data distribution**.
2.  Run the job, to update the changes in the POS database.
3. Close the form

**Point of Sales**

1.  Login into POS.
2.  Enter an Item, and then click **Enter**.

**Example**:

Taxable value = 10000.00

CGST – 12% ; SGST – 11%

![](media/685a4f2d644079b0db9511cb84445468.png)

1.  Click **Exact,** to process the payment.

**Validate the receipt**

1.  Click **Show journal**.
2.  Select the transactions.
3.  Click **Receipt**.

![](media/ec13811218348456926d3cc08bc586c5.png)

**Validate the retail sales invoice in Microsoft Dynamics AX headquarters**

1.  Go to **Retail** \> **Retail IT** \> **Data distribution**.
2.  Run job **P-0001** (**Channel transactions**).
3.  Close the form.

**Post the statement**

1.  Go to **Retail \> Channels \> Retail stores \> Open statements.**
2.  Create a statement.
3.  Click **Calculate statement** and then **Post statement**.

**Validate voucher transactions**

1.  Go to **Retail \> Customers \> All sales orders**.
2.  Select the sales invoice.
3.  On the Action Pane, on the **Invoice** tab, click **Invoice journals**.
4.  Click **Voucher**.

    ![](media/e90f01ccef6e66f96b2a826d1d4bc7de.png)

5.  Click **Tax document.**
6.  Validate the **Transaction id** is updated as per the GST number sequence
    defined in **the GST reference number sequence group**.

![](media/edfe6aa41d6f6375734b9da767d52894.png)

### Sales of exempted good 

1.  Login to POS.
2.  Enter an exempted item.

>   [./media/image30.png](./media/image30.png)

1.  Click **Exact**, to process the payment.

**Validate the receipt**

1.  Click **Show journal**.
2.  Select the transactions.
3.  Click **Receipt**.

>   [./media/image31.png](./media/image31.png)

**Validate the retail sales invoice at Microsoft Dynamics AX headquarters**

1.  Go to **Retail** \> **Retail IT** \> **Data distribution**.
2.  Run job **P-0001** (**Channel transactions**).
3.  Close the form.

**Post the statement**

1.  Go to **Retail** \> **Channels \> Retail stores** \> **Open statements**.
2.  Create a statement.
3.  Click **Calculate statement** and then **Post statement**

**Validate the voucher transactions**

1.  Go to **Retail** \> **Customers** \> **All sales orders**.
2.  Select the sales invoice.
3.  On the Action Pane, on the **Invoice** tab, click **Invoice journals**.
4.  Click **Voucher**.

![](media/f8b9b7396ff26e3fc67ee436c7d10f12.jpg)

1.  Click **Tax document**.
2.  Verify **Exempt** is yes
>   [./media/image33.png](./media/image33.png)

### Return transaction with GST

1.  Login to POS.
2.  Click **Show journal**.
3.  Select the transaction and Click **Return**.
4.  Click **Select all** and then **Return**.
5.  Verify the GST computation is as per the selected original transactions to
    be returned.

>   [./media/image34.png](./media/image34.png)

1.  Click **Exact**.

**Validate the receipt**

1.  Click **Show journal**.
2.  Select the transactions.
3.  Click **Receipt**.

>   [./media/image35.png](./media/image35.png)

**Validate the retail sales invoice at Microsoft Dynamics AX headquarters**

1.  Go to **Retail \> Retail IT \> Data distribution**.
2.  Run job P-0001 (Channel transactions).
3.  Close the form.

**Post the statement**

1.  Go to **Retail \> Channels \> Retail stores \> Open statements**.
2.  Create a statement.
3.  Click **Calculate statement** and then **Post statement**.

**Validate the voucher transactions**

1.  Go to **Retail \> Customers \> All sales orders**.
2.  Select the sales invoice.
3.  On the Action Pane, on the **Invoice** tab, click **Invoice journals**.
4.  Click **Voucher**.

![](media/dcaf7e838c23813ac82c544708708b78.png)

1.  Click **Tax document**.
2.  Validate the return receipt number is updated as the **Transaction id**.

![](media/473bb893a2b1c82bf034bd2f4f8c9069.png)
