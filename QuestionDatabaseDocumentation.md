# Question Database Documentation

## Columns

The question database is the backbone for all userinterface elements in the PowerApp. This SharePoint list allows the PowerApp to show the translated labels. By default English is the only populated column, but there are columns for Arabic, English, French, Portuguese, Spanish, and Swahili. It is possible to add columns for other languages, but those changes need to be reflected in the logic of the Power App.

This is an overview of the columns used by the application:

Column name | What it is for
--- | ---
Entity Category | Identifier used by Power App
Entity Type | Identifier used by Power App
Description | Optional description
Code | Identifier for the Power Application
English | Translations used by the Power Apps to display translated text (required)
French | Translations used by the Power Apps to display translated text (optional)
Portuguese | Translations used by the Power Apps to display translated text (optional)
Swahili | Translations used by the Power Apps to display translated text (optional)
Arabic | Translations used by the Power Apps to display translated text (optional)
Spanish | Translations used by the Power Apps to display translated text (optional)

The Entity Category distinguishes between:
1. AnswerDropdown
1. Question
1. UserInterface
1. StandardOperatingProcedres

The Entity Type is more granular, as it for example distinguishes between specific answer dropdowns such as "TypeOfIncidentDropdown"

> Note: It is possible to change the default language of the Power App from English to another language 


## The 'Code' column
In the Code column you will find 4 different usages of codes (keys) in the application:

Title | How to recognise | What it is for
--- | --- | ---
Country | ISO 3166-1 alpha-2 country codes (two upper case letters) | Populates country dropdown
User interface text | All lower case key | Used in questions, answers, and other user interface elements in the Power App
Incident Database mappings | Starts with upper case letters | Used by PowerApp to map questions to the Incident Database
Standard Operating Procedures | Numbers | Allows PowerApp to list Standard Operating Procedures

Note: Standard Operating procedures are disabled by default, and is not required

## The use of 'zzz'

Some of the translated answers, e.g. **zzzOther** contain **zzz**. 

This is necessary if you want to have **Other** as the last answer in a dropdown, rather than the otherwise lowest ranked answer in the alphabetical answer. The Power App filters the **zzz** out and only displays the option as **Other**