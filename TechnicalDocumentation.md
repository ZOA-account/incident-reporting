# Technical Documentation Incident Reporting application
by Klaas de Jong

## Table of contents

1. [Technical design](#Technical-design)
    1. [Introduction](#Introduction) to the technical design
    1. [Justification for design choices](#Justification)
1. [Security](#Permissions)
    1. [Disclaimer](#Disclaimer)
    1. [Permission levels](#Permission-levels)
1. [Administration](#Administration)
    1. [Power Apps definitions](#Power-Apps-definitions)
    1. [Incident form](#Incident-form)
    1. [Adding a question](#Adding-a-question)
    1. [Removing a question](#Removing-a-question)
    1. [To bottom](#Navigation-in-this-document)

# Technical design

## Introduction
The application can broadly be divided into two parts, the **Power Platform** and **SharePoint Online**. As seen in the diagram below, those two parts can be devided into distict parts. The Power Platform runs the application, SharePoint Online acts as the database

**Power Apps** is the front end application platform, which uses three tables (SharePoint lists) on SharePoint Online. It can be accessed through the web or through an authenticated session in the Power Apps app for Android, iOS, or Windows

**Power Automate** (formerly Flow) handles email notifications for security advisors, country directors, and programme managers

The **Incident Database** SharePoint list is used for storing the incidents. Access to this list is restricted. Regular users can only access their reports through the PowerApp, admins and security advisors can be given direct access by using standard SharePoint permissions

The **App Admins** SharePoint list is used for setting up in-app permissions for admins, country directors, and security advisors. The PowerApp uses information in this list to ascertain which items any user has access to. Just like the Incident Database and the Question database, access to this list is restricted

The **Question Database** contains all elements, including form questions, form answers, and user interface elements that change when a user changes the language. Just like the Incident Database and the App Admins list, access to this list is restricted

![Broad overview of the applications' technical design](https://github.com/ZOA-account/incident-reporting/blob/master/pics/technicaldesign1.png)

The arrows represent the direction of the information flow; only the Power App writes to the Incident Database. From the perspective of the Power App, the other lists are read only
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

## Justification

The design of the application has a few central requirements:
1. Affordability
1. Support multiple languages
1. Support offline functionality

### Affordability
Many of the organisations that this application was targeted for already have access to Office 365 E1/E2/E3 licences which gives them access to SharePoint Online and the Power platform. For those organisations, the installation of this app will not require additional licenses fees. Because Microsoft's Common Data Service is not included in PowerApps for Office 365, SharePoint lists will be used to store data

### Support multiple languages

To enable multiple languages we have opted to do a lookup from Power Apps to a locally cached copy (collection) of the the **Question Database** SharePoint list for user interface text elements and the main form's questions and dropdown answers. This choice does hurt the performance of the application, especially in the incident form screen. A solution to this performance hit may come in an update

### Offline functionality

To enable offline functionality, the app is designed to copy the contents of the three connected SharePoint lists to the local storage of the phone. The ability to create incidents while offline is currently in preview, and may be buggy. It is not possible to upload attachments while offline

# Security

## Disclaimer

For any security application, security, reliability, and confidentiality are critical. The entire solution is built within Microsoft's Office 365 environment, and will be installed entirely inside your tennant. This means that the creators of this project do not have access to your data

The application uses Power Apps, Power Automate, and SharePoint Online, which are all part of Office 365, which is a SaaS-solution, so you do not have to update those applications. Microsoft will do this for you

However, keep in mind that this application is released without warranty, and the author cannot be held liable for any damages inflicted by the application

## Permission design

If you follow the installation guide closely, you will have a SharePoint site which is only accessible to security advisors and dedicated admins

End users, including country directors and programme managers will only have access through the power app. The the table [**Access overview**](#Access-overview) and [**Permissions per incident status**](#Permissions-per-incident-status) give an overview of what the default permissions give access to

### Access overview:
- | Access to SharePoint (lists) | Access to own incidents in app | Access to all country incidents in app | Access to incididents in app if mentioned as PGM
--- | --- | --- | --- | ---
**End user** | No | Yes | No | Yes
**Country Director** | No | Yes | Yes | Yes
**Programme Manager** | No | Yes | No | Yes
**Security Advisor** | Yes | Yes | Yes | Yes
**Admin** | Yes | Yes | Yes | Yes | Yes | Yes

### Permissions per incident status:

- | Open | Finalised by CD | Closed
--- | --- | --- | ---
**End user (own tickets only)** | Read/Write | Read | Read
**Country Director** | Read/Write | Read (CD can change status to Finalised, but cannot change it back)| Read
**Programme Manager** | Read/Write | Read | Read
**Security Advisor** | Read/Write | Read/Write | Read/Write
**Admin** | Read/Write | Read/Write | Read/Write

### Permission levels

Two custom SharePoint permissions are used with the permissions in the overview below. Through these SharePoint groups, regular users, country directors, and PGMs will not have access to the SharePoint lists other then through the Power Apps application

Permissions for *Power Apps Users (site permission)* | Permissions for *Power Apps Users (list permission)*
--- | ---
Open - Allow users to open a Web site, list, or folder in order to access items inside that container | Add Items - Add items to lists and add documents to document libraries
 -| Edit Items - Edit items in lists, edit documents in dicument libraries, and customize Web Part Pages in document libraries
 -| View Items - View items in lists and documents in document libraries
 -| View Pages - View pages in a Web site
 -| Open - Allow users to open a Web site, list or folder in order to access items inside that container

## Administration

### Power Apps Definitions
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
