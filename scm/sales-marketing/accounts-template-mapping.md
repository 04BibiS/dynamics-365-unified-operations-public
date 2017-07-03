---
# required metadata

title: Template mapping for products
description: The topic discusses the templates and underlying tasks that are used to synchronize accounts from Microsoft Dynamics 365 for Sales to Microsoft Dynamics 365 for Finance and Operations, Enterprise edition. 
author: ChristianRytt
manager: AnnBe
ms.date: 07/3/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-applications
ms.technology: 

# optional metadata

ms.search.form: 
# ROBOTS: 
audience: Application User, IT Pro
# ms.devlang: 
ms.reviewer: YuyuScheller
ms.search.scope: Core, Operations, UnifiedOperations
# ms.tgt_pltfrm: 
ms.custom: 
ms.assetid: 
ms.search.region: global
ms.search.industry: 
ms.author: ChristianRytt
ms.dyn365.ops.intro: July 2017 update 
ms.search.validFrom: 2017-07-8

---

# Accounts (Customers)

[!include[banner](../includes/banner.md)]

The topic discusses the templates and underlying tasks that are used to synchronize accounts from Microsoft Dynamics 365 for Sales to Microsoft Dynamics 365 for Finance and Operations, Enterprise edition. 

## Template and task

The following templates and underlying tasks are used to synchronize accounts from Microsoft Dynamics 365 for Sales (Sales) to Microsoft Dynamics 365 for Finance and Operations, Enterprise edition (Finance and Operations).

-   Name of template: Accounts (Sales to Fin and Ops)

-   Name of task in project: Accounts - Account - Customers

Sync tasks required prior to Account / Customer sync: None

## Entity set

| **Sales** | **CDS** | **Finance and Operations** |
|-----------|---------|----------------------------|
| Accounts  | Account | Customers                  |

## Entity flow

**Accounts** are managed in Sales and synchronized to Finance and Operations as **Customers** where they are marked with **Is Externally Maintained = Yes**. This is done to keep track of which **Customers** originate from Sales. During invoicing, this information is used to filter invoices that sync to Sales.  

## Prospect to cash solution for Sales 

**Account Number** is made visible on the **Account** form and made into a natural and unique key to support the integration. The natural key feature of the CRM solution may affect customers who already use the **Account Number** field, but not use the unique **Account Numbers per account**. Currently, the integration solution does not support this case.

When creating a new account, if the **Account Number** does not already exist, it is automatically generated using a number sequence starting with ACC followed by an increasing number sequence and ending with a suffix of 6 characters, for example, ACC-01000-BVRCPS.

When the integration solution for Sales is applied, an upgrade script populates**Account Numbers** for existing accounts in Sales. When there are no **Account Numbers**, the above number sequence is used.

## Preconditions and mapping setup

-   **CustomerGroupId** must be updated to a valid value in Finance and Operations. This can be defaulted or set with **ValueMap**.

    -   Template value is defaulted to 10.

-   **Address country region code** is required in Finance and Operations. To avoid sync errors, you can default a value that is used if the field is left blank in Sales.

    -   Template value for **AddressCountryRegionISOCode** is defaulted to USA.

    -   Template value for **DeliveryAddressCountryRegionISOCode** is defaulted to USA.

-   It is possible to add the following mappings from **CDS-\>Destination** to reduce the manual updates needed in Finance and Operations. This can be done with a default or a **ValueMap** from, for example, Country/Region or City.

    -   **SiteId** - Site is required to generate **Quote** and **Sales order lines** in Finance and Operations. It can default from either the product, or the customer from the order header.

        -   Template value for **SiteId** is defaulted to 1.

    -   **WarehouseId** - Warehouse is required to process **Quote** and **Sales
        order lines** in Finance and Operations. Warehouse can be defaulted from
        either the product, or the customer from the order header in Finance and
        Operations.

        -   Template value for WarehouseId is defaulted to 13.

    -   **LanguageId** - Language is required to generate **Quote** and **Sales order** in Finance and Operations and defaults to the order header from the customer.

        -   Template value for **LanguageId** is defaulted to en-us.

-   Update the **CDS Organization ID Organization_OrganizationId** in **Source -\> CDS**.

    -   Template value is defaulted to ORG001.

## Template mapping in Data integrator

> [!NOTE]
> **Payment terms**, **Freight terms**, **Delivery terms**, **Shipping method**, and **Delivery mode** are not included in the default mappings. Mapping of these fields requires value mapping to be set up, which is specific to the data in the organizations between which the entity is synchronized.

The following screenshots show how the template mapping can look like in data integrator.

![template mapping in data integrator](./media/accounts-template-mapping-data-integrator-1.png)

![template mapping for products in data integrator](./media/accounts-template-mapping-data-integrator-2.png)

