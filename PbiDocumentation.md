# Migration guide .pbit file to new tenant

By Ard Jan Koster

This document is a guide for setting up the Power BI dashboards provided with the Incident Reporting App.
There are a few steps that need to be taken in order to set up the reports in Power BI, as listed in the **Table of contents**.

## Table of contents


1. [Setting up Power BI](#Setting-up-Power-BI)

1. [Importing the report template](#Importing-the-report-template)
1. [Changing the data source](#Changing-the-data-source)
1. [Loading the data into the dashboard](#Loading-the-data-into-the-dashboard)
1. [Configuring details in the dashboard](#Configuring-details-in-the-dashboard)
1. [Saving and uploading the dashboard](#Saving-and-uploading-the-dashboard)
1. [Setting up the scheduled refresh](#Setting-up-the-scheduled-refresh)

These steps will be outlined in more detail below

## Setting up Power BI
When you have Power BI running in your organization, you can skip this part, and go to **Importing the report template**. When you don't have Power BI running, take the following steps. 
<br><br>**Note:** *You will need to decide who will run the dashboard. When you share a dashboard, a pro-license of Power BI is needed for the sharing user and receiving users of the dashboard! For more info, visit the [Power BI website.](https://powerbi.microsoft.com/en-us/pricing//)*<br>

1. Assign your service account the **Power BI (free/pro)** license

1. Install Power BI Desktop from the Microsoft Store (Windows) or [Power BI website](https://powerbi.microsoft.com/en-us/desktop/)
1. Open Power BI and log in with the service account

## Importing the report template
Before using the dashboards that are delivered with the incident reporting app, you need to import the template for the dashboards in PowerBI. To do this, take the following steps:
1. Open Power BI

1. In the upper left corner, click on the icon with the text **'File'**
1. In the menu that drops down, hover over the section with the text **'Import'**
1. In the window that pops up, click on the section with the text **'Power BI template'**
1. In the window that pops up, select the Power BI template that is provided with the installation files for the incident reporting app, called **'IncidentReportingTemplate'**
1. Click on the button with the text **'Open'**
1. In the window that pops up, click on the button with the text **'Cancel'**

<br><br>**Note:** *When you import the project template, the data refresh wil fail. This is because of the data connections within the Power BI template. The data connections in the dashboard will have to be changed after the step of importing the report template.* <br><br>

1. When the data refresh has finished, in the popped-up window, click on the button with the text **'Close'**

## Changing the data source
For changing the data source, take the following steps:
1. In the upper menu bar, click on the button with the text **'Edit Queries'**
<br><br> 
**Note:** *In the window that pops up (the Power Query Editor), take the following steps for each query, listed in the **Queries** section at the left side, exept the **'Measure table'** query*

1. In the **Queries** section at the left, click on the query that you want to edit
1. In the **APPLIED STEPS** section at the right, in the **Query Settings** steps section, double-clik on the step with the text *'Source'*
1. In the window that pops up, change the hyperlink of the sharepoint site to the *SharePoint site adress* that contains the lists and app of Incident Reporting
1. Pres the *'enter'* key

**Note:** *IF you haven't used the data source for the incidents in Power BI before, a window pops up that let's you sign in for the data source. You have to sign in once, then you can use the data source in Power BI.*

6. In the **APPLIED STEPS** section at the right, select the last step. Wait until the table has loaded.
<br><br>

**Note:** *If the table does not load or an error is returned, fill in the right sharepoint-site-adress, as needed in step 4*

7. Repeat these steps for each query in the **Queries** section at the left

**Note:** *When you have finished the steps mentioned above for all queries, you need to save the changes you have made to the queries in the dashboard. To do this, take the following steps:*
<br>

1. In the upper menu bar, click on the button with the text 'Close & Apply'

## Loading the data into the dashboard
When you have changed the data connections in your dashboard, you need to refresh the data source. To refresh the data in your dashboard, take the following steps:

1. In the upper menu bar, click on the button with the text **'Refresh'**

## Configuring details in the dashboard
When you have succesfully imported the dashboard and it's data, it is necessary to configure details in the dashboard. To do this, take the following steps:

1. Import your logo
    1. In the upper menu bar, click on the button with the text 'Image'

    1. In the popped-up screen, select an image of the logo of your company
    1. In the popped-up screen, click on the button with the text 'Open'
    1. When the image is imported, on all pages replace the image with the text 'Your Company Logo'

1. Change the dynamic text box
    1. In the **Fields** section at the right, select the **Measure table** table.

    1. In the **Measure table**, select the **Overview_Text_Box_I**
    1. Expand the formula bar, below the upper menu bar
    1. In the formula bar, change the text *'YourCompany'* to The name of your company

## Saving and uploading the dashboard
When you have succesfully configured the dashboard, and loaded your data into it, you are ready to save the dashboard. To do this, take the folowing steps:
1. In the upper menu, click on the button with the text 'File'

1. In the new menu that drops down, click on the button with the text 'Save As'
1. Save the Power Bi file on a location that you choose. 

Once you have saved the dashboard, you can upload it to your Power BI tenant. For more information on this proces, visit the [Power Bi Website.](https://docs.microsoft.com/en-us/power-bi/desktop-upload-desktop-files)<br>
To embed the dashboard on a SharePoint site, visit the [Power Bi Website.](https://docs.microsoft.com/en-us/power-bi/service-embed-report-spo)<br><br>

## Setting up the scheduled refresh

Once you have uploaded your Power BI dashboard, you need to configure the scheduled refresh of the data within the dashboard. To do this, take the following steps:

1. Sign in at the [Power BI website](https://powerbi.microsoft.com/en-us/)

1. Within the Power BI website, go the **workspace** wich has the dashboard deployed. The workspaces can be accessed via the **'Workspaces'** button, or the **'My workspace'** button in the left menu bar. 
1. Within the workspace, go to the **'Datasets'** tab
1. Within the 'Datasets' tab, under the **'ACTIONS'** text, click on the three dots
1. In the menu that pops up, click on the option with the text **'Settings'**
1. In the window that pops up, select the **'Scheduled refresh'** option
1. Select the slider with the text  above **'Keep your data up to date'**
1. From here, you can select options like the 'Refresh frequency', 'Time zone', etc. 

[To top](#Migration-guide-.pbit-file-to-new-tenant)