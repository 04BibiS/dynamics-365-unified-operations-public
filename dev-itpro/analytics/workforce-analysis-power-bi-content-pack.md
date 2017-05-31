---
# required metadata

title: Workforce metrics Power BI content
description: This topic describes the Dynamics 365 for Operations - Workforce Metrics Power BI content. It explains how to access the reports that are included in the content pack, and provides information about the data model and entities that were used to build the content pack.
author: twheeloc
manager: AnnBe
ms.date: 04/04/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-platform
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Application User, IT Pro
# ms.devlang: 
# ms.reviewer: 71
ms.search.scope: Operations
# ms.tgt_pltfrm: 
ms.custom: 264084
ms.assetid: 8e700583-3a7d-4f5f-9ac8-58c4feed1a02
ms.search.region: Global
# ms.search.industry: 
ms.author: jcart
ms.search.validFrom: 2016-11-30
ms.dyn365.ops.version: Version 1611

---

# Workforce metrics Power BI content

[!include[banner](../includes/banner.md)]


This topic describes the **Workforce metrics** Power BI content. It explains how to access the Power BI reports, and provides information about the data model and entities that were used to build the content.

## Accessing the Power BI content

Embedded Power BI allows individuals in your organization to take advantage of analytics out of the box. They can quickly analyze your data using the provided reports and visuals without having to model the data or create reports. You can also provide great analytics to those who do not log into Dynamics 365 by using the content packs available on Lifecycle Services (LCS). These content packs can be modified to include other reports or visuals, then published to your Power BI.com tenant for analysis. 

### Embedded content
If you're using Dynamics 365 for Finance and Operations, Enterprise edition, the **Workforce metrics** Power BI content is displayed in the **Personnel management** workspace.

