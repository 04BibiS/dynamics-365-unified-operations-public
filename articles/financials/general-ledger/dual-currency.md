---
# required metadata

title: Dual currency and the changes to reporting currency
description: This topic provides information about the change to reporting currency for Microsoft Dynamics 365 for Finance and Operations.  
author: kweekley
manager: AnnBe
ms.date: 10/10/2018 
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-applications
ms.technology: 

# optional metadata

ms.search.form: LedgerJournalTable, Ledger, AssetTransReportingCurrencyAmountsWizard,BankAccountTransReportingCurrencyAmountsWizard, LedgerTrialBalanceListPage
audience: Application User
# ms.devlang: 
ms.reviewer: shylaw
ms.search.scope: 
# ms.tgt_pltfrm: 
# ms.custom:
ms.search.region: Global
# ms.search.industry: 
ms.author: kweekley
ms.search.validFrom: 2018-10
ms.dyn365.ops.version: 8.1

---

# Dual currency: Changes to reporting currency

The reporting currency has been repurposed to become a second accounting currency. The changes for dual currency are inherent to the system and cannot be disabled through a confirmation key or parameter. Because we are treating the reporting currency like a second accounting currency, changes were made to the posting logic in how the reporting currency is calculated.

In addition, various modules have been enhanced to track, report, and utilize the reporting currency in various processes. The impacted modules include General ledger, Financial reporting, Accounts payable, Accounts receivable, Cash and bank management, and Fixed assets. After upgrading, there are required steps that must be completed for the Cash and bank management and Fixed asset modules. Please read those sections closely to complete the necessary steps.

## Posting – all modules

The posting logic has been changed for all transactions that post to general ledger. Previously the reporting currency was calculated as follows:

**Transaction currency amount > Accounting currency amount > Reporting currency amount**

For example, a transaction is entered in CAD currency. The CAD amount is translated to the Accounting currency, which is USD. Then the USD amount is translated to the reporting currency, which is EUR. This means the exchange rates must exist between CAD and USD and USD and EUR.

The new calculation is as follows:

Transaction currency amount > Accounting currency amount

Transaction currency amount > Reporting currency amount

With this change, exchange rates must now exist between CAD and USD and CAD and EUR.

## Reports and inquiries – impacted modules only

Various reports and inquiries have been updated to show the reporting currency amounts, in addition to the accounting currency amounts. Not every report and inquiry has been updated. For example, no changes were made to reports that only show amounts in the transaction currency.

The changes follow one of two patterns. First, if the report or inquiry had enough space to show amounts in both the accounting and reporting currency, the reporting currency amounts were added. If the report or inquiry did not have enough space to show amounts in both currencies, an option was added for the user to select which currency they wanted to view.

For various reports and inquiries, logic was also added to suppress the reporting currency amounts if the reporting currency was the same as the accounting currency, or if the reporting currency wasn’t defined on the Ledger for the legal entity.

## Financial journals – impacted modules only

The financial journals, such as the general journal, vendor invoice journal, etc. have been updated to include additional information on the reporting currency. First, totals for the voucher and journal are now shown in the reporting currency. We also added the reporting currency’s exchange rate information on the General tab of the journal Lines, allowing you to override the reporting currency’s exchange rate during transaction entry.

## Module changes

The following modules have been enhanced to use the reporting currency as a second accounting currency:

-   General ledger
-   Financial reporting
-   Accounts payable
-   Accounts receivable
-   Cash and bank management
-   Fixed assets

### General ledger

If a reporting currency is defined on the Ledger, the general ledger already tracked reporting currency amounts on every accounting entry but is now translated from the transaction currency amount. The following additional changes were made in the general ledger module:

-   A separate exchange rate type can be defined on the Ledger for the reporting currency. If an organization doesn’t want to use a different exchange rate type, the field can be left blank or select the same exchange rate type that is used for the accounting currency. If the exchange rate type for the reporting currency is left blank, the system will use the exchange rate type for the accounting currency.

-   A new journal, Reporting currency adjustment journals, has been created to allow posting adjustments to ledger accounts in only the reporting currency. The journal only allows posting to ledger accounts, does not support intercompany, and the currency must be the reporting currency of the legal entity in which the journal is posted. When posted, the transaction currency and accounting currency amounts will be zero and the reporting currency amount will be posted with the amount entered on the transaction. This journal may be used for adjustments after upgrading due to changes in how the reporting currency is used in Accounts payable, Accounts receivable and    Fixed assets. See the sections for these modules for examples of where this journal may be used.

