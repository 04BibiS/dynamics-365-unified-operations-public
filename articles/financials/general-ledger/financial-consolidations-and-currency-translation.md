# Financial consolidations and currency translation
This document takes you through the approach that both Microsoft Dynamics 365 for Finance and Operations and Financial reporting use for consolidations. It describes scenarios that involve multi-company reporting, aggregation, elimination, and minority interest. It also explains how to handle special situations, such as scenarios where legal entities have different fiscal periods or different charts of accounts.

This document was written for users and functional consultants, and it assumes that readers have a general understanding of Finance and Operations and Financial reporting. Basic setup isn’t covered.
Note: The term legal entity is used in Finance and Operations, and the term company is used in Financial reporting. Both these terms are used in this document. However, for the purposes of this document, their meanings are the same.

## Audience

This document is intended for finance and accounting users and application consultants who want to use Finance and Reporting and Financial reporting to consolidate multi-company and multi-currency data.

## Approach

Finance and Operations uses a separate legal entity to process a consolidation. It enables single-instance consolidation but provides an option to bring in data from other sources. The consolidation process must be run every time that changes are made in the source legal entities.

Financial reporting can consolidate multiple companies during report generation. Although the data is stored in a data mart, is versioned, and can be exported, every source company is the owner and container of the data. The report can be run at any time, even every minute (for example). It provides many additional benefits, such as the ability to drill down to all companies and dimensions.

The following illustrations show the steps for doing consolidation in Finance and Operations and Financial reporting.

Users can use Consolidation Online, Financial reporting, or a combination. Their choice depends on the needs of their company and the preferences of their auditors.

#### Consolidate Online in Finance and Operations

![Consolidate Online in Finance and Operations](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/currency-cons-trans/articles/financials/general-ledger/media/consolidate-online-in-fin-op.png "Consolidate Online in Finance and Operations")
 
#### Consolidation by using Financial reporting

![Consolidation by using Financial reporting](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/currency-cons-trans/articles/financials/general-ledger/media/consolidate-financial-reporting.png "Consolidation by using Financial reporting")
 
## Consolidations

The **Consolidations** module includes options for consolidating multiple legal entities during the consolidation process, and for importing or exporting a company’s balance. You can also set up eliminations and post elimination journals.

## Benefits of using Consolidate online
Customers who use the Consolidations module will gain various benefits:
- **Depth of data** – You can create consolidated reports that bring together actual and budget data at both the account level and the dimension level.
- **Dynamic consolidations** – Consolidations can be processed multiple times.
- **Audit capabilities** – Dimensions and accounts are maintained for analysis and audit, and balances are created by date.
- **Currency translation** – You can set up the account ranges and rates to translate from the accounting currency of the source company to the accounting currency of the consolidation company.
- **Process eliminations in a consolidated or elimination company** – You can process and post eliminations as a single process during consolidation. Alternatively, you can run a proposal separately.

## Supported consolidation scenarios
Here are some of the consolidation scenarios that Consolidate online supports:
- Single-level consolidations across legal entities
- Consolidations that involve eliminations
- Minority interest (For this scenario, manual calculation and entry in the company must be used.)
- Multiple charts of accounts across legal entities
- Different fiscal calendars across multiple legal entities
- Consolidations that involve multiple reporting currencies

## Legal entity setup
Before you process a consolidation, you must set up the legal entity. You can run consolidation as many times as you require, and all data will be translated from the source company’s accounting currency to the currency that is defined for the consolidation company. Therefore, for the following organizational structure, if you must translate all North American companies first to US dollars (USD) and then to euros (EUR), the currency of the parent company, you must have at least two consolidation companies.

![Organizational structure](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/currency-cons-trans/articles/financials/general-ledger/media/organizational-structure.png "Organizational structure")
 