### Content accessible from Lifecycle Services
If you are using Dynamics 365 for Operations version 1611 or Dynamics 365 for Finance and Operations, Enterprise edition July 2017 update, you can find the **Workforce metrics** Power BI content in the Shared assets library in LCS. For more information about how to download the content and implement it in your organization, see [Power BI content in LCS from Microsoft and your partners](power-bi-content-microsoft-partners.md). To watch a demo that shows how to implement the Power BI content, see the [Power BI content from Microsoft and your partners in Dynamics Lifecycle Services](https://mix.office.com/watch/9puyb1b2xs1w) Office Mix.

Be sure to download the **Workforce metrics** content that applies to the version of Dynamics 365 that you're using.


## Reports that are included in the Power BI content
The following table lists the metrics shown on each report.

| Report                                           | Metrics                                                                                                                                                                                                            |
|--------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| People Metrics                                   | Summary of other reports                                                                                                                           |
| Headcount Analysis Company, Department, Location | Headcount by company, headcount by department, headcount by location, and total headcount                                                                                                                           |
| Headcount Analysis Job, Step, Manager            | Headcount by job, headcount by step, headcount by manager, and total headcount                                                                                                                                      |
| Headcount Trend Analysis                         | Headcount this year versus last year by company and rolling headcount for the last 12 months                                                                                                                        |
| FTE Analysis                                     | Total FTE, Total assigned FTE, FTE by department, FTE for the last 12 months, FTE by job |
| Workforce Demographics                           | Headcount by age and gender, headcount by ethnic origin, headcount by veteran status, headcount by marital status, number of full-time students, average tenure, average age, ratio of female to male employees, languages spoken by employees |
| Position Analysis                                | Open positions by department, open-to-filled positions, active-to-inactive positions, and positions by department                                                                                                   |
| Attrition Analysis                               | Attrition this year versus last year, attrition, exiting employees by age and gender, average tenure of employees leaving, employees exiting this month, and employees leaving by reason                                                                   |
| People by department                             | Employees with a personnel number by department, position, and assignment start and end dates                                                                                                                       |
| Seniority Analysis                               | Average tenure, average years of service by company and seniority list                                                                                                                                                              |
| Employee Anniversaries                           | Anniversaries this month, anniversaries next month, employees by years of service, and anniversaries, years of service by department                                                                                                                                                                    |
| Employee Birthdays                               | Birthdays this month, birthdays next month, employee birthdays, birthdays by month and department                                                                                                                                                                    |
| Mass Hire Projects                               | Total mass hire projects, mass hire projects by status, mass hire projects by department and owner, mass hire projects by job, mass hire projects                                                                                                                                                                    |

You can filter the charts and tiles on these reports, and pin the charts and tiles to the dashboard. For more information about how to filter and pin in Power BI, see [Create and configure a dashboard](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-4-2-create-configure-dashboards).

## Understanding the data model and entities
The following table shows the entities that the content pack was based on.

| Entity                            | Contents                                                                                                   | Relationships with other entities                                                                                                                                                                                                                                                                                                |
|-----------------------------------|------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Calendar Offset         | Calendar offsets to slice reports                                                                          | Past Position Assignment Position Trend Employee Trend Terminated Employee                                                                                                                                                                                                                     |
| Company                | Companies to filter reports by                                                                             | Current Employee Terminated Employee Employee Trend                                                                                                                                                                                                                        |
  | Current Position                | Positions as of the current date, full-time equivalent (FTE), open positions, and open-to-filled positions                                                                             | Job Position                                                                                                                                                                                                                        |                                                                                                          |
| Current Employee          | Workers as of the current date, age, and headcount                                                         | Company Geographic Location  Employee Name Reports To Employee Title Demographics Job Employment Position                                             |
| Date                   | Days, weeks, months, and years                                                                             | Past Position Assignment Position Trend Terminated Employee Employee Trend                                                                                                                                                                                                                     |
| Demographics           | Date of birth, gender, ethnic origin, and marital status                                                   | Current Employee Terminated Employee Employee Trend                                                                                                                                                                                                                                                      |
| Employment             | Start date, end date, and transition date                                                                  | Current Employee Terminated Employee Employee Trend                                                                                                                                                                                                                                                       |
| Geographic Location     | City, county, postal code, and state or province                                                           | Current Employee Terminated Employee Employee Trend                                                                                                                                                                                                                                                       |
| Job                    | Function, type, and title                                                                                  | Current Position Current Employee                                                                                                                                                                                                                                                                              |
| Past Position Assignment | Assignment reason, start date, end date, and job                                                           | Calendar Offset Date Job Position                                                                                                                                                                                           
| Position               | Department, FTE, position, position type, and title                                                        | Current Position Current Employee                                                                                                                                                                                                                                                                              |
| Position Trend          | Positions over time, FTE, and job                                                                          | Calendar Offset Date Job Position                                                                                                                                                                                                                                                     |
| Reports To     | First name, last name, and full name                                                                       | Current Employee Terminated Employee Employee Trend                                                               
| Terminated Employee       | Terminated workers, termination date, title, position, and job                                             | Company Geographic Location Employee Name Reports To Calendar Offset Date Employee Title Demographics Employment Job Position  |
| Employee Name            | First name, last name, and full name                                                                       | Current Worker Terminated Employee Employee Trend                                                                                                                                                                                                                        |
| Employee Title           | Title and seniority date                                                                                   | Current Employee Terminated Employee Employee Trend                                                                                                                                                                                                                                                       |
| Employee Trend          | Workers over time, headcount, company, and position                                                        | Company Geographic Location Employee Name Reports To Calendar Offset Date Employee Title Demographics Employment Job                 |
| Mass Hire Project          | Number of mass hire projects, project owner, project status                                                       | Company Mass Hire Project Line                 |
| Mass Hire Line          |   Department, employment type, position                                                     | Date, Job Mass Hire Project                 |

These entities were used to create calculated measures in the data model. These calculated measures are then used to calculate the key performance indicators (KPIs) and reports that are used in the content pack. If you want to include additional calculations on your reports and dashboard, you can download and modify the CompensationandBenefits.pbix file from LCS. This file is the default data model that was used to create the content pack. After you've made modifications, you can create an organizational content pack and dashboard that contain the information that you’ve added.

## Additional resources
Here are some helpful links that are related to entities and building Power BI content:

-   [Data entities](https://blogs.msdn.microsoft.com/dynamicsaxbi/2016/06/09/power-bi-integration-with-entity-store-in-dynamics-ax-7-may-update/)
-   [Creating organizational content packs](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-organizational-content-packs-introduction/)
-   [Data modeling using Power BI](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-2-1-intro-modeling-data)
-   [Adding Power BI tiles to workspaces](https://blogs.msdn.microsoft.com/dynamicsaxbi/2016/07/06/pinning-power-bi-reports-to-dynamics-ax-client/)