-   The Period allocation process has been updated to allocate amounts in the transaction, accounting and reporting currency. Previous, we would only allocate the amounts in transaction and accounting currency, and then the accounting currency amount was translated to the reporting currency, which could result in the a balance remaining on the ledger account in the reporting currency. Now the amounts are calculated and ‘plugged’ into the accounting entry without any translation occurring.

-   The foreign currency revaluation process already revalued amounts in the reporting currency. The only change is that the reporting currency amount is now calculated through the transaction currency amount, as described in the Posting logic changes section.

-   Many reports and inquiries in general ledger already had the reporting currency, but a few did not. One example is the Trial balance list page, which now includes columns for both the accounting and reporting currency. Note that the reporting currency columns are hidden if the accounting currency and reporting currency are the same, or if there wasn’t a reporting currency defined on the Ledger.

### Financial reporting

An enhancement has been made to Financial reporting which allows a financial statement to include reporting currency amounts in one of two way. On the column definition, you can now choose to either directly report on the reporting currency amounts posted to the Ledger (new functionality) or continue to translate from the accounting currency to the reporting currency amounts (existing functionality).

The change is available in the column definition under the Currency Display setting. If **Reporting currency from Ledger** is selected, the amounts will be reported directly from the general ledger rather than being translated. If you want the column to print translated amounts, select the **Translate to XXXX** option, where XXXX is the reporting currency you want to display in the column. The accounting currency amounts will be translated to the selected currency using the existing translation functionality.

### Accounts payable/Accounts receivable

The Accounts payable and Accounts receivable modules already tracked reporting currency amounts but the amounts weren’t shown or used for various processes.

The following changes were made:

-   The reporting currency amounts are now shown on Transactions for both customers and vendors. This also includes the open balance of each transaction.

-   The aging process has been updated allowing an organization to view the aging buckets in either the accounting or reporting currency.

-   Various inquiries and reports were updated to show the reporting currency amounts, such as the Customer/Vendor to Ledger reconciliation reports.

-   The foreign currency revaluation process already revalued amounts in the reporting currency. The only change is that the reporting currency amount is now calculated through the transaction currency amount, as described in the Posting – all modules section.

-   UPGRADE CONSIDERATIONS - Prior to upgrade, the reporting currency amounts for documents (invoices, payments, etc) were calculated through the accounting currency. Let’s assume an invoice was posted prior to the upgrade and Dual currency changes. The invoice isn’t paid. During the upgrade, the invoice’s accounting entry is not changed. Now the organization upgrades and the Dual currency changes are in affect. When the payment is made, the payment’s reporting currency amount will now be calculated through the transaction currency amount. There may be a slight difference in the realized gain/loss amount that is calculated when the payment and invoice are settled due to the difference in how the reporting currency amounts were calculated. If the amount is considered a material difference, the new Reporting currency adjustment journal can be used to adjust the balance of the realized gain/loss and Accounts payable/receivable ledger accounts in only the reporting currency.

### Cash and bank management

The Cash and bank management module did not track any reporting currency amounts for the transactions posted against each bank account. After the upgrade, the reporting currency amounts will automatically be tracked for any new transactions posted, but the reporting currency amounts need to be added to the already posted transactions in the subledger.

-   UPGRADE CONSIDERATIONS – Because the bank account transactions did not track the reporting currency amounts in the subledger, a wizard has been added to add the reporting currency amounts on the bank account transactions. **The wizard will not post to general ledger again, but instead is taking the reporting currency amounts from general ledger and updating them in the subledger tables.**

    -   The wizard is located at Cash and bank management – Setup – Add reporting currency amounts to bank account transactions.

    -   The wizard will show transactions for all bank accounts in the current company that have a reporting currency amount of zero. This will only include transactions posted prior to the upgrade.

    -   The wizard will find the corresponding accounting entries in general ledger for each bank account transaction and default the reporting currency amount for each transaction. The amounts can be edited, but that is not recommended. You may not be able to reconcile the bank account balances to general ledger. The only time an amount should be edited is if the reporting currency amount could not be found in general ledger. In that scenario, the wizard would show an amount of zero for the reporting currency.

    -   When selecting Cancel on the wizard, the bank account transactions and any changes to the reporting currency amounts will be saved for the next time you run the wizard. The amounts are not updated on the bank account transactions though. When you select Finish on the wizard, the reporting currency amounts will be updated on the bank account transactions. If a mistake is made, the wizard can be run multiple times, allowing you to fix any incorrect reporting currency amounts.

