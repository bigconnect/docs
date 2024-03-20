# Choices editor

### Introduction <a href="#introduction" id="introduction"></a>

You can edit the possible choices for selection controls by clicking on the icon that shows to the right of a control. When doing so, a dialog, like the one shown in the following screenshot, will appear.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBvnaLnTOjJ7Y4-1d%252Fitemset-editor.png%3Fgeneration=1528743319534300%26alt=media\&width=768\&dpr=4\&quality=100\&sign=53c8e8864fd2b1a3744ad6b64b552693c881c7f84e355da91ce388e8f97d720a)

For each choice, you can enter:

* **Label**: This is what users see when they fill out the form.
* **Value**: This is what is stored as part of the data when users select this choice.
* **Hint**: The third column only shows for radio buttons and checkboxes.
* **Selection**: For radio buttons and checkboxes, whether the item is selected by default.

If you provide a hint for a choice, that choice will be highlighted and the hint you provided will show when users move the mouse pointer over the label, as shown in the following screenshot:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBvnd998utaw7mdoP%252Fitemset-hint.png%3Fgeneration=1528743316728694%26alt=media\&width=768\&dpr=4\&quality=100\&sign=bd98f7bc147ba461f94aedf74ec989463b7ce62d356ee4fbf8e3b805bcb12c7b)

For radio buttons and checkboxes, the "Mark this checkbox if you'd like to type HTML tags" option shows. If you check the HTML checkbox, all the hints and labels you type in dialog are interpreted as HTML, allowing you to use HTML tags in label and hints, say to make text bold or italic:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBvnfC\_HboZQaRHng%252Fitemset-editor-html.png%3Fgeneration=1528743317722428%26alt=media\&width=768\&dpr=4\&quality=100\&sign=190df2595af2e0b149cf062f12ef0df24c86c54b9a70b40f28410124cc43d02c)

This results in the following hint:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBvnh2c3Qket73H-e%252Fitemset-hint-html.png%3Fgeneration=1528743323867221%26alt=media\&width=768\&dpr=4\&quality=100\&sign=0b9fe263f95cfe07ca25e11a4d9c3a2c063579028fe06b4b49f1caab989b2305)

You add items using the "+" button.

You move, insert and remove items using the menu to the left of each row. For example to remove an item entirely:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBvnj5JjO7p6TQTZc%252Fitemset-editor-remove.png%3Fgeneration=1528743318709104%26alt=media\&width=768\&dpr=4\&quality=100\&sign=d19fe5caf1b37448e7d38c629761a9dba4809af6aa7bb06cb7c4baf810a30f91)

### Selected items <a href="#selected-items" id="selected-items"></a>

You can select a default value for a radio button directly in the Form Builder editor simply by clicking the desired radio button or checkbox. You can also do this in the choices editor by clicking the radio button or checkbox associated with an item.

For radio buttons, the choices editor allows you to clear the selected value with the "Clear Value" button.

### Internationalization <a href="#internationalization" id="internationalization"></a>

When the form has more than one language, the choices editor opens in the language selected in Form Builder. You can switch between languages directly in the choices editor. Switching the language allow you to localize labels and hints. However, values are shared between all languages.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBvnlGLw6vPM5mAD9%252Fitemset-editor-language.png%3Fgeneration=1528743321769292%26alt=media\&width=768\&dpr=4\&quality=100\&sign=508aa97f52035f5b21e1f81343257d4149ae98bc6e81b2698d42b162de43a003)

### Removing all items <a href="#removing-all-items" id="removing-all-items"></a>

The "Remove All" button removes all the items in the itemset. This is particularly useful when opening the choices editor for the first time for a given control, since by default itemsets contain default values.

### Usability notes <a href="#usability-notes" id="usability-notes"></a>

* When in a label field, pressing the "tab" key into an empty value field automatically creates a default value. For example:
  * "Apple" becomes "apple"
  * "Wax Apple" becomes "wax-apple"
* When in a value field, pressing the "enter" key automatically adds a new item after the current item.

### Constraints on multiple selection controls values <a href="#constraints-on-multiple-selection-controls-values" id="constraints-on-multiple-selection-controls-values"></a>

While for _single selection controls_ (e.g. radio buttons), the value can be any string of characters, for _multiple selection controls_ (e.g. checkboxes), the value must **not contain spaces**.

The reason for this restriction is that, in the data, the values of checkboxes are stored as _space-separated tokens_. So if you have values `chocolate` and `strawberry` selected, you'll have in the data:

```
chocolate strawberry
```

If the value has a space in it, say `chocolate chip`, then the result would be:

```
chocolate chip strawberry
```

and it would be unclear which values are selected.

### Localization <a href="#localization" id="localization"></a>

Item values are not localizable: they remain the same for each language. On the other hand, item labels can be localized. For example:

* English
  * Name: "Apple"
  * Value: "apple"
* French
  * Name: "Pomme"
  * Value: apple

This ensures that the data captured is machine-readable even if the user interface language changes.
