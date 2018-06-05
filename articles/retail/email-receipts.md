---
# required metadata

title: Send email receipts from Retail Modern POS
description: In Retail Modern Point of Sale (MPOS), you can send receipt emails at the time a transaction is tendered at the point of sale.  
author: jashanno
manager: AnnBe
ms.date: 06/05/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-365-retail
ms.technology: 

# optional metadata

ms.search.form: RetailParameters, SysEmailTable,
# ROBOTS: 
audience: IT Pro
# ms.devlang: 
ms.reviewer: josaw
ms.search.scope: Core, Operations, Retail
# ms.tgt_pltfrm: 
ms.custom: 252934
ms.assetid: 4b9f733b-bf28-4b85-94de-4f7adf67a62c
ms.search.region: global
ms.search.industry: Retail
ms.author: jashanno
ms.search.validFrom: 2016-11-30
ms.dyn365.ops.version: Version 1611

---

# Send email receipts from Retail Modern POS

[!include [banner](includes/banner.md)]

In Retail Modern Point of Sale (MPOS), you can send receipt emails at the time a transaction is tendered at the point of sale.  

Prerequisite
------------

You must configure a SMTP server to send email receipts. 

## Set up email receipts
### Set default options for email receipts

1.  Click **Retail &gt; Headquarters setup &gt; Parameters &gt; Retail parameters**.
2.  Click the **Posting** tab, and then under **Email receipt**, in the **Receipt option** field, select a default option:
    -   **Standard receipt** – Print receipts from the point of sale register.
    -   **E-mail** – Send receipts to customers in email messages.
    -   **Both** – Print receipts from the point of sale register and send receipts to customers in email messages.

3.  In the **Subject** field, enter the text that you want to appear by default in the subject line of a receipt that is sent as an email message.

### Set email receipt options for a customer

1.  Click **Retail &gt; Customers &gt; All customers**.
2.  On the **All customers** list page, select a customer, then click **Edit**.
3.  On the **Customers details** page, on the **Retail** FastTab, select an option in the **Receipt option** field:
    -   **Standard receipt** – The customer will receive only printed receipts. The printed receipt is generated from the point of sale register.
    -   **E-mail** – The customer will receive only email receipts.
    -   **Both** – The customer will receive both a printed receipt and an email receipt.

4.  In the **Receipt email** field, if you selected either **E-mail** or **Both** in the **Receipt option** field, enter the customer’s email address.

### Set up an email receipt profile

1. Click **Retail &gt; Channel setup &gt; POS setup &gt; POS profiles &gt; Receipt profiles**.
2. Press **CTRL**+**N** to create a receipt profile.
3. Provide a **Receipt profile ID** and **Description**.
4. On the **General** tab, click **Add** to add a new receipt type.
5. Select receipt type **Receipt** and select a receipt format to be used for email receipts.

### Add an email receipt profile to the functionality profile

1. Click **Retail &gt; Channel setup &gt; POS setup &gt; POS profiles &gt; Functionality profiles**.
2. Click **Edit**.
3. On the **General** tab, specify an email receipt profile for **Receipt profile ID**.

### Set up an email template for receipts

1. Click **Organization Administration &gt; Setup &gt; E-mail Templates.**
2. Press **CTRL**+**N** to create a new template.
3. On the **Overview** tab, complete the following:
   - In the **E-mail ID** field, enter **EmailRecpt**.
   - In the **E-mail description** field, enter a description.
   - In the **Default language code** field, select the  language.
   - In the  **Sender name** field, specify a name to appear as the sender of the email. Customers will see this name on the email as the **From** name.
   - In the **Sender e-mail** field, specify a valid email address. Customers will see this email address as the **From** email address.

4. In the lower grid, configure the following:
   - **E-mail ID** should already be populated as **EmailRecpt**.
   - In the **Subject** field, enter a title for the email receipts.
   - In the **Language** field, specify the language.
   - In the **Email** field, enter the following string: **%message%**.
   - If you want to have more than just the receipt in the message, click the **E-mail message** button to fill out the template for the body of the email messages to be sent. If you want the receipt to appear (in MPOS), insert the placeholder **%message%.**

This is the only placeholder that will be replaced when sending MPOS receipts. To get more placeholder options, you'll need to create customization on the MPOS side.

Depending on the settings that you configured, you'll need to run the respective distribution schedule jobs to sync the changes to MPOS.

-   **1010** – Customer
-   **1070** – Channel configuration
-   **1090** – Registers
-   **1110** – Global configuration

## MPOS transaction
After synchronizing the changes to the store, MPOS will prompt the user for an email address for each transaction (if enabled). If the customer already has an email address on file, that address will appear in the email address prompt. If a customer hasn't been named, or the named customer doesn’t have an email address, enter an email address and then click **Send**. When the transaction is finalized, the real-time service will send the customer an email with the receipt in the body of the message as configured above.
