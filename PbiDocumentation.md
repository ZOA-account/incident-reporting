# Migration guide .pbix file to new tenant

This document is a guide for setting up the Power BI dashboards provided with the Incident Reporting App.
There are a few steps that need to be taken in order to set up the reports

These steps are: 
1. Changing the data source

1. Signing in to the new data source
1. Refresh data sources

These steps wil be outlined in more detail below

## Setting up

1. Assign your service account the **Power BI (free)** license

1. Install Power BI Desktop from the Microsoft Store (Windows)
1. Open Power BI and log in with the service account
1. 

## Changing the data source
For changing the data source, please take the following steps:
1. Open the Power BI file (a .pbix file) from the **installationfiles** folder

1. Click in the upper left corner on the icon with the text 'File'
1. In the menu that drops down, hover over the section with the text 'Options and settings'
1. In the pane on the right, click on the section with the text 'Data source settings'
1. In the window that pops up, click the button 'Change source...'
1. In the window that pops up, fill in the SharePoint site adress
1. In the popped up window, click on the 'OK' button

## Signing in to the new data source
When you have changed the data source, the next step is tho sign in tho the new data source. To do this, please take the following steps:
1. In the last screen of the following step (named 'Data source settings'), click the button with the text 'Edit permissions'

1. In the window that pops up, click the button with the text 'Edit...'
1. In the window that pops up, click the button with the text 'Sign in as a different user'
1. In the window that pops up, click on the field with the text 'Use antoher account'
1. In the new screen, fill in your Email 
1. Click the button with the text 'Next'
1. In the new screen, fil in your password
1. Click the button with the text 'Sign in'

## Refresh data sources
Now that you have configured the new data source, and have logged in to it, you have to get your data to the .pbix file. In order to get data to load to your dashboard, you have to execute the following steps:
1. Close al popped up screens within Power BI

1. Within the upper toolbar in Power BI, click on the icon with the text 'Refresh'