In the preceding organizational structure, you must have a legal entity for the North American consolidation, because consolidations always consolidate from the accounting currency of the source company to the currency of the consolidation company. In the example, if all companies are included in a single consolidation, the Mexican subsidiary will be translated from Mexican pesos (MXN) to EUR, not from MXN to USD to EUR.
When you create the legal entity, you can specify whether the company is used for both the consolidation process and the elimination process, or for just one of those processes. In the following illustration, the company is used for both processes. Note that you can’t post daily journals in a consolidation company, but you can post them in an elimination company. Therefore, you might want to have a separate elimination company.

![Legal entities separate elimination company](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/currency-cons-trans/articles/financials/general-ledger/media/sep-elimination-company.png "Legal entities separate elimination company")
 
## Main accounts and consolidation account groups
One of the choices that you must make is how you want to consolidate your chart of accounts. During the consolidation process, you have three options for consolidating main accounts.

The first option is to use the main accounts from the source companies. In this case, every account from all companies will be consolidated. For example, if Cash is account 100000 in the USMF company and account 1100 in the DEMF company, the consolidation company will include both accounts, each of which will have its respective balance.

The second option is to specify a default consolidation account on the **Main accounts** page. The account will then be mapped to the consolidation account. This option can be helpful when you have different charts of accounts or must map to a chart that is defined by the headquarters.

![Main accounts consolidation](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/currency-cons-trans/articles/financials/general-ledger/media/main-accounts.png "Main accounts consolidation")
 
The third option is to use consolidation account groups. You can define as many consolidation account groups as you require. Then, on the **Additional consolidation accounts** page, you just map the main account from the chart of accounts to the account that you require for that group.
 
## Consolidate Online 

To find more information about how to enter details of consolidations online, please see [Consolidate Online](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/currency-cons-trans/articles/financials/general-ledger/consolidate-online.md)

## Managing consolidation transactions
To view the results of the consolidation, you have multiple options:
- Generate a financial report against the consolidation company.
- Review the **Trial balance** list page in the consolidation company.
- In the list of consolidation transactions on the **Consolidations** page, view the balances that are created by date for every source company for every period.

![Managing consolidation transactions](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/currency-cons-trans/articles/financials/general-ledger/media/managing-consolidation-transactions.png "Managing consolidation transactions")

To run the consolidation again, you can just process the consolidation. Alternatively, you can first click **Remove the transactions** on the **Consolidations** page.

## Consolidate with import
The Consolidate with import functionality works like the Consolidate online functionality. When you select the legal entities, you will browse out to the source file that contains the data.

## Export company balances
The Export company balances functionality also works like the Consolidate online functionality. When you select the legal entities, you will set a file path for the output.

## Elimination rules
To eliminate intercompany transactions, you can create an elimination rule. Alternatively, you can do a manual elimination entry in a company that is designated an elimination company. If you create an elimination rule, you have two options for the elimination method: **Net change** and **Fixed**.

### Set up elimination rules
When you set up elimination rules in Finance and Operations, you can create a financial dimension that is used specifically for elimination. Most customers name this financial dimension **Trading Partner** or something similar. If you decide not to use a financial dimension, make sure that you have main accounts that are used only for intercompany transactions.

You can find the setup for eliminations in the **Setup** area of the **Consolidations** module. After you enter a description for the rule, you must select the company that the elimination journal will be posted to. The company that you select should have **Use for financial elimination process** selected in the legal entity setup.

You can set the date when the elimination rule becomes effective and the date when it expires, as you require. If you want the elimination rule to be available in the elimination proposal process, you must set the **Active** option to **Yes**. Select a journal name that has a type of **Elimination**.

![Ledger elimination rule journal](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/currency-cons-trans/articles/financials/general-ledger/media/ledger-elimination-rule-journal.png "Ledger elimination rule journal")
 
After you’ve defined the basic properties, click **Lines** to define the actual processing rules. There are two options for eliminations: you can eliminate the net change amount or define a fixed amount.

Select the source accounts. You can use an asterisk ( * ) as a wildcard character. For example, **1*** selects all accounts that start with a **1** as a source of data for the allocation.

After you’ve selected the source accounts, use the **Account specification** field to specify the account that is used from the destination company. Select **Source** to use the same main account that is defined in the source account. If you select **User defined**, you must specify a destination account.

