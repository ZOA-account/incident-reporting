# Technical Documentation Incident Reporting application
by Klaas de Jong

## Table of contents
1. [Introduction](#Introduction)
1. [Prerequisites](#Prerequisites)
1. [Required admin skills](#Required-admin-skills)
1. [Before you continue](#Before-you-continue), or: why you shouldn't deploy this
1. [Features](#Features)
1. [Technical design](#Technical-design)
1. [Broad overview](#Broad-overview) of the technical design
1. [Installation](#Installation)
1. [Permission levels](#Permission-levels)
1. [Power Apps definitions](#Power-Apps-definitions)
1. [Incident form](#Incident-form)
1. [Adding a question](#Adding-a-question)
1. [Removing a question](#Removing-a-question)
1. [To bottom](#Navigation-in-this-document)

## Introduction
This is an application powered by the [Microsoft Power platform](https://powerplatform.microsoft.com/) and [SharePoint Online](https://products.office.com/en-us/sharepoint/collaboration) that allows users in your environment to report security incidents.

The application was built by and for nonprofits under the GNU FPLv3 license. This means you are allowed to do almost anything you want with the project, but you are not allowed to distribute closed source versions. For more information about the license of this project, view the license file of this project.

This application was made possible through funding from the [Dutch Relief Alliance Innovation Fund (DIF)](https://www.dutchrelief.org/) and the [Dutch Ministry of Foreign Affairs](https://www.government.nl/ministries/ministry-of-foreign-affairs).

This branch of the project is maintained by the IT department of the international relief and recovery organization [ZOA](https://www.zoa-international.com/).

This application is released without warranty, and the author cannot be held liable for any damages inflicted by the application. The author does not provide any official support for this project.
## Prerequisites

> * Office 365 enterprise tenant
> * User licenses that include at minimum (e.g. Office 365 E2 or E3):
>   * Power Apps for Office 365
>   * Power Automate for Office 365
>   * SharePoint Online
> * Google Chrome (Power Apps studio works most reliably on Chrome)

> Note: [Microsoft's licensing plans](https://docs.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq) may have changed by the time you are reading this. The requirements are minimum requirements; it may be possible to deploy this application if your organisation uses more extensive Power-platform licenses.

While [Power Apps does support connections to older SharePoint versions](https://powerapps.microsoft.com/en-us/blog/support-for-sharepoint-on-premises/), the solution was only tested on an SharePoint Online environment.

## Required admin skills
While this guide is meant to be as detailed as possible to allow anyone with good reading comprehension and admin permissions to deploy the application, it may be helpful to have some experience in the following:
> * Administering SharePoint Online
>   * Setting up and configuring team sites
>   * Setting up and configuring lists
>   * Creating SharePoint groups
>   * Configuring SharePoint permissions
> * Managing Power Apps applications
>   * Installing a Power Apps from package
>   * Setting up connections to SharePoint
> * Managing Power Automate (Flow) automations
>   * Installing Power Automate (Flow) from package
>   * Setting up connections to SharePoint

If you want to configure the application in detail, it is advisable to follow the [Microsoft Power Platform Fundatmentals](https://docs.microsoft.com/en-us/learn/certifications/power-platform-fundamentals) course. A fee online course is available.

## Before you continue

While the application is free to deploy if you are already using the required licenses, there are some important things to consider before deploying.

1. This application has no official support channels
2. The Power Platform is a relatively young part of Microsoft 365, and not all aspects of it are mature
3. This application is fully dependent on Microsoft's continuation the SaaS applications Power Apps and Power Automate (Flow) and SharePoint integration with it
4. While making modifications to a Power Apps project requires less knowledge than making modifications to a traditional web application, many modifications to the app (from turning on/off features to adding/removing form questions) have to be done through Power Apps studio
5. Mid-tier smartphones struggle to render the input form quickly. This issue may be fixed in a future release, however with smartphones becoming more performant this issue may be resolved in a few years. Most laptops seem to render the form fine

## Features

> * App features:
>   * Create incident reports (both online and offline)
>   * Submit incident reports (online only)
>   * View own incident reports (both online and offline)
>   * Auto-hide irrelevant questions
>   * Switch between languages (online/offline)
>   * Ring an emergency number
>   * Send a SMS text message with a Google maps location link (in preview)
>   * Set up in-app standard operating
precedures  (e.g. "What to do in case of fire...")
>   * Viewing submitted incidents (both online and offline)
> * Super user features (for security advisors):
>   * View all reports
>   * Close reports
>   * Delete reports
> * Administrative features (for IT admins, extends the super user features):
>   * Set up special roles for security advisors, IT administrators, and country directors (national management staff).

# Technical design

## Broad overview
The application can broadly be divided into two parts, the **Power Platform** and **SharePoint Online**. As seen in the UML diagram below, those two parts can be devided into distict parts.

**Power Apps** is the front end application platform, which uses three tables (SharePoint lists) on SharePoint Online. It can be accessed through the web or through an authenticated session in the Power Apps app for Android, iOS, or Windows.

**Power Automate** (formerly Flow) handles email notifications for security advisors, country directors, and programme managers.

The **Incident Database** SharePoint list is used for storing the incidents. Access to this list is restricted. Regular users can only access their reports through the PowerApp, admins and security advisors can be given direct access by using standard SharePoint permissions.

The **App Admins** SharePoint list is used for setting up in-app permissions for admins, country directors, and security advisors. The PowerApp uses information in this list to ascertain which items any user has access to. Just like the Incident Database and the Question database, access to this list is restricted.

The **Question Database** contains all elements, including form questions, form answers, and user interface elements that change when a user changes the language. Just like the Incident Database and the App Admins list, access to this list is restricted.

![Broad overview of the solitions technical design](https://github.com/ZOA-account/incident-reporting/blob/master/pics/technicaldesign1.png)

The arrows represent the direction of the information flow; only the Power App writes to the Incident Database. From the perspective of the Power App, the other lists are read only.
<!--- Use following plantuml code to generate updated versions of this app (remove the <removethis> before doing so)

@startuml
cloud "Power Platform" {
[Power Apps]
[Power Automate]
}
database "SharePoint Online" {
[App Admins]
[Question Database]
[Incident Database]
}
[Power Apps] <-- [App Admins]
[Power Apps] <-- [Question Database]
[Power Apps] <-<removethis>-> [Incident Database]
[Power Automate] <-- [Incident Database]
[Power Automate] <-- [App Admins]
@enduml -->

## Installation

1. Create a [SharePoint team site (link to instruction)](https://support.office.com/en-gb/article/create-a-site-in-sharepoint-online-4d1e11bf-8ddc-499d-b889-2b48d10b1ce8) for hosting your SharePoint list "databases" and name it "Incident Reporting Admin". This SharePoint site will **not** be accesible for regular users after we set the permissions later on, and therefore you should not publish your PowerApps application on a modern page here, only your "databases." Hint: you might want to preserve names like "Incident Reporting" for a SharePoint site to publish the application on

1. Go to your new SharePoint team site in a browser. Click on the SharePoint settings cogwheel in the top right and click on **Add an app**. Search for "Import Spreadsheet" click on the results. Enter the name "Question Database" with the description "Questions and translations for the Incident Reporting App" and upload question_database.xlsx from "installationpackages" folder in the repositry. Do the same for the following
    * Name: "Incident Database", description: "List with all reported incidents", upload: incident_database.xlsx
    * Name: "App Admins", description: "List with permissions and roles", upload: app_admins.xlsx

> Note: The permission solution below gives the minimum required permissions for interacting with the Incident Database through PowerApps. It gives PowerApps users limited permissions to SharePoint, so they can access their incidents through the Incident Reporting app, but not through the SharePoint site/list

3. [Create a SharePoint group (link to instruction)](https://docs.microsoft.com/en-us/sharepoint/customize-sharepoint-site-permissions#create-a-group) called **Incident Reporting Power Apps Users** in the Incident Reporting Admin team site context

1. Create 2 new permission levels called **Power Apps Users (site permission)** and **Power Apps Users (list permission)** at Incident Reporting site level, with the permissions as described below here: [permission levels](#Permission-levels) for an overview of the permission levels

1. Assign t**Incident Reporting Power Apps Users** group the permission PowerApps Users (site permission) on site level

1. For each SharePoint lists that the app uses (App Admins, Incident Database, Question Database), [break permission inheritance (search linked page for "break permission inheritance" for instructions)](https://support.office.com/en-us/article/customize-permissions-for-a-sharepoint-list-or-library-02d770f3-59eb-4910-a608-5f84cc297782) and assign the **Incident Reporting Power Apps Users** group the permission **Power Apps Users (list permission)**

1. Add any admins to the default SharePoint **Incident Reporting Admin Owners** group, and add security advisors to the default SharePoint **Incident Reporting Admin members** groups. These people can access the **Incident Reporting Admin** site and its lists through these permissions.

1. [Add your end users/groups, including programme managers and country directors, to the **Power Apps Users group**](https://docs.microsoft.com/en-us/sharepoint/customize-sharepoint-site-permissions#add-users-to-a-group)

> Note: The access may be expanded if the user has additional permissions on the SharePoint. For people who can only use the PowerApp, use above permissions and make sure they do not have additional permissions. 

9. Import the PowerApp

1. Import the Power Automate flow

1. Nog een punt

## Permission levels
Permissions for *Power Apps Users (site permission)* | Permissions for *Power Apps Users (list permission)*
--- | ---
"Open - Allow users to open a Web site, list, or folder in order to access items inside that container" | "Add Items - Add items to lists and add documents to document libraries"
 -| "Edit Items - Edit items in lists, edit documents in dicument libraries, and customize Web Part Pages in document libraries"
 -| "View Items - View items in lists and documents in document libraries"
 -| "View Pages - View pages in a Web site"
 -| "Open - Allow users to open a Web site, list or folder in order to access items inside that container"

1. [Import the Incicident Reproting package (link to instruction)](https://docs.microsoft.com/en-us/power-platform/admin/environment-and-tenant-migration#importing-a-canvas-app) and make sure to

1. 

## Power Apps definitions
Definition | Explanation
--- | ---
Application | A Power Apps application that is built up of an "App" object, which contains "screen" objects, which contain user interface elements. All logic of the application itself resides in the properties of each user interface element
Property | A charactaristic or behaviour of a user interface element, e.g. the height of an element, the text to display, or an action to take when the element is clicked
Screen | A screen in the application, which is populated by user interface elements such as controls and labels
User interface element | Any object that is visible in the graphical user interface of the application, such as a  control, label, 
Control (input) | A user input element, such as a text field or a dropdown
Label | A text label which can contain static or dynamic data
Formula | The "code" or programming language of PowerApps. It is similar in syntax to the Excel formula language. Formulas are written in the properties of user interface elements.
Gallery| A userinterface element which can display a list of items, such as the items from a SharePoint list
Form | A userinterface element which contains "cards"
Card | An element of a form which maps to a database column, i.e. a SharePoint column. In our the incident reporting form, each card represents a question in the form. When a form is submitted, the PowerApp submits the code that is in the OnUpdate property of the card 
OnUpdate | A property of a card which  the action to take when a form is submitted
OnStart | A property of the application which is run every time the app starts. For instance, it updates the question list if the app is connected to the internet
Tree view | An overview of all elements in your project. "App", which contains the OnStart-property, will always be on top, followed by the various screens of the application. Click on the screens to open them and view the user interface elements that it contains such as groups, buttons, input controls, etc.


## On start
Every time the app starts, it will run the code in the "on start" property in the App.

## Incident form
The incident form uses the standard Power Apps form control. 

### Navigation in this document
[To top](#Technical-Documentation-Incident-Reporting-application)