-   No processes within Cash and bank management were changed, but various reports and inquiries were updated to display the reporting currency amounts. This includes the Cash flow forecasting reports, which were not updated to include the reporting currency amounts.

### Fixed assets

The Fixed assets module did not track any reporting currency amounts for the transactions posted against each fixed asset book. After the upgrade, the reporting currency amounts will automatically be tracked for any new transactions posted, but the reporting currency amounts need to be added to the already posted transactions in the subledger.

In addition, major changes have been made to the Depreciation process which require user action after the upgrade. It’s important to read and understand the following changes, even if you are not using the Fixed asset module yet.

-   The depreciation process has been changed in how it determines the reporting currency amount. The following scenario compares how depreciation previously determined the reporting currency amount and how it determines the reporting currency amount now.

**Depreciation scenario**

An asset is acquired and posted with the following amounts. Note that the reporting currency amount is posted in general ledger but isn’t stored in AssetTrans in the subledger.

| **Transaction type** | **Transaction amount** | **Exchange rate** | **Accounting currency amount** | **Exchange rate** | **Reporting currency amount** |
|----------------------|------------------------|-------------------|--------------------------------|-------------------|-------------------------------|
| Acquisition          | 1,000,000 DKK          | 2.0               | 500,000 USD                    | 2.5               | 200,000 EUR                   |

**Previous depreciation for reporting currency**

At the end of the year, the depreciation proposal is run and calculates the following amounts.

| **Transaction type** | **Transaction amount** | **Exchange rate** | **Accounting currency amount** | **Exchange rate** | **Reporting currency amount** |
|----------------------|------------------------|-------------------|--------------------------------|-------------------|-------------------------------|
| Depreciation         | 50,000 USD             | 1.0               | 50,000 USD                     | 2.6               | 19,230.77 EUR                 |

When the depreciation proposal is run, it calculates the accounting currency amount based on the depreciation method, but then it translates that amount to the reporting currency using the current exchange rate between the USD and EUR. This causes issues because the asset cannot be fully depreciated in the reporting currency when using the spot rate.

**New depreciation for reporting currency**

The depreciation process has been changed so that the reporting currency amount is also calculated using the depreciation method. If depreciation is run on that asset, you will now get the following results:

| **Transaction type** | **Transaction amount** | **Exchange rate** | **Accounting currency amount** | **Exchange rate**                                                   | **Reporting currency amount** |
|----------------------|------------------------|-------------------|--------------------------------|---------------------------------------------------------------------|-------------------------------|
| Depreciation         | 50,000 USD             | 1.0               | 50,000 USD                     | Displays 2.5, but isn’t used to translate to the reporting currency | 20,000 EUR                    |

When the depreciation proposal is run, it calculates the accounting currency and reporting currency amount based on the depreciation method. The amounts are then ‘plugged’ into the accounting entry in General ledger, without any translation occurring to determine the reporting currency amount.

-   UPGRADE CONSIDERATIONS – Because the fixed asset book transactions did NOT track the reporting currency, a wizard has been added allowing you to add the reporting currency amounts on the fixed asset book transactions. This wizard will NOT post to general ledger again. **The wizard must be run and completed for every company before an organization can enter any depreciation transactions because of the change to how the depreciation proposal calculates the reporting currency amount.**

    -   The wizard is located at Fixed assets – Setup – Add reporting currency amounts to fixed asset books.

    -   The wizard will show transactions for all fixed asset books in the current company that have a reporting currency amount of zero. This would only include transactions posted prior to the upgrade.

    -   The wizard does NOT pull the reporting currency amounts from the general ledger. As described in the previous scenario, the reporting currency amounts originally posted to general ledger were incorrectly using the spot rate. We don’t want those amounts in the fixed asset subledger, because the next depreciation calculation will use the incorrect amounts. Instead the wizard, will find the exchange rate on the date of the first acquisition and use that to recommend the reporting currency amount that should be posted in the subledger. The wizard will show something like the following based on our previous scenario:

| **Fixed asset** | **Book** | **Transaction type** | **Transaction date** | **Currency** | **Amount in transaction currency** | **Amount** | **Exch rate** | **Reporting currency amount** |
|-----------------|----------|----------------------|----------------------|--------------|------------------------------------|------------|---------------|-------------------------------|
| BUIL-00001      | 200_SLLT | Acquisition          | 6/3/2016             | DKK          | 1,000,000                          | 500,000    | 2.5           | 250,000                       |
| BUIL-00001      | 200_SLLT | Depreciation         | 6/3/2016             | DKK          | 50,000                             | 50,000     | 2.5           | 250,000                       |
| BUIL-00001      | 200_SLLT | Depreciation         | 6/3/2016             | DKK          | 50,000                             | 50,000     | 2.5           | 250,000                       |
| BUIL-00001      | 200_SLLT | Depreciation         | 6/3/2016             | DKK          | 50,000                             | 50,000     | 2.5           | 250,000                       |

-   Many customers tracked their asset transaction details in spreadsheets,
    including the exchange rates and amounts. If you have this data in a
    spreadsheet, it can be used to create a custom exchange rate type. The
    exchange rate type can be created and updated with the exchange rates from
    the spreadsheet. This exchange rate type will then be used to default the
    exchange rate on the acquisition date and calculate the reporting currency
    amount. If an exchange rate type isn’t selected, the wizard will use the
    exchange rate type defined on the Ledger.

-   The Exchange rate and Reporting currency amounts can be changed. If the
    exchange rate is changed, the reporting currency amount will be recalculated
    using the new rate.

-   When selecting Cancel on the wizard, the fixed asset book transactions and
    any changes to the exchange rate or reporting currency amounts will be saved
    for the next time you run the wizard. The amounts are not updated on the
    fixed asset book transactions though. When you select Finish on the wizard,
    the reporting currency amounts will be updated on the fixed asset book
    transactions. If a mistake is made, the wizard can be run multiple times,
    allowing you to fix any incorrect reporting currency amounts.

-   **When the reporting currency amounts are updated completely in the
    subledger, you MUST change the setting “Have you updated all the reporting
    currency amounts on the fixed asset book transactions?” to Yes and choose
    Finish. Depreciation calculations cannot be completed until this flag is
    changed to Yes. After the flag is changed to Yes, the wizard will no longer
    be available.**

-   After updating the Fixed assets transactions in the subledger with the
    reporting currency amounts, these amounts will not match the amounts
    originally posted to general ledger. The Reporting currency adjustment
    journal can be used to update the balances of the depreciation ledger
    accounts in general ledger, so the balances will now match.

-   An entity **Asset transaction reporting currency amounts** has been added in
    the Data management workspace, allowing you to export the data from the
    wizard. You can use this data to determine the balance in the fixed asset
    subledger for the depreciation transactions, which can then be used to
    determine the amount of the reporting currency adjustment in general ledger.

-   UPGRADE CONSIDERATIONS – Multiple setup fields have been added to fixed
    assets that will be used in the depreciation calculation. These fields may
    need to be updated prior to the next depreciation calculation.

    -   On the fixed asset book (Fixed assets – Books), the General fast tab has
        a new field Acquisition price in reporting currency.

    -   On the fixed asset book (Fixed assets – Books), the Depreciation fast
        tab has a new field Expected scrap value in reporting currency.

    -   On fixed asset parameters (Fixed assets – Setup – Fixed asset
        parameters), the General fast tab has a new field Minimum depreciation
        amount in reporting currency.

    -   On books (Fixed assets – Setup – Books), the General fast tab has two
        new fields: field Round off depreciation in reporting currency and Leave
        net book value at reporting currency.

-   Because the depreciation proposal now calculates amounts in both the
    accounting and reporting currency, the Fixed asset journal has been updated
    to show the depreciation amounts in the reporting currency. The transaction
    currency is always the accounting currency for depreciation transactions, so
    those amounts will continue to show in the Debit and Credit columns. Two new
    columns, Debit in reporting currency and Credit in reporting currency, are
    added to the grid.

    -   The new fields are only available when the transaction type is one of
        the four depreciation types: Deprecation, Depreciation adjustment,
        Extraordinary depreciation, or Special depreciation allowance.

    -   If a deprecation transaction type is entered, the reporting currency
        amounts will show in the new fields and the amounts can be changed.

    -   If the accounting and reporting currency is the same on the Ledger, the
        amounts will be kept in sync. If you change the Credit amount, the
        Credit in reporting currency will automatically change to match.

    -   If any other transaction type is entered in the Fixed asset journal, the
        Debit and Credit in reporting currency amounts will never show, both
        prior to posting and after posting. The accounting and reporting
        currency amounts are still available on the Voucher that posts to
        general ledger.
