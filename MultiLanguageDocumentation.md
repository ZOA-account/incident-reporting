# Multi-language documentation

The language feature of the app uses the QuestionDatabase to fetch translations

To add a language, first add a column to the Question Database using the following settings:

Setting | What to choose
---|---
Column name: | English name of the language
The type of information in this column is: | Multiple lines of text
Specify the type of text to allow: | Plain text

Then make appropriate edits to the Power App:

HomeScreen.LanguageDropdown.Items by default:
```
["English"]
```
To show other options in the dropdown, add them in this style:
```
["English","Spanish","Swahili"]
```



HomeScreen.LanguageDropdown.OnChange by default:

```
Set(AlignVar,Align.Left);

If(
    LanguageDropdown.SelectedText.Value="Français",
    ClearCollect(tempItems,DropColumns(questionCollection,"Translation"));
    ClearCollect(questionCollection,AddColumns(tempItems,"Translation",French));
    Clear(tempItems)
);

If(
    LanguageDropdown.SelectedText.Value="English",
    ClearCollect(tempItems,DropColumns(questionCollection,"Translation"));
    ClearCollect(questionCollection,AddColumns(tempItems,"Translation",English));
    Clear(tempItems)
);

If(
    LanguageDropdown.SelectedText.Value="عربى",
    ClearCollect(tempItems,DropColumns(questionCollection,"Translation"));
    ClearCollect(questionCollection,AddColumns(tempItems,"Translation",Arabic));
    Clear(tempItems);
    //Add the line below only for languages that are aligned from the right
    Set(AlignVar,Align.Right)
);

If(
    LanguageDropdown.SelectedText.Value="Español",
    ClearCollect(tempItems,DropColumns(questionCollection,"Translation"));
    ClearCollect(questionCollection,AddColumns(tempItems,"Translation",Spanish));
    Clear(tempItems);
);
```
> Note: Since Arabic is written from right to left, we set the AlignVar to Align.Right. Do the same for other languages that are written from the right to the left.

To add extra languages, make sure to add a block in this style:
```
If(
    LanguageDropdown.SelectedText.Value="Italiano",
    ClearCollect(tempItems,DropColumns(questionCollection,"Translation"));
    ClearCollect(questionCollection,AddColumns(tempItems,"Translation",Italian));
    Clear(tempItems);
);
```