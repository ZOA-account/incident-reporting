# Incident Reporting Installation Guide

Contents:
1. [Installation](#Installation)
    1. [Downloading the project from GitHub](#Downloading-the-project-from-GitHub)
    1. [Creating a SharePoint Site and Lists](#Creating-a-SharePoint-Site-and-Lists)
    1. [Importing the Power App](#Importing-the-Power-App)
    1. [Importing the Power Automate flow](#Importing-the-Power-Automate-flow)
1. [Assigning permissions](#Assigning-permissions)
    1. [Sharing access to the lists](#Sharing-access-to-the-lists)
    1. [Adding people to the App Admins list](#Adding-people-to-the-App-Admins-list)
    1. [Sharing the Power App](#Sharing-the-Power-App)

Note: Make sure you have read the readme before installing the application.

## Installation

> Note: it is advised to set up the SharePoint site, Power App, and Power Automate flow using a service account (e.g. automation@yourorganisation.org). This way the app is not connected to a regular user's account. Do not activate multi-factor authentication (MFA) on this service account, as MFA would require you to periodically activate your Power Automate flow connections

### Downloading the project from GitHub

1. Go to the [GitHub repository](https://github.com/ZOA-account/incident-reporting/) for the application
1. Click on **Clone or dowload** > **Download ZIP**. Extract the archive on your computer. We will need the files later in the process.
    * question_database.stp
    * incident_database.stp
    * app_admins.stp
    * power_apps_application.zip
    * power_automate_flow.zip

### Creating a SharePoint Site and Lists

1. Go to https://\<yourtennant>-admin.sharepoint.com/_layouts/15/online/AdminHome.aspx#/siteManagement > click on **Create** > **Other options**.

1. Choose the **Team site** template and name it **Incident Reporting Admin**.

1. Choose **English** as your primary language. If you choose another language, you will need to [edit the template file](https://developers.de/blogs/nadine_storandt/archive/2009/03/18/moss-how-to-change-list-site-template-language.aspx) but this solution is not tested by us

1. Under **Advanced settings** make sure to set your primary time zone, and click **Finish** to create the site

> Note: Team sites with an Office 365 attached will not currently work with list templates. This is why we go through **Other options** to create a team site without an Office 365 group.

> Note: This SharePoint site will **not** be accesible for regular users after we set the permissions later on, and therefore you should not publish your PowerApps application on a modern page here, only your "databases." Hint: you might want to preserve names like "Incident Reporting" for a SharePoint site to publish the application on

5. Go to https://\<yourtennant>.sharepoint.com/_layouts/15/ManageFeatures.aspx, find Team Collaboration Lists and click on **Activate** if it is not already active

1. Go to your **Incident Reporting Admin** team site in **SharePoint** (/sites/incidentreportingadmin/) > click the settings cogwheel > **Site Settings** or, if you are not seeing the button **Site Settings**, go the your site settings url: https://\<yourtennant>.sharepoint.com/sites/IncidentReportingAdmin/_layouts/15/settings.aspx

1. In the Web Designer Galleries column > click **List templates** and upload **app_admins.stp**, **incident_database.stp**, and **question_database.stp** via **FILES** > **Upload Document**

> Note: You have to be a site owner to see the **List templates**

<!--1. Go to your new SharePoint team site in a browser. Click on the SharePoint settings cogwheel in the top right and click on **Add an app**. Search for "Import Spreadsheet" click on the results. Enter the name "Question Database" with the description "Questions and translations for the Incident Reporting App" and upload question_database.xlsx from "installationpackages" folder in the repositry. Do the same for the following
    * Name: "Incident Database", description: "List with all reported incidents", upload: incident_database.xlsx
    * Name: "App Admins", description: "List with permissions and roles", upload: app_admins.xlsx-->

8. Go to your site contents at **https://\<yourtennant>.sharepoint.com/sites/IncidentReportingAdmin/_layouts/15/viewlsts.aspx** and click on **New** > **App** > search for **incident** and install the three templates that you will find under the same name as the template (e.g. install template App Admins under the name App Admins)

> Note: If you have selected a language other than English as the main language of the team site, you will not see the list templates

> Note: you should be able to see the three lists appear in your site contents

9. Open the **App Admin** list, click on the **cogwheel > List settings**

1. Below **Columns** click on the **Country** column and add your organisation's required countries to the list. Make sure you leave **All** as an option. We suggest using the [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) standard country codes, e.g.:
    * All
    * AF
    * GB
    * IN
    * NL
    * UG
    * YE
    * ZW
> Note: Do not change the entry name "All" above unless you have made changes to the Power App to reflect this

11. In the **Question Database** list, click on the **cogwheel > List settings**

1. Instead of going to list settings, filter the column **Entity Type** by **CountryDropdown**

1. Edit the list with your country information. Make sure it matches the list created above, but do not add **All** here. E.g.:

Entity Category | Entity Type | Description | Code | English | French | Portuguese | Swahili | Arabic | Spanish
--- | --- | --- | --- | --- | --- |--- | --- |--- | ---
AnswerDropDown | CountryDropdown | | AF | Afghanistan | Afghanistan | | | أفغانستان |  |
AnswerDropDown | CountryDropdown | | GB | United Kingdom | Royaume-Uni | | | المملكة المتحدة |  |
AnswerDropDown | CountryDropdown | | IN | India | l'Inde | | | الهند |  |

> Note: We suggest using the [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) standard country codes, but whatever you choose make sure it matches the choice in the **App Admin Country** column. Fill only the translations for the languages you want to enable. The app is in English, and adding languages will involve translation work on your organisation's part

14. [Create a SharePoint group (link to instruction)](https://docs.microsoft.com/en-us/sharepoint/customize-sharepoint-site-permissions#create-a-group) called **Incident Reporting Power Apps Users** in the Incident Reporting Admin team site context

1. Create 2 new permission levels called **Power Apps Users (site permission)** and **Power Apps Users (list permission)** at Incident Reporting site level, with the permissions as described below

> Note: The permission solution below gives the minimum required permissions for interacting with the Incident Database through PowerApps. It gives PowerApps users limited permissions to SharePoint, so they can access their incidents through the Incident Reporting app, but not through the SharePoint site/list

#### Permission levels
Permissions for *Power Apps Users (site permission)* | Permissions for *Power Apps Users (list permission)*
--- | ---
Open - Allow users to open a Web site, list, or folder in order to access items inside that container | Add Items - Add items to lists and add documents to document libraries
 -| Edit Items - Edit items in lists, edit documents in dicument libraries, and customize Web Part Pages in document libraries
 -| View Items - View items in lists and documents in document libraries
 -| View Pages - View pages in a Web site
 -| Open - Allow users to open a Web site, list or folder in order to access items inside that container

11. Assign the **Incident Reporting Power Apps Users** group the permission **PowerApps Users (site permission)** on site level

1. For each SharePoint lists that the app uses (App Admins, Incident Database, Question Database), [break permission inheritance (search linked page for "break permission inheritance" for instructions)](https://support.office.com/en-us/article/customize-permissions-for-a-sharepoint-list-or-library-02d770f3-59eb-4910-a608-5f84cc297782) and assign the **Incident Reporting Power Apps Users** group the permission **Power Apps Users (list permission)**

> Note: See [Assigning permissions](#Assigning-Permissions) for more information on permissions

> Note: The access may be expanded if the user has additional permissions on the SharePoint. For people who can only use the PowerApp, use above permissions and make sure they do not have additional permissions. 

### Importing the Power App

1. Open [Power Apps studio](https://make.powerapps.com/) click on **Apps** > **Import canvas app** > select and upload **power_apps_application.zip** from the downloaded repository

1. Make sure that under **IMPORT SETUP** "Create as new" is selected; optionally rename your application here
1. Under **Related resources** you will find a SharePoint connection. Under **Select during import** select your admin or service account to make the connection to these resources.
> If you do not see any connections, first click on **Create new** and create the required connection. You can also find the **Connections** interface from Power **Automate > Connections**. You will have to have connections between the account that owns the Flow and Power App and these services: **SharePoint Connection, Office 365 Outlook Connection, Office 365 Users Connection**, but the Power App will only use SharePoint Connection.    
4. Click on **Import** to import the application

1. After a while the application will be installed
1. Find the application under **Apps** > click on it > **Edit**
1. Click on **View > Data sources** and remove App Admins, Incident Database, and Question Database.
1. Click on **Add data source > SharePoint** and connect your own SharePoint lists to your app: Admins, Incident Database, and Question Database.
1. Once you have set up these lists, right click on **App** in the tree view > **Run OnStart**
1. If you have done everything correctly, you should see the text appear.
1. Click on **File** > **Save** >  **Publish**
### Importing the Power Automate flow

1. Open [Power Automate](https://emea.flow.microsoft.com/en-us/) and click on **My flows** > **Import** and select/upload **power_automate_flow.zip** from the downloaded repository

1. [Import the Incicident Reporting package (link to instruction)](https://docs.microsoft.com/en-us/power-platform/admin/environment-and-tenant-migration#importing-a-canvas-app)
1. Make sure that under **IMPORT SETUP** "Create as new" is selected; optionally rename your flow here
1. Under **Related resources** you will find a your SharePoint, Outlook, Office 365 Users-connections. Under **Select during import** select your admin or service account to make the connection to these resources.
> If you do not see any connections, first click on **Create new** and create the required connection. You can also find the **Connections** interface from Power **Automate > Connections**. You will have to have connections between the account that owns the Flow and Power App and these services: **SharePoint Connection, Office 365 Outlook Connection, Office 365 Users Connection**    
> You may have to do this for each of the three connections.
4. Click on **Import** to import the flow

1. Find the flow under **My flows** > click on it > **Edit**
1. Set the **Site Address** and **List Name** to your location of the **Incident Database**
1. Update the action that is called **Before turning flow on, update time in input below: www.utctime.net** with your current time. Remember to update this time every time you turn the flow on after it has been turned of, otherwise Flow will see all items in the list as new items. You can find the UTC time format at [this website](https://www.utctime.net/).
1. Click on **Condition** to open it, then click to open **If yes**
1. In the **If yes** find the **Get items (App Admins)** and set the **Site Address** and **List Name** to your location of the **App Admins** list
1. When you are ready, click on Save. Check if the flow is turned on: if not, turn it on.

## Assigning permissions

> Note: make sure to test thoroughly before sharing the application with your colleagues

### Sharing access to the lists

1. Add any admins to the default SharePoint **Incident Reporting Admin Owners** group, and add security advisors to the default SharePoint **Incident Reporting Admin members** groups. These people can access the **Incident Reporting Admin** site and its lists through these permissions.

1. [Add your end users (including country directors and programme managers) to the Power Apps Users SharePoint group **Incident Reporting Power Apps Users**](https://docs.microsoft.com/en-us/sharepoint/customize-sharepoint-site-permissions#add-users-to-a-group)

### Adding people to the App Admins list
During the installation, you have set up the countries and roles in the App Admin list. Now it is time to apply them

The **Email** column must be filled with the exact email address of the user. It may be possible to upgrade the Power App to work with a SharePoint person field, but that functionality does not currently work with the current version of the Power App.

The **Country** column reflects which country country management staff belong to, such as Uganda for the country director in the example below. For the admin and security advisor, choose **All** to make sure they have access to all incidents in the Power App.

The **Role** column reflects the specific user role. It is not strictly necessary to register PGMs, but the permissions of all other roles are managed through this list.

#### Example:

Email | Country | Role
--- | --- | ---
r.hauer@contoso.ngo | UG | CD
f.janssen@contoso.ngo | All | ADMIN
a.hepburn@contoso.ngo | All | SA
c.vanhouten@contoso.ngo | All | DMT
m.huisman@contoso.ngo | UG | PGM

### Sharing the Power App
1. Go to [Power Apps](https://make.powerapps.com/)

1. Click on **Apps** > select the Incident Reporting app > **Share**
1. Select your users or user group
1. Uncheck **Send an email invitiation to new users** > click on **share**