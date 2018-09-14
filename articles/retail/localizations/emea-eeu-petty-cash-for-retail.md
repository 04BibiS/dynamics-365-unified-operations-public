---
# required metadata

title: Petty cash management for Retail for Eastern Europe
description: 
author:  
manager: epopov
ms.date: 10/01/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-365-retail
ms.technology: 

# optional metadata

# ms.search.form: 
audience: Application user
# ms.devlang: 
ms.reviewer: josaw
ms.search.scope: Retail, Operations
# ms.tgt_pltfrm: 
# ms.custom: 
ms.search.region: Eastern Europe
ms.search.industry: Retail
ms.author: v-kikozl
ms.search.validFrom: 2018-10-01
ms.dyn365.ops.version: 8.1
---
# Petty cash management for Retail for Eastern Europe


*Applies To: Dynamics 365 for Retail and Dynamics 365 for Finance and Operations*

Retailers can accept various types of payment in exchange for the products and services that they sell. Although cash is the most common form of payment, retailers can also receive payment in the form of checks, cards, or vouchers.

In Retail point of sale (POS), cash, credit card receipts, and other payments are processed through a cash office. You can do the following by using Cash management in Retail:

- Create a cash account for the selected payment method for each retail store.
- Use cash journals to post cash transactions and customer payments that are received at a retail POS.
- Aggregate transactions in a statement line when you post a retail statement in Microsoft Dynamics AX. You can aggregate safe drops, bank drops, voucher transactions, remove tender transactions, float entry transactions, income transactions, expense transactions, customer payments, sales transactions, and return transactions.

All transactions that take place in Retail POS are posted using a ledger journal. You can use cash payment journals, customer payment journals, and general journals to create and post the statements. For more information, go to [Create, calculate, and post statements for a retail store](https://docs.microsoft.com/en-us/dynamics365/unified-operations/retail/tasks/create-calculate-post-statement-retail-store).

  Note:
  In the **Posted statements** form, on the Action Pane, you can do the following:
  - Click **Inquiries > Cash payment journal** to access the cash payment journals that are related to the statement.
  - Click **Inquiries > General journal** to access the ledger journals that are related to the statement, other than customer payments and cash payments.

## Set up for cash management for Retail POS

You must complete the following setup procedure before you use cash management in Retail:
- Set up a payment method for each payment type that the retailer accepts in the **Payment methods** form. You can use different payment methods for posting transactions in Retail POS. For more information about payment methods in Retail, go to [Payment methods](https://docs.microsoft.com/en-us/dynamics365/unified-operations/retail/payment-methods).

- Set up retail parameters for cash operations

- Set up a payment method for cash payments in a retail store

### **Set up retail parameters for cash operations**

You can set up parameters to create and post cash transactions in Retail. You can use cash payment journals, customer payment journals, or general journals to post sales transactions and payment transactions in the Retail POS. You can aggregate transactions that have the same properties when you post a statement. 

1. Click **_Retail > Headquarters setup > Parameters > Retail parameters_**. In the left pane click **Posting**

2. In the **_Posting_** area, on the **_Aggregation_** FastTab, toggle the **Tender remove/float** to Yes to aggregate the remove tender transactions or float entry transactions that are associated with a statement line when you post the statement. A remove tender transaction is created when you withdraw cash from the POS cash drawer. A float entry transaction is created when you deposit cash in the POS cash drawer.

3. Activate the individual parameters listed below to aggregate the transactions that are associated with a statement line when you post the statement:
   - **Bank drop** – Aggregate bank transactions
   - **Safe drop** – Aggregate safe transactions
   - **Income/Expense transactions** – Aggregate income transactions or expense transactions
   - **Voucher transactions** – Aggregate voucher transactions
   - **Customer payments** – Aggregate customer payments
   - **Sales and returns** – Aggregate sales and returns transactions

4. On the **_Payments_** FastTab, select a default journal name for the following options:
     - **Customer payment journal** – This journal is used to post customer payments.
     - **Cash payment journal** – This journal is used to post cash payments.
     - **General journal** – This journal is used to post transactions other than cash payments and customer payments.

### **Set up a payment method for cash payments in a retail store**

Use the following procedure to set up a payment method for cash payments in a retail store.

1. Click **_Retail > Channels > Retail stores > All retail stores_**.

2. On the **All retail stores** list page, select the store to set up a payment method for.

3. On the Action Pane, on the **_Set up_** tab, in the **_Set up_** group, click **Payment methods**.

4. In the **Payment method** form, create or select a payment method. 

5. On the **_Posting_** FastTab, in the **_Account_** field group, in the **Account type** field, select **Cash account**.

    Note: You can select **Cash account** in the **Account type** field only if you select **Normal** or **Tender remove/float** in the **Function** field.

6. In the **Account number** field, select a cash account number for the payment method.

7. In the **_Tender remove/float_** field group, in the **Offset account** field, select the offset account to post remove tender or float entry transactions for the payment method.

    Note: You must set up offset accounts for both the cash tender payment method and the remove tender or float entry payment method for a store to create balanced general ledger entries for remove tender or float entry transactions.