![Ledger elimination rule line](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/currency-cons-trans/articles/financials/general-ledger/media/ledger-elimination-rule-line.png "Ledger elimination rule line")
 
The **Dimension specification** field works like the **Account specification** field. Select **Source**, to use the same dimensions in the destination company and the source company. If you select **User defined**, you must specify the dimensions in the destination company by clicking **Destination dimensions**. Then select source dimensions and the financial dimensions and values that are used as a source of the elimination.

### Process elimination transactions
There are two ways to process elimination transactions. The transaction can be processed during the Consolidate online process, or you can create an elimination journal and run the elimination proposal process. This section focuses on the second option.

In a company that is defined as an elimination company, click **Elimination journal** in the **Consolidations** module. After you’ve selected the journal name, click **Lines**. To run the proposal, click **Proposals** > **Elimination proposal**.

Select the company that is the source of the consolidated data, and then select the rule to process. Enter start and end dates to define the date range that is searched for elimination amounts. The **GL posting date** field specifies the date that is used to post the journal to the general ledger. After you click **OK**, you can review the amounts and post the journal.

**Note**: The elimination journal shows the amounts for account values in the currency of their originating transactions, not in the accounting currency. When you review the amounts in the elimination journal, you might find this behavior confusing.

For more information and examples, see Elimination rules.

## Currency revaluation in a consolidation company

When you consolidate data from one accounting currency to another, you must still run currency revaluation if exchange rates change, so that your account balances are correctly revalued. When you originally consolidate the data, use the **Currency translation** tab to select the initial exchange rates that should be used for translation during the consolidation process. After a new exchange rate is entered (for example, in the next month), you must revalue the account balances. The unrealized gains or losses are then updated based on the new exchange rate and date.
For more information about currency revaluation in a consolidation company see Currency revaluation in a consolidation company.
For more information about how currency revaluation works in the **General ledger** module, see Foreign currency revaluation for General ledger.

### Additional information
- All posting layers are consolidated when the consolidation is processed.
- Elimination journals can be posted only to the Current layer.
- Only operating balances are consolidated. Therefore, to see opening balances, you must still run a year-end close in the consolidation company.
- You can post a daily journal in an elimination company, but not in a consolidation company.

## Benefits of using Financial reporting for financial consolidations and currency translation, or to complement Consolidate online for consolidated reporting
Customers who use Financial reporting for financial consolidations and currency translation, or to complement Consolidate online for consolidated reporting, will gain various benefits:
- **Depth of data** – You can create consolidated reports that bring together actual and budget data at both the account level and the dimension level. For Finance and Operations, this data includes data from both budge control and Bbudget planning.
- **Dynamic consolidations** – Consolidations can be done at any time and at any level in the organizational hierarchy.
- **Complete audit capabilities** – All dimensions, accounts, and transactional detail are maintained for analysis and audit. In addition, Financial reporting provides full drill-back to the original transaction in any of the legal entities that are consolidated.
- **Streamlined currency translation** – After minimal setup in Finance and Operations, you can translate any Financial reporting report into any reporting currency that has been set up. In addition, you can set up an unlimited number of reporting currencies.
- **Post eliminations at the source** – You can create and print an elimination report to verify elimination transactions. You can then post any new eliminations as standard intercompany transactions. You can also use an elimination legal entity for any transaction that you don’t want in your legal entities.

## Supported consolidation scenarios
Here are some of the consolidation scenarios that Financial reporting supports:
- Single-level and multilevel consolidations across legal entities
- Consolidations that use organization structures that are created from legal entities
- Consolidations that involve eliminations
- Minority interest
- Multiple charts of accounts across legal entities
- Different fiscal calendars across multiple legal entities
- Consolidations that involve multiple reporting currencies
- Business unit consolidations

## Generating consolidated financial statements 

To find information about scenarios where you might generate consolidate financial statements, please see [Generating Consolidated Financial Statements](https://github.com/MicrosoftDocs/Dynamics-365-Operations/blob/currency-cons-trans/articles/financials/general-ledger/generating-consolidated-financial-statements.md)
