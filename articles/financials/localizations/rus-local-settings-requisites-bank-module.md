
# (RUS) Set up bank accounts 


_**Applies To:** Microsoft Dynamics AX 2012 R3, Microsoft Dynamics AX 2012 R2_


> [!NOTE]
> <P>This topic has not been fully updated for Microsoft Dynamics AX 2012 R2.</P>



You must register the bank accounts for the company.

1.  Click **Cash and bank management** \> **Common** \> **Bank accounts**.

2.  On the **Overview** tab, press CTRL+N to create a new line.

3.  In the **Bank account** field, enter the bank account code.

4.  In the **Bank groups** field, select the bank group code for the bank. The **Bank name**, **BIC**, and **Corr. bank account** fields are completed based on the selected bank group code.

5.  In the **Bank account number** field, enter the bank account number.

6.  In the **Ledger account** field, select the account that will be shown on vouchers for the specified bank account.

7.  In the **Currency** field, select the currency code.

8.  On the **General** tab, in the **SWIFT code** field, enter the payment code in the SWIFT system.

9.  On the **Setup** tab, select the **More currencies** check box if the account conducts operations with various currencies.

10. In the **Payment order template** field, in the **Payment order in currency** field group, specify the path to the Microsoft Office Word template used to create and print a payment order in currency.
    

    > [!NOTE]
    > <P>The template folder must be set up in the <STRONG>Report Folder</STRONG> field on the <STRONG>Other</STRONG> tab in the <STRONG>Company information</STRONG> form.</P>



11. In the **Payment order template** field, in the **Purchase order for currency** field group, select the path to the template used to create and print a purchase order for funds in currency.

12. In the **P/O numeration** field, select the code for the document numbering series for the account.

13. On the **Address** and **Contact information** tabs, view and modify the address and contact information for the bank account. If a bank is selected in the **Bank groups** field for which the address and contact details are entered automatically, these address values are retrieved from the corresponding fields in the **Banks** table.

14. Press CTRL+S or close the form.

  
**Announcements:** To see known issues and recent fixes, use [Issue search](http://go.microsoft.com/fwlink/?linkid=389258) in [Microsoft Dynamics Lifecycle Services](http://go.microsoft.com/fwlink/?linkid=306505) (LCS).

# (RUS) Set up a foreign bank 


_**Applies To:** Microsoft Dynamics AX 2012 R3, Microsoft Dynamics AX 2012 R2_

When you make payments to the foreign bank account of a vendor, the vendor must register the bank account with a Russian bank. Otherwise, the vendor cannot receive the payments. You can use the **Banks** form to set up a foreign bank. You can then link the foreign bank to the vendor account that is associated with it.

1.  Click **Cash and bank management** \> **Setup** \> **Bank groups**.

2.  Create a foreign bank.

3.  In the **Bank type** field, select **Foreign**.

4.  Click the **General** FastTab.

5.  In the **Vendor account** field, select the vendor account to associate with the foreign bank.

## See also

[(RUS) Payments to foreign bank accounts](rus-payments-to-foreign-bank-accounts.md)

  
**Announcements:** To see known issues and recent fixes, use [Issue search](http://go.microsoft.com/fwlink/?linkid=389258) in [Microsoft Dynamics Lifecycle Services](http://go.microsoft.com/fwlink/?linkid=306505) (LCS).

# (RUS) Changes to company bank accounts 


_**Applies To:** Microsoft Dynamics AX 2012 R3, Microsoft Dynamics AX 2012 R2_

The status of the bank account for an organization determines the account reconciliation, transaction posting, and statement generation processes. The status of a bank account can be inactive either for all transactions or only for new transactions that are created after the account is inactivated. If the status of the bank account is inactive for all transactions, you cannot post any outgoing or incoming payments and payment receipt transactions. You cannot generate incoming or outgoing transaction statements for inactive bank accounts. If the status is inactive for new transactions, you cannot post incoming payments, but you can post outgoing payments for transactions that were posted when the account was active.

You cannot change the bank account status to inactive if the account has any vendor transactions that are open. You also cannot use the **Transit account** function to change the date of a posted transaction when you reconcile a bank account that is inactive.

  
**Announcements:** To see known issues and recent fixes, use [Issue search](http://go.microsoft.com/fwlink/?linkid=389258) in [Microsoft Dynamics Lifecycle Services](http://go.microsoft.com/fwlink/?linkid=306505) (LCS).

# (RUS) Create a document type code to export or import payments 


_**Applies To:** Microsoft Dynamics AX 2012 R3, Microsoft Dynamics AX 2012 R2_

Use the **Document type** form to create a code for a document type that you can use to export or import payments. The document type is used to identify the payment files when you export or import payments. Document type codes are used to identify incoming and outgoing payments. These payments are posted to bank accounts for the selected payment document types. A unique two-digit code is assigned to every type of document. For example, 01 can be used for payment orders, and 09 can be used for memorial slips.

1.  Click **Cash and bank management** \> **Setup** \> **Payment order** \> **Kinds of documents**.

2.  Press CTRL+N to create a new document type code.

3.  In the **The code of document kind** field, enter a two-digit document code. For example, enter 01 for payment orders.

4.  In the **Description** field, enter the name of the document.

5.  In the **Document type** field, select the type of document:
    
      - **Pay document** – Create a payment order, which includes a payment transaction for import or export to the client bank.
    
      - **Memorial order** – Create a memorial slip. A memorial slip is a primary document that the bank uses to transfer money from a cash account to a bank account, or from a bank account to a cash account.
    
      - **Currency transfer** – Create a currency transfer order.
    
      - **Currency sale** – Create a currency sale when you sell foreign currency for business transactions.
    
      - **Currency purchase** – Create a currency purchase transaction when you transfer foreign currency for business transactions.

## See also

[(RUS) Document type (form)](https://technet.microsoft.com/en-us/library/jj665436\(v=ax.60\))

  
**Announcements:** To see known issues and recent fixes, use [Issue search](http://go.microsoft.com/fwlink/?linkid=389258) in [Microsoft Dynamics Lifecycle Services](http://go.microsoft.com/fwlink/?linkid=306505) (LCS).

