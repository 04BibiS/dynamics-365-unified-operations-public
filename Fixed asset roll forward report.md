---
title: Fixed asset roll forward report
---

The Fixed assets roll forward report provides detailed fixed asset data you need
for period closing, financial statements, and tax reporting in an easy to read
Excel format. The report includes fixed asset starting and ending balances with
valuation movements for the period, along with any new asset acquisitions and
disposals that occurred during the period. Data is reported for individual fixed
assets as well as summarized values for fixed asset groups and the legal entity.

The Fixed asset roll forward report is utilizing the Electronic reporting
framework. You must have the Fixed assets model and Fixed asset roll forward
configurations imported from LCS before you can run the report. For
instructions, see [Download Electronic reporting configurations from Lifecycle
Services](https://docs.microsoft.com/en-us/dynamics365/unified-operations/dev-itpro/analytics/download-electronic-reporting-configuration-lcs).

This report is available on the 7.3 release, or as a hotfix on the July 2017
release. Environments with the July 2017 release will need three hotfixes
applied:

-   KB 4041754: Electronic reporting (ER) configuration can’t be downloaded from
    LCS as not applicable for the current application version after applying the
    platform update package

-   KB 4056107: Electronic reporting (GER) cumulative update 5

-   KB 4056353: Fixed assets Statement and Notes reports don't meet the
    requirements in GAAP and IFRS

The following fields are available on the report.

| **Field**                                   | **Description**                                                                                                                                                                      |
|---------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Balances: Opening                           | Fixed asset net book value as of the From date specified on the report.                                                                                                              |
| Balances: Closing                           | Fixed asset net book value as of the To date specified on the report.                                                                                                                |
| Acquisitions: Opening value                 | Sum of all transactions with type Acquisition and Acquisition adjustment up to the From date specified on the report.                                                                |
| Acquisitions: Period acquisitions           | Sum of all transactions with type Acquisition and Acquisition adjustment posted during the report date range.                                                                        |
| Acquisitions: Period disposals              | Sum of all acquisition reversals posted with a disposal transaction during the report date range.                                                                                    |
| Acquisitions: Closing value                 | Sum of all transactions with type Acquisition and Acquisition adjustment up to the To date specified on the report.                                                                  |
| Depreciations: Opening value                | Sum of all transactions with type Depreciation, Depreciation adjustment, Special depreciation allowance, and Extraordinary depreciation up to the From date specified on the report. |
| Depreciations: Period depreciations         | Sum of all transactions with type Depreciation, Depreciation adjustment, and Extraordinary depreciation posted during the report date range.                                         |
| Depreciations: Period special depreciations | Sum of all transactions with type Special depreciation allowance posted during the report date range.                                                                                |
| Depreciations: Period disposals             | Sum of all depreciation reversals posted with a disposal transaction during the report date range.                                                                                   |
| Depreciations: Closing value                | Sum of all transactions with type Depreciation, Depreciation adjustment, Special depreciation allowance, and Extraordinary depreciation up to the To date specified on the report.   |
| Write-ups/Write downs: Opening value        | Sum of all transactions with type Write up adjustment, Write down adjustment, and Revaluation up to the From date specified on the report.                                           |
| Write-ups/Write downs: Period write ups     | Sum of all transactions with type Write up adjustment posted during the report date range.                                                                                           |
| Write-ups/Write downs: Period write downs   | Sum of all transactions with type Write down adjustment posted during the report date range.                                                                                         |
| Write-ups/Write downs: Period revaluations  | Sum of all transactions with type Revaluation posted during the report date range.                                                                                                   |
| Write-ups/Write downs: Period disposals     | Sum of all write up, write down, and revaluation reversals posted with a disposal transaction during the report date range.                                                          |
| Write-ups/Write downs: Closing value        | Sum of all transactions with type Write up adjustment, Write down adjustment, and Revaluation up to the To date specified on the report.                                             |
| Disposals: Disposal date                    | Disposal date for the fixed asset book.                                                                                                                                              |
| Disposals: Net book value at disposal       | Fixed asset book net book value at the time of disposal.                                                                                                                             |
| Disposals: Sale value                       | The Sales value for the fixed asset book with a disposal – sale transaction.                                                                                                         |
| Disposals: Scrap value                      | The Scrap value for the fixed asset book with a disposal – scrap transaction.                                                                                                        |
| Disposals: Profit/Loss                      | The profit or loss value calculated as part of the fixed asset book disposal transaction.                                                                                            |
