---
# required metadata

title: Configure Power BI integration for workspaces
description: This tutorial describes how to configure a new Microsoft Dynamics 365 for Finance and Operations, environment to support integration with PowerBI.com. This configuration enables workspaces to show the Power BI control and lets users pin visualizations to a workspace.
author: MilindaV2
manager: AnnBe
ms.date: 01/09/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-platform
ms.technology: 

# optional metadata

ms.search.form: PowerBIConfiguration
# ROBOTS: 
audience: IT Pro
# ms.devlang: 
ms.reviewer: sericks
ms.search.scope: Core, Operations
# ms.tgt_pltfrm: 
ms.custom: 27661
ms.assetid: 861cfa94-c6f3-4c84-89ac-22c78bf6b7a4
ms.search.region: Global
# ms.search.industry: 
ms.author: milindav
ms.search.validFrom: 2016-02-28
ms.dyn365.ops.version: AX 7.0.0

---

# Configure Power BI integration for workspaces

[!include [banner](../includes/banner.md)]

## Overview

Microsoft Dynamics 365 for Finance and Operations lets users pin tiles, dashboards, and reports from their own PowerBI.com account to workspaces.

This functionality requires a one-time configuration of your environment. An administrator must do this step to enable Finance and Operations and Microsoft Power BI to communicate and authenticate correctly.

Both Finance and Operations and PowerBI.com are cloud-based services. For a Finance and Operations workspace to show a Power BI tile, the Finance and Operations server must contact the Power BI service on behalf of a user and access the visualization. It must then redraw the visualization in the Finance and Operations workspace. The fact that the Finance and Operations server contacts the Power BI service "on behalf of a user" is important. When a user, such as `D365User@contoso.com`, contacts the PowerBI.com service, Power BI should show only tiles and reports from D365User's PowerBI.com subscription.

By completing this configuration step, you enable Microsoft Dynamics 365 Finance and Operations to contact the PowerBI.com service.

## Things you must know before you start 

- You must be a system administrator in Finance and Operations. This option is available on the **System administration** menu.
- You must have a PowerBI.com account. You can create a trial account if you don't have an account. (A Pro license isn't required for this configuration step.)
- You must have at least one dashboard and one report in your Power BI account. Although the dashboard and report aren't required for this configuration step, you might not be able to validate the configuration if you don't have any content in your PowerBI.com account.
- You must be an administrator for your Microsoft Azure Active Directory (Azure AD) account. If you aren't the administrator, an administrative user must perform this configuration step for you.
- The Azure AD domain that is configured for Finance and Operations must be the same domain that you used for your PowerBI.com account. For an example, if you provisioned Finance and Operations in the Contoso.com domain, you must have Power BI accounts in that domain, such as `Tim@ContosoAX7.onmicrosoft.com`.

## Registration process 

1. Log in https://portal.azure.com/ by using Azure tenant admin account<br>
   **Reminder:** The user executing this procedure must have Admin rights for the tenant to register applications

2. Go to **Azure Active Directory >> App registrations >>  New application registration** 
    ![Azure Portal App Registration](media/Azure-Portal-AppRegistration.png)

3. Enter the following values in the dialog.
	**Name:** Your app name
	**Application type:** Web app / API
	**Sign-on URL:** The base URL of your Finance and Operations client, and then add the OAuth suffix<br>
			 For example: http://contosoax7.cloud.dynamics.com/oauth
			 
4. Click **Create** button
5. Copy the value of **Application ID** which will be used in Finance and Operations to connect to PowerBI.com service
6. Click **Settings >> Required permissions >> Add >> Select an API >> Power BI Service (Power BI)**
7. Click **Select**.
8. Enable Access as below and click **Select**
    ![Azure Portal App Permissions](media/Azure-Portal-AppPermissions.png)

9. Now, Click **Done**
10. Click **Grant Permissions**.
11. Click **Settings >> Keys**.
12. Set value for **Key description** and **Expires**, and then Click **Save** button

Make a note of the **Application ID** and **Application Key**. You will use these values in the next procedure.

## Specify Power BI settings in Finance and Operations

1. In the Finance and Operations client, open the **Power BI configuration** page.
    ![Power BI configuration dialog](./media/D365-PBI-Configuration.png)

2. Select **Edit**.
3. Set the **Enabled** option to **Yes**.
4. In the **Application ID** field, enter the **Application ID** value that you got from Power BI in the previous procedure.
5. In the **Application Key** field, enter the **Application Key** value that you got from Power BI in the previous procedure.

    You can apply the company filter only if your Power BI content has a table that is named **Company** and a column that is named **ID**. Ready-made Power BI content that is released with Finance and Operations uses this convention.

6. Select **Save** button to apply changes

Now, use the following steps to verify the changes to enabled PowerBI.com integrations.

## Pin tiles to a workspace

1. To validate the PowerBI.com configuration, select **Get started** action.<br>
   **Note:** You may need to refresh the browser to apply the changes
    ![Authorize Power BI](./media/D365-PBI-GetStarted.png)

If you're starting Power BI from Finance and Operations for the first time, you're prompted to authorize sign-in to Power BI from the Finance and Operations client. Select **Click here to provide authorization to Power BI**.

Users must complete this step the first time they pin Power BI content.

4. The Azure AD consent page asks for your consent. User consent is required for Finance and Operations to access PowerBI.com on behalf of the user. Select **Accept**.

5. Because you're already signed in to Azure AD in Finance and Operations, you don't have to enter your credentials again. A new tab appears, where you're prompted to authorize the connection between Finance and Operations and Power BI. Authorize the connection, and then return to the original tab.

6. A list of tiles from your PowerBI.com account appears. Select one or more tiles to pin to the selected workspace.
    ![Validate Power BI integration](./media/D365-PBI-Validation.png)

## Troubleshooting common errors

After you select **Accept** in the previous procedure, you might receive the following error message if the process is unsuccessful. Notice that details of the error appear at the bottom of the message. Additional technical information provide clues that can help you determine what went wrong.

### Some common issues and the resolution steps

| Error                                                       | Resolution |
|-------------------------------------------------------------|------------|
| The Power BI service is unavailable.                        | This issue doesn't occur very often, but the Power BI service might sometimes be unreachable. You don't have to re-register. Try to pin a tile to a workspace later. |
| You can't access the application.                           | You probably didn't select all the check boxes under **Step 3 Choose APIs to access** during the registration process. Start Power BI, and rerun the registration process. |
| The Power BI tiles page is empty (no content is shown).     | Your PowerBI.com account might not have a dashboard or any tiles. Add a dashboard, such as a sample dashboard, and try to pin a tile again. |
| Error when Authorizing Power BI                             | Within the Azure Admin Dashboard, under **Users and Groups \> User settings**, make sure that the **Users can consent to apps accessing company data on their behalf** option is set to **Yes**. |

