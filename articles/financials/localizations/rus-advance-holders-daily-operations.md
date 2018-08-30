---
# required metadata
title: Daily operations for advance holders in Russia
description: This topic provides information on daily operations like handing cash and closing balance for advance holders for Russia. 
author: ShylaThompson
manager: AnnBe
ms.date: 10/28/2018
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

# Daily operations for advance holders in Russia

[!include [banner](../includes/banner.md)]

## Create and post disbursement slips for advance holders 

Use this procedure to create and post disbursement slips with advance holder details. The advance holder balances are posted to the related employee balance account.

1.  Click **Cash and bank management** \> **Cash transactions** \> **Slip journal**.

2.  Select **New** to create a slip journal. For more information, see **Register reimbursement or disbursement slips** chapter in [Cash - Setup and daily cash operations](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/rus-set-daily-cash-op/articles/financials/localizations/rus-setup-daily-cash-operations.md).

3.  In the **Lines** form, select **Advance holder** in **Offset account type** column, then in **Offset account** column select an advance holder and in **Credit** coulmn enter the amount.

4. Approve and post the disbursement slip. For more information, see **Post a cash voucher with cash reimbursements and cash disbursements** chapter in [Cash - Setup and daily cash operations](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/rus-set-daily-cash-op/articles/financials/localizations/rus-setup-daily-cash-operations.md).

5. To inqure the related transactions and advance holder balance select **Transactions** or **Balance** forms in **Accounts payable** \> **Advance holders** \> **Advance holders** for the selected employee.

## Create and post vendor invoices with advance holder details 

Use this procedure to create and post vendor invoices with advance holder details. The advance holder balances are posted to the employee balance account instead of the vendor balance account.

1.  Click **Accounts payable** \> **Purchase orders** \> **All purchase orders**.

2.  Select **New** to create a purchase order. For more information, see [Create a purchase order](../../supply-chain/procurement/tasks/create-purchase-order.md).

3.  In the **Purchase order** form, select **Header** view mode, and then click the **Price and discount** FastTab.

4.  In the **Terms of payment** field, select the payment term. 
    
    > [!NOTE]
    > <P>Select a payment term that has the <STRONG>From advance holder</STRONG> check box selected in the <STRONG>Terms of payment</STRONG> form.</P>

5.  In the **Advance holder** field, select the advance holder for the purchase order.

6.  Enter all the required lines details and click **Purchase** \> **Actions** \> **Confirm** to confirm the purchase order.

7.  Click **Invoice** \> **Generate** \> **Invoice** to create a vendor invoice for the purchase order.

8.  In the **Vendor invoice** form, in the **Invoice identification** field group, in the **Number** field, enter the invoice number.

9.  Click **Post** to post the invoice.and close the form.

10. Click **Accounts payable** \> **Inquiries and reports** \> **Advance holders inquires and reports** \> **Transactions** to view related advance holder transactions. 

11. Click **Voucher** to open the **Voucher transactions** form. The vendor balance account is replaced by the employee balance account, and the **Posting type** field is updated to **Employee balance**.


## Create and post advance reports 

Use this procedure to create, modify, post, inquire and print advance reports.

### Create advance report

1.  Click **Accounts payable** \> **Advance holders** \> **Advance report**.

2. Select **New** to create an advance report. Alternatively, you can create a new advance report directly from **Advance holders** form - select **New** \> **Advance report**.

3. Enter advance report date in **Date** field

4. Advance report number will be automatically generated in **Advance report** field based on the number sequence defined in the parameters.

5. Select an advance holder in **Employee number** field group.

6. Enter the purpose of the advance in **Advance purpose**  field

7. In **Header** view mode select **Options** FastTab to define confirmation dates and **Officials** FastTab to define responcible managers.

### Generate advance report lines from expenses

1. Swith to **Lines** view mode.

2. Click **Copy from expends** to open **Create advance report lines** form. In this form you can see all the expense types previously defined in the parameters.

3. Select an expense line. In the **Date** field, enter the date of the transaction.

4. In the **Document number** field, enter the number of the confirmed disbursement voucher.
   > [!NOTE]
   > <P>The entries in the <STRONG>Document name</STRONG>, <STRONG>Rate</STRONG>, and <STRONG>Currency</STRONG> fields are automatically displayed from the expense rates dictionary.</P>

5. In the **Quantity of days** field, enter the number of days of the business trip.

6. In the **To weekday** field, enter the daily expense amount.
   > [!NOTE]
   > <P>The <STRONG>Total amount</STRONG> field displays the total expense amount.</P>

