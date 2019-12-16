# Incident Reporting

## Introduction
This is an application powered by the [Microsoft Power platform](https://powerplatform.microsoft.com/) and [SharePoint Online](https://products.office.com/en-us/sharepoint/collaboration) that allows users in your environment to report security incidents.

The application was built by and for nonprofits under the GNU FPLv3 license. This means you are allowed to do almost anything you want with the project, but you are not allowed to distribute closed source versions. For more information about the license of this project, view the license file of this project.

This application was made possible through funding from the [Dutch Relief Alliance Innovation Fund (DIF)](https://www.dutchrelief.org/) and the [Dutch Ministry of Foreign Affairs](https://www.government.nl/ministries/ministry-of-foreign-affairs).

This branch of the project is maintained by the IT department of the international relief and recovery organization [ZOA](https://www.zoa-international.com/).

This application is released without warranty, and the author cannot be held liable for any damages inflicted by the application. The author does not provide any official support for this project.
## Prerequisites

* Office 365 enterprise tenant
* User licenses that include at minimum (e.g. Office 365 E2 or E3):
    * Power Apps for Office 365
    * Power Automate for Office 365
* SharePoint Online
* Excel
* Google Chrome (Power Apps studio works most reliably on Chrome)

> Note: [Microsoft's licensing plans](https://docs.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq) may have changed by the time you are reading this. The requirements are minimum requirements; it may be possible to deploy this application if your organisation uses more extensive Power-platform licenses.

> Note 2: While [Power Apps does support connections to older SharePoint versions](https://powerapps.microsoft.com/en-us/blog/support-for-sharepoint-on-premises/), the solution was only tested on an SharePoint Online environment.

## Required admin skills
While this guide is meant to be as detailed as possible to allow anyone with good reading comprehension and admin permissions to deploy the application, it may be helpful to have some experience in the following:
* Administering SharePoint Online
  * Setting up and configuring team sites
  * Setting up and configuring lists
  * Creating SharePoint groups
  * Configuring SharePoint permissions
* Managing Power Apps applications
  * Installing a Power Apps from package
  * Setting up connections to SharePoint
* Managing Power Automate (Flow) automations
  * Installing Power Automate (Flow) from package
  * Setting up connections to SharePoint

> Note: If you want to do advanced configurations of the application, it is advisable to follow the free online [Microsoft Power Platform Fundamentals](https://docs.microsoft.com/en-us/learn/certifications/power-platform-fundamentals) course.

## Before you continue

While the application is free to deploy if you are already using the required licenses, there are some important things to consider before deploying.

1. This application has no official support channels
2. The Power Platform is a relatively young part of Microsoft 365, and not all aspects of it are mature
3. This application is fully dependent on Microsoft's continuation the SaaS applications Power Apps and Power Automate (Flow) and SharePoint integration with it
4. While making modifications to a Power Apps project requires less knowledge than making modifications to a traditional web application, many modifications to the app (from turning on/off features to adding/removing form questions) have to be done through Power Apps studio
5. Mid-tier smartphones struggle to render the input form quickly. This issue may be fixed in a future release, however with smartphones becoming more performant this issue may be resolved in a few years. Most laptops seem to render the form fine

## Features

### App features:
  * Create incident reports (both online and offline)
  * Submit incident reports (online only)
  * View own incident reports (both online and offline)
  * Auto-hide irrelevant questions
  * Switch between languages (online/offline)
  * Ring an emergency number
  * Send a SMS text message with a Google maps location link (in preview)
  * Set up in-app standard operating
precedures  (e.g. "What to do in case of fire...")
  * Viewing submitted incidents (both online and offline)
* Super user features (for security advisors):
  * View all reports
  * Close reports
  * Delete reports
* Administrative features (for IT admins, extends the super user features):
  * Set up special roles for security advisors, IT administrators, and country directors (national management staff).

  To install the application, read the [installation guide](InstallationGuide.md).
  For more information about the design of the application, read the [TechnicalDocumentation](TechnicalDocumentation).