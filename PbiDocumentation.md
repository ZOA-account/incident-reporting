# Migration guide .pbix file to new tenant

This document is a guide for setting up the Power BI dashboards provided with the Incident Reporting App.
There are a few steps that need to be taken in order to set up the reports. 
<br/>
<br/>
These steps are: 
- Changing the data source
- Signing in to the new data source
- Refresh data sources

These steps wil be outlined in more detail below.
<br/>
## Changing the data source
For changing the data source, please take the following steps:
- Open the Power BI file (a .pbix file)
- Click in the upper left corner on the icon with the text 'File'
- In the menu that drops down, hover over the section with the text 'Options and settings'
- In the pane on the right, click on the section with the text 'Data source settings'
-  In the window that pops up, click the button 'Change source...'
- In the window that pops up, fill in the SharePoint site adress
- In the popped up window, click on the 'OK' button
<br/>
## Signing in to the new data source
When you have changed the data source, the next step is tho sign in tho the new data source. To do this, please take the following steps:
- In the last screen of the following step (named 'Data source settings'), click the button with the text 'Edit permissions'
- In the window that pops up, click the button with the text 'Edit...'
- In the window that pops up, click the button with the text 'Sign in as a different user'
- In the window that pops up, click on the field with the text 'Use antoher account'
- In the new screen, fill in your Email 
- Click the button with the text 'Next'
- In the new screen, fil in your password
- Click the button with the text 'Sign in'
<br/>
## Refresh data sources
Now that you have configured the new data source, and have logged in to it, you have to get your data to the .pbix file. In order to get data to load to your dashboard, you have to execute the following steps:
- Close al popped up screens within Power BI.
- Within the upper toolbar in Power BI, click on the icon with the text 'Refresh'