7. Click **Save** to transfer the entries to the **Advance report** form.
   > [!NOTE]
   > <P>The entry in the <STRONG>Ledger account</STRONG> field is displayed according to the expense rate posting setup. If the actual expense is more than the established rates, then a supplementary line for the excess amount is created when the expenses are transferred to the advance report. In this situation, the <STRONG>Over rate</STRONG> check box is selected automatically. The <STRONG>Line type</STRONG> field of the advance report that is generated by using the <STRONG>Copy from expends</STRONG> function displays the value <STRONG>Expense</STRONG>.</P>

### Generate advance report lines from sources

1. Swith to **Lines** view mode.

2. Click **Copy from sources** to open **Create lines** form. In this form you can see the list of the advance holder's documents (credit transactions) with date that coincides with the advance report date.
   > [!NOTE]
   > <P>In the upper pane, the <STRONG>Source type</STRONG> field displays the identification type of the original document. The source for type <STRONG>Facture</STRONG> is the purchase order, and the source for type <STRONG>Accounts payable</STRONG> is the vendor transaction. The <STRONG>Amount in transaction currency</STRONG>, <STRONG>Currency</STRONG>, and <STRONG>Amount</STRONG> fields indicate the total amount in the secondary currency of the transaction, the currency code, and the amount in the company's standard currency. In the lower pane, the <STRONG>Number</STRONG> field displays the number of the vendor's facture. The <STRONG>Description</STRONG> field displays the item name. The <STRONG>Currency</STRONG>, <STRONG>Amount in transaction currency</STRONG>, and <STRONG>Sales tax amount in currency</STRONG> fields display the voucher currency, the line amount, and the sales tax amount respectively.</P>

3. In the upper pane, select the <STRONG>Mark</STRONG> check box to confirm the documents to be copied.

4. Click <STRONG>OK</STRONG> to transfer the expense data to the advance report.
   In the <STRONG>Document number</STRONG> field of the advance report, the number of the source facture is copied if the <STRONG>Line Type</STRONG> field of the advance report is set to <STRONG>Facture</STRONG>, or the number of the transaction is copied if the <STRONG>Line Type</STRONG> field of the advance report is set <STRONG>to Vendor</STRONG>.
   > [!NOTE]
   > <P>You cannot edit the <STRONG>Amount</STRONG>, <STRONG>Currency</STRONG>, and <STRONG>Main account</STRONG> fields.</P>

### Generate advance report lines manually

Use this procedure to generate and post advance report lines manually. You can distribute the expense amount between different ledger dimensions. You can then view the ledger entries and verify the distribution of the expense amount.

1. Click the **Advance report lines** FastTab, and then click **Add line** to create a new line.

2. In the **Disbursement date** field, enter the date of the document that describes the expense.

3. In the **Document number** field, enter the number of the confirming document.

4. In the **Document name** field, enter the name of the confirming document.

5. In the **Currency** field, select the currency that is used for the transaction.

6. In the **Amount** field, enter the amount that is spent for the transaction.

7. In the **Confirmed amount of advance report** field, enter the confirmed expense for the advance report.

8. In the **Main account** field, select the general ledger account that the expense belongs to.

9. Click **Distribute amounts** to open **Accounting distributions** form.

10. In the **Distributed by** field, select whether to distribute amounts by extended price or discount percentage. You can create distributions in the following ways:
 
 - To create multiple distributions that have the same quantity, percentage, or amount distribution, click **Split** for each distribution. Select a ledger account for each distribution, and then click **Distribute equally**.
 
 - To create one distribution at a time, click **Split**. Select the ledger account to distribute the invoice line to, and then enter the quantity, percentage, or amount to distribute. For example, if you selected **Percent** in the **Distributed by** field, enter a percentage in the **Percent** field.

   Repeat until you have finished creating distributions.
 
11. Select **Save** and close **Accounting distributions** form.

12. In **Line details** FastTab select **Setup** tab.

13. In the **Sales tax group** field, select the code for the sales tax group for the advance report.

14. In the **Item sales tax group** field, select the code for the item sales tax group for the advance report.

15. Select the **Prices include sales tax** check box to indicate that the amount on the advance report includes sales tax.

### Post advance report

Use this procedure to post an advance report when all the advance report header and lines details are completed.
    
1.  Click **Post** \> **Post** in **Actions** pane to post the advance report.

### Print advance report 

Use this procedure to print a posted advance report. You can print an advance report after an advance invoice that is issued to an advance holder is settled.

 > [!NOTE]
    > <P>The amounts in the <STRONG>Advance reports</STRONG> form are recalculated on the date when the payment journal for the advance holder is posted.</P>
   
1.  Click **Print** \> **Print** in **Actions pane** to open the **Advance report** form.

4.  Click **OK** to print the advance report in Microsoft Excel format.


## Closing balances for an advance holder 

You can close balances for an advance holder via cash or bank.

### Closing via cash

1.  Click **Accounts payable** \> **Inquiries abd reports** \> **Advance holders inquiries and reports** \> **Balance**.

2.  In the **To date** field, enter the date to obtain advance holder balances.

3.  Select **Close via cash** to open **Close via cash** form.

4.  In the **Date of payment** field, enter the date of payment.

5.  In the **Amount to be transferred** field, enter the balance amount while closing.
    
    > [!NOTE]
    > <P>The amount indicated in the <STRONG>Amount</STRONG> field in the <STRONG>Balance</STRONG> form is displayed by default.</P>

6.  Select the **Automatic** check box to create and post the slip journal automatically.
    
    > [!NOTE]
    > <P>If the <STRONG>Automatic</STRONG> check box is cleared, a journal will be created automatically, and the journal number is displayed. This journal must be posted manually.</P>

7.  Click **OK** to generate the slip journal.
    
    > [!NOTE]
    > <P>After the slip journal is processed, either a disbursement slip (if the amount in the <STRONG>Amount to be transferred</STRONG> field is negative) will be generated, or a reimbursement slip (if the amount in the <STRONG>Amount to be transferred</STRONG> field is positive) will be generated for the advance holder when the balances are closed.</P>

8.  Close the form.

### Closing via bank

The procedure for closing balances through a bank is similar to closing through cash. You can set up the code for the journal and the bank on the **Advance holders** tab in the **Accounts payable parameters** form.

> [!NOTE]
> <P>In the <STRONG>Balance</STRONG> form, select <STRONG>Close via bank</STRONG> to open the <STRONG>Close via bank</STRONG> form.</P>


## Complete the settlement for an advance holder 

You can complete a manual settlement or periodic settlement for an advance holder. If you clear the **Automatic settlement** check box in the **Accounts payable parameters** form, you must settle the posting or closing transactions manually, or by using the periodic settlement function.

### Settle advance holder transactions manually

Use this procedure to settle advance holder transactions manually.

1.  Click **Accounts payable** \> **Advance holders** \> **Advance holders**.

2.  Select an advance holder.

3.  Click **Settle** \> **Settle open transactions** to open the **Open-transaction editing in several currencies** form. The upper pane displays debit transactions for an advance holder, and the lower pane displays credit transactions.

4.  In the upper and lower panes, select the **Mark** check box to select the transactions. The **Balance** field displays the balances after the transactions are settled.

5.  Click **Update** to complete the settlement.

6.  Close the form.

7.  In the **Advance holders** form, click **Settlements** to open the **Transaction settlements** form. You can verify the completed settlements in this form.


    > [!NOTE]
    > <P>If exchange adjustments occur when advance holder transactions are settled, exchange adjustment transactions are generated from the settlement. The exchange rate adjustment is displayed in the <STRONG>Exchange rate adjustment amount</STRONG> field in the <STRONG>Transaction settlements</STRONG> form.</P>

8.  Close the form.

9.  Click **Accounts payable** \> **Advance holders** \> **Advance reports**.

10. Select the advance report that is created for the selected advance holder.

11. On the **Financials** tab, click **View distributions** to verify that the distribution of the expense amount is correct.

### Settle advance holder transactions periodically

Use this procedure to settle advance holder transactions periodically. When you use the periodic settlement function, all open transactions are settled in chronological order.

1.  Click **Accounts payable** \> **Periodic tasks** \> **Advance holders** \> **Periodic settlement**.

2.  In the **Date of transaction** field, select the advance holder transaction date. All transactions that are posted before this date are settled.

3.  Select the **Settlement by profile** check box to settle transactions that have identical posting profiles.

4.  Select **Records to include** FastTab to define additional filtering for settlement.

5.  Click **OK** to settle the transactions.

### Cancel a periodic settlement

Use this procedure to cancel a periodic settlement for advance holder transactions.

1.  Click **Accounts payable** \> **Periodic tasks** \> **Advance holders** \> **Periodic reverse**.

2.  In the **Date of transaction.** field, select the advance holder transaction date. All transactions that are settled before this date are reversed.

3.  Select **Records to include** FastTab to define additional filtering to cancel the settlements for.

4.  Click **OK** to cancel the periodic settlement.

