# Incident Reporting Installation Guide

In order to deploy the application in your organisation you need to follow 1.1 through 2.3. Setting up a Power BI dashboard is recommended, but can be done after deploying the main application

> Note: Make sure you have read the readme before installing the application

Contents:
1. [Installation](#Installation)
    1. [Downloading the project from GitHub](#Downloading-the-project-from-GitHub)
    1. [Creating and setting up a SharePoint Site](#Creating-and-setting-up-a-SharePoint-Site)
    1. [Creating and setting up SharePoint Lists](#Creating-and-setting-up-SharePoint-Lists)
    1. [Setting up SharePoint groups and permissions](#Setting-up-SharePoint-groups-and-permissions)
    1. [Updating the database with your organisation's information](#Updating-the-database-with-your-organisation's-information)
    1. [Importing the Power App](#Importing-the-Power-App)
    1. [Importing the main Power Automate flow](#Importing-the-Power-Automate-flow)
    1. [Creating the App Admins flow](#Importing-the-App-Admins-flow)
1. [Assigning permissions](#Assigning-permissions)
    1. [Sharing access to the lists](#Sharing-access-to-the-lists)
    1. [Adding people to the App Admins list](#Adding-people-to-the-App-Admins-list)
    1. [Sharing the Power App](#Sharing-the-Power-App)
1. [Setting up a Power BI dashboard](#Setting-up-a-Power-BI-dashboard)

<br/><br/>
# Installation

It is advised to set up the SharePoint site, Power App, and Power Automate flow using a service account (e.g. automation@yourorganisation.org). This way the app is not connected to a regular user's account. Do not activate multi-factor authentication (MFA) on this service account, as MFA would require you to periodically activate your Power Automate flow connections

<br/><br/>
## Downloading the project from GitHub

1. Go to the [GitHub repository](https://github.com/ZOA-account/incident-reporting/) for the application
1. Click on **Clone or dowload** > **Download ZIP**. Extract the archive on your computer. We will need the files later in the process
    * question_database.stp
    * incident_database.stp
    * app_admins.stp
    * power_apps_application.zip
    * power_automate_flow.zip
    * app_admin_flow.zip

<br/><br/>
## Creating and setting up a SharePoint Site

1. Go to **https://\<yourtennant>-admin.sharepoint.com/_layouts/15/online/AdminHome.aspx#/siteManagement** > click on **Create** > **Other options**

> Note: Team sites with an Office 365 attached will not currently work with list templates. This is why we go through **Other options** to create a team site without an Office 365 group

2. Choose the **Team site** template and name it **Incident Reporting Admin**

1. Choose **English** as your primary language. If you choose another language, you will need to [edit the template file](https://developers.de/blogs/nadine_storandt/archive/2009/03/18/moss-how-to-change-list-site-template-language.aspx) but this solution is not tested by us

1. Under **Advanced settings** make sure to set your primary time zone, and click **Finish** to create the site

> Note: This SharePoint site will **not** be accesible for regular users after we set the permissions later on, and therefore you should not publish your PowerApps application on a modern page here, only your "databases." Hint: you might want to preserve names like "Incident Reporting" for a SharePoint site to publish the application on

<br/><br/>
## Creating and setting up SharePoint Lists

1. Go to **https://\<yourtennant>.sharepoint.com/_layouts/15/ManageFeatures.aspx**, find Team Collaboration Lists and click on **Activate** if it is not already active

1. Go to your **Incident Reporting Admin** team site in **SharePoint** (**https://\<yourtennant>.sharepoint.com/sites/incidentreportingadmin/**) > click the settings cogwheel > **Site Settings** or, if you are not seeing the button **Site Settings**, go the your site settings url: **https://\<yourtennant>.sharepoint.com/sites/IncidentReportingAdmin/_layouts/15/settings.aspx**

1. In the Web Designer Galleries column > click **List templates** and upload **app_admins.stp**, **incident_database.stp**, and **question_database.stp** via **FILES** > **Upload Document**

> Note: You have to be a site owner to see the **List templates** option

<!--1. Go to your new SharePoint team site in a browser. Click on the SharePoint settings cogwheel in the top right and click on **Add an app**. Search for "Import Spreadsheet" click on the results. Enter the name "Question Database" with the description "Questions and translations for the Incident Reporting App" and upload question_database.xlsx from "installationpackages" folder in the repositry. Do the same for the following
    * Name: "Incident Database", description: "List with all reported incidents", upload: incident_database.xlsx
    * Name: "App Admins", description: "List with permissions and roles", upload: app_admins.xlsx-->

4. Go to your site contents at **https://\<yourtennant>.sharepoint.com/sites/IncidentReportingAdmin/_layouts/15/viewlsts.aspx** and click on **New** > **App** > search for **incident** and install the three templates that you will find under the same name as the template (e.g. install template App Admins under the name App Admins)

> Note: If you have selected a language other than English as the main language of the team site, you will not see the list templates

> Note: you should be able to see the three lists appear in your site contents

<!--9. Open the **App Admin** list, click on the **cogwheel > List settings**

1. Below **Columns** click on the **Country** column and add your organisation's required countries to the list. Make sure you leave **All** as an option. We suggest using the [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) standard country codes, e.g.:
    * All
    * AF
    * GB
    * IN
    * NL
    * UG
    * YE
    * ZW
> Note: Do not change the entry name "All" above unless you have made changes to the Power App to reflect this -->

<br/><br/>
## Updating the database with your organisation's information

The **Question Database** SharePoint list stores all the translated text which the application uses. Before you deploy the application, you need to update this list to match your organisation's data. In the instruction below, we show how to update the country list, which is the a required change

1. Go to the **Question Database** list, and filter the column **Entity Type** by **CountryDropdown**

1. Edit the list with your country information. Make sure it matches the list created above, but do not add **All** here. E.g.:

Entity Category | Entity Type | Description | Code | English | French | Portuguese | Swahili | Arabic | Spanish
--- | --- | --- | --- | --- | --- |--- | --- |--- | ---
AnswerDropDown | CountryDropdown | | AF | Afghanistan | Afghanistan | | | أفغانستان |  |
AnswerDropDown | CountryDropdown | | GB | United Kingdom | Royaume-Uni | | | المملكة المتحدة |  |
AnswerDropDown | CountryDropdown | | IN | India | l'Inde | | | الهند |  |

> Note: We suggest using the [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) standard country codes. Fill only the translations for the languages you want to enable. The app is in English by default, and adding languages will involve translation work on your organisation's part

> Note: If you want to do bulk updates to your list, you may want to consider [exporting from the SharePoint list to an Excel file](https://support.office.com/en-us/article/export-to-excel-from-sharepoint-bfb2ea48-6118-4fa9-abb6-cced9424e5d9) and [importing it](https://sharepointmaven.com/3-ways-import-excel-sharepoint/) after editing. However, SharePoint's quick edit feature is usually sufficient

<br/><br/>
## Setting up SharePoint groups and permissions

> Note: The permission solution below gives the minimum required permissions for interacting with the Incident Database through PowerApps. It gives PowerApps users limited permissions to SharePoint, so they can access their incidents through the Incident Reporting app, but not through the SharePoint site/list

13. Go to https://\<yourtennant>.sharepoint.com/sites/IncidentReportingAdmin/_layouts/15/user.aspx > **Create Group** > and [create a SharePoint group (link to instruction)](https://docs.microsoft.com/en-us/sharepoint/customize-sharepoint-site-permissions#create-a-group) called **Incident Reporting Power Apps Users**. Make sure your service account is the group owner

1. Go to https://\<yourtennant>.sharepoint.com/sites/IncidentReportingAdmin/_layouts/15/user.aspx > click on **Permission Levels** > **Add a Permission Level**. Create two new permission levels, one called **Power Apps Users (site permission)** and one called **Power Apps Users (list permission)** at Incident Reporting site level, with the permissions as described below

### Permission levels
Permissions for *Power Apps Users (site permission)* | Permissions for *Power Apps Users (list permission)*
--- | ---
Open - Allow users to open a Web site, list, or folder in order to access items inside that container | Add Items - Add items to lists and add documents to document libraries
 -| Edit Items - Edit items in lists, edit documents in dicument libraries, and customize Web Part Pages in document libraries
 -| View Items - View items in lists and documents in document libraries
 -| View Pages - View pages in a Web site
 -| Open - Allow users to open a Web site, list or folder in order to access items inside that container

15. Go to https://\<yourtennant>.sharepoint.com/sites/IncidentReportingAdmin/_layouts/15/user.aspx > **Grant Permissions** > enter **Incident Reporting Power Apps Users** > **SHOW OPTIONS** > Uncheck **Send an email invitation** > Select permission level **Power Apps Users (sites permission)** > **Share**

1. Do the following for each of the SharePoint lists. Go to https://\<yourtennant>.sharepoint.com/sites/IncidentReportingAdmin/_layouts/15/viewlsts.aspx > click on the three option dots that appear next to the list when you hover over it > **Settings** > **Permissions for this list** > **Stop Inheriting Permissions** > **OK** > **Grant Permissions** > enter **Incident Reporting Power Apps Users** > **SHOW OPTIONS** > Uncheck **Send an email invitation** > Select permission level **Power Apps Users (list permission)** > **Share**. Make sure to select **Power Apps Users (list permission)** instead of **Power Apps Users (sites permission)**

> Note: See [Assigning permissions](#Assigning-Permissions) for more information on permissions

> Note: The access may be expanded if the user has additional permissions on the SharePoint. For people who can only use the PowerApp, use above permissions and make sure they do not have additional permissions

<br/><br/>
## Importing the Power App

1. Open [Power Apps studio](https://make.powerapps.com/), log in if necessary, click on **Apps** > **Import canvas app** > select and upload **power_apps_application.zip** from the downloaded repository

1. Wait for the upload process to complete

1. Click on the **Import** button

1. Before starting the application, you will need to update the connections between the application and SharePoint. You can do this from [the main Power Apps](https://make.powerapps.com/) page by clicking on the three dots next to the application > **Edit**

> Note: until you fix the connections between the application and SharePoint, text won't properly display in the application

5. If your browser asks for access to your location information, you must share it for the location sharing functionality to work. The location data is only used by the Power App, and is not stored anywhere

1. Click on **View** in the menu bar > **Data sources**

1. Remove the Incident Database, App Admins, and Question Database connections by hovering over the connectsion and clicking the thee dots > **Remove** 

1. Click **Add data source** > **New connection** > search for **SharePoint** and select it

1. Select the **Incident Reporting Admin** > select **App Admins**, **Incident Database**, and **Question Database** > click **Connect**

1. When prompted with: **The data source 'Incident Database' you are trying to add has the same name as an existing collection. Would you like to replace the existing collection? Any local data saved in the collection will be lost.** > click **Yes** and wait a short while for Power Apps studio to make the connections

1. Right click on **App** in the tree view > **Run OnStart**

1. If you have done everything correctly, you should see the text appear

1. Click on **File** > **Save** >  **Publish**

<br/><br/>
## Importing the main Power Automate flow

1. Open [Power Automate](https://emea.flow.microsoft.com/en-us/) and click on **My flows** > **Import** > select and upload **power_automate_flow.zip** from the downloaded repository

1. Wait for the upload process to complete

1. Below **IMPORT SETUP** click on **Select during import** for each of the three connections listed. Select a your own connection and click Save. If you do not see a connection listed, read the note below

* SharePoint Connection
* Office 365 Outlook Conncection
* Office 365 Users Connection

> Note: If you do not see any connections once you click on **Select during import**, first click on **Create new** and create the required connection. You can also find the **Connections** interface from Power **Automate > Connections**. You will have to have connections between the account that owns the Flow and Power App and these services: **SharePoint Connection, Office 365 Outlook Connection, Office 365 Users Connection**   

4. Click in **Import** and wait for the import to process

1. Click on **My flows**

1. Click on **New** > **Instant-from blank** > **Skip** (next to Create and Cancel)

1. Click on **Untitled** in the top left corner and type **Incident Reporting: New Item**

1. Click on **Search connectors and triggers** and search for **When an item is created**. Click on **When an item is created** in the results

1. In the trigger, select your Incident Reporting Admin site address and the list **Incident Database**

1. Open a new browser tab and go to https://emea.flow.microsoft.com/ > **My flows** > Select **Incident Reporting: Copy** and click on **Edit**

1. You will see 1 trigger (**When an item is created**) followed by 4 actions. For each of the actions (starting with **Initialize variable**, so skip the trigger in the top) click on the three dots in the top right > **Copy to my clipboard**

1. Once you have all 4 actions copied, go back to your other browser tab (with your **Incident Reporting: New Item** flow) and click on **New step** > **My clipboard** (next to the Premium and Custom tabs) > click on the first action you copied. Repeat this step until you have all 4 actions in the correct order in your new flow.

1. Click on the condition action (**Condition**) to open it. Click on **Get item (App Admins)** and update the **Site Address** to the Incident Reporting Admin site, and the **List Name** the **App Admins** SharePoint list

1. Go to https://www.utctime.net/ and scroll to the header **UTC Date and Time in Various Formats** > copy the UTC formatted time e.g. **2019-12-20T13:34:59Z**

1. Click on the action **Last Edited** and replace the time with the copied time. Make sure to remove any spaces that you may have accidentally added when pasting

1. Click **Save** to save

> Note: the flow uses the **App Admins** list to find the the Security Advisors and/or Country Directors. Make sure that this list is accurately filled in: [Adding people to the App Admins list](#Adding-people-to-the-App-Admins-list). If the App Admins list is empty, the application cannot inform the country director or security advisor.

<br/><br/>
## Creating the App Admins flow

Since it simpler to recreate this flow rather than import it, the steps below describe in detail how to create the flow 

1. Open Power Automate and click on **My flows** > **New** > **Instant-from blank** > **Skip**

1. Click on **Untitled** in the top left, and enter the name **Incident Reporting: Update Email from Admin List** as the name of the flow

1. Search for a trigger called **Recurrence** and select it, and set it to a 15 minute interval

1. Click on **New step** and search for and select the SharePoint action called **Get items**

1. Set the **https://\<yourtennant>.sharepoint.com/sites/IncidentReportingAdmin** as the Site Address, and set **App Admins** as the list

1. Click on **New step** and search for and select **Apply to each**, click in the text field next to **Select an output from previous steps** then select the **value** dynamic content that shows up on the right

1. Inside the **Apply to each** block, click on **Add an action** and search for and select the SharePoint action called **Update item**

1. Set the **https://\<yourtennant>.sharepoint.com/sites/IncidentReportingAdmin** as the Site Address, and set **App Admins** as the list. For the **Id** field, select the **ID** below **Get items** in the Dynamic content pop up screen

1. Inside the Update item block, click on **Show advanced options** amd set the **Email** field to be **User Email** (you can find this below Get items in the Dynamic content pop up)

1. Inside the Update item block, set the **DisplayName** field to be **User DisplayName** (you can find this below Get items in the Dynamic content pop up)

1. Click in **Save** in the top right corner to save the flow. You can now close the Power Automate browser tab

<br/><br/>
# Assigning permissions

> Note: make sure to test thoroughly before sharing the application with your colleagues

## Sharing access to the lists

1. Add any admins to the default SharePoint **Incident Reporting Admin Owners** group, and add security advisors to the default SharePoint **Incident Reporting Admin members** groups. These people can access the **Incident Reporting Admin** site and its lists through these permissions

1. [Add your end users (including country directors and programme managers) to the Power Apps Users SharePoint group **Incident Reporting Power Apps Users**](https://docs.microsoft.com/en-us/sharepoint/customize-sharepoint-site-permissions#add-users-to-a-group)

### Adding people to the App Admins list

1. Open the App Admin list with a global admin account and add a new item. Fill in the following information:

Title | Fill in
--- | ---
Email | your personal admin email address
Country | All
Role | ADMIN
User | admin account

2. Click on **save**

1. Open the Power Apps Incident Reporting Application with the admin account

1. Click on the people icon in the top right

1. Click **+** to assign your existing Office 365 users a role in the app

The **Country** dropdown reflects which country country management staff belong to, such as Uganda for the country director in the example below. For Admins, PGMs, and Security Advisors, the country field is ignored

The **Role** dropdown reflects the role of the user: ADMIN, CD (Country Director), DMT (Management Team), PGM (Programme Manager), or SA (Security Advisor)

> Note: Regular users do not need to be added to the App Admin list in order to access the application

<br/><br/>
## Sharing the Power App
1. Go to [Power Apps](https://make.powerapps.com/) and make sure you are signed in

1. Click on **Apps** > select the Incident Reporting app > **Share**
1. Select your users or user group
1. Uncheck **Send an email invitiation to new users** (it is always better to make users aware using a comprehensive explanatory email, Yammer, or Teams post) > click on **share**

<br/><br/>
# Setting up a Power BI dashboard

Read the [Power BI documentation](#PbiDocumentation.md) for more information