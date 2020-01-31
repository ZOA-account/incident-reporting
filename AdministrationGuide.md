# Administration

<br/><br/>
## Table of contents

1. [Editing the wording of a question](#Editing-the-wording-of-a-question)
1. [Adding an open question](#Adding-an-open-question)
1. [Removing a question](#Removing-a-question)
1. [To bottom](#Navigation-in-this-document)

<br/><br/>
## Editing the wording of a question

1. Open the question database list and filter the Entity Category column to show all questions

1. Find the question that you want to change
1. Edit the English column, and/or any of the questions that you want to change and **save** after

The wording of the question will be changed upon relaunching of the application. It will automatically be deployed for all users

<br/><br/>
## Adding an open question

Note: the following steps are case sensitive

1. Go to **Incident Database** > **settings cogwheel** > **List settings** > **Create column**

1. Fill in the following information to create a new column:

Field | Fill in | Explanation
--- | --- | ---
Column name | Think of a clear and short column name and fill it in | Some of the other column names are **Country**, **Type of Incident**, **Summary**, and suggest using something in that spirit
Type of information in tis column is:| Single line of text | Use single line of text for every question

3. Click on **OK**

1. Add an item to the **Question database**. Make sure to add the following information

Entity Category | Entity Type | Description | Code | English | French | Portuguese | Swahili | Arabic | Spanish
--- | --- | --- | --- | --- | --- |--- | --- |--- | ---
Question | Question | | \<column name you just created> | \<Your question in English> | \<Your question in French> | \<Your question in Portuguese> | \<Your question in Swahili> | \<Your question in Arabic> | \<Your question in Spanish> |

5. In Power Apps studio > select the **EditForm** in the **Tree view** > Click on **Edit fields** in the menu on the right >  click on **+ Add field** > Select the datacard that matches your newly created column

6. In the tree view, select your new datacard > click on **Advanced** in the menu on th right > click on **Unlock to change properties**

7. Update the properties of the datacard like below


Element | Property | Formula
--- | --- | ---
DataCardKey | Text | LookUp(questionCollection,Title="Question"&&Entity_x0020_Type="Question"&&Code="**\<column name you just created>**").Translation
DataCardValue | Default | Parent.Default
DataCard | Default | ThisItem.'**\<column name you just created>**'
DataCard | Update | **\<Insert DataCardValue title of your DataCard>**.Text, e.g. DataCardValue01.Text

8. Click on the submit button **Submit** and open the property **OnSelect** > add your new question to the Collect and Patch payloads.
Replace all spaces with **\_x0020_**. For example, if your card is called **Police Called** and that card's DataCardValue is called DataCardValue04, add **, Police_x0020_Called: DataCardValue04.Text** (remember to add a comma before your new question, or you will get an error) 

Example:

```
If(
    !Connection.Connected && !isNewForm && SelectedScreen = "Unsaved",
    Set(loadingVar_IncidentFormScreen,true);
    Patch(
        incidentsToBeAdded,
        LookUp(
            incidentsToBeAdded,
            UniqueIdentifier = UniqueIdentifierValue.Text
        ),
{
Measures_x0020_Taken_x0020__x002: OtherMeasuresTakenValue.Text,
PGM: PGMValue.Selected.Email,
Counselling_x0020_Required: CouncellingRequiredValue.Selected.Code,
Contributing_x0020_Factors: ContributingFactorsValue.Text

// Our example payload below
, Police_x0020_Called: DataCardValue04.Text

}
    );


If(!Connection.Connected && isNewForm,
    Set(loadingVar_IncidentFormScreen,true);
    Collect(incidentsToBeAdded,
{
Measures_x0020_Taken_x0020__x002: OtherMeasuresTakenValue.Text,
PGM: PGMValue.Selected.Email,
Counselling_x0020_Required: CouncellingRequiredValue.Selected.Code,
Contributing_x0020_Factors: ContributingFactorsValue.Text

// Our example payload below
, Police_x0020_Called: DataCardValue04.Text

}
    );
```
9. Check if Power Apps studio shows any errors next to the elements (datacard, submit button, etc.) that you have changed. If you see errors, resolve them before publishing  

1. Remember to save and publish the app to deploy the changes

### Removing a question

sdf

### Adding an answer

### Removing an answer

### Adding/removing a language