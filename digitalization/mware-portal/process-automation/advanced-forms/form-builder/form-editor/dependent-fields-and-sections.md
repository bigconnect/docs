# Dependent fields and sections

### Rationale <a href="#rationale" id="rationale"></a>

* Both fields and sections can:
  * Be shown or hidden depending on the value of other fields, by setting their **visibility** property.
  * Become read-only, as opposed to read-write, depending on the value of other fields, by setting their **read-only** property.
* In addition, fields can also:
  * Have a dynamic value set when the form is first loaded by users, by setting their **initial value** property.
  * Have a value that is updated "live" as users set or change the value of other fields in the form, by setting their **calculated value** property.

By using a combination of the aforementioned visibility, read-only, initial value, and calculated value properties for sections and fields, you can create forms that are very dynamic, which can behave like a spreadsheet, or like a small application.

The visibility, read-only, initial value, and calculated value are set using [formulas](https://doc.orbeon.com/form-builder/formulas) in the [Control Settings](https://doc.orbeon.com/form-builder/form-editor/control-settings) or [Section Settings](https://doc.orbeon.com/form-builder/form-editor/section-settings) dialogs.

### Examples <a href="#examples" id="examples"></a>

#### Hide a field based on the user's answer to an earlier questions <a href="#hide-a-field-based-on-the-users-answer-to-an-earlier-questions" id="hide-a-field-based-on-the-users-answer-to-an-earlier-questions"></a>

Say you want to show a textarea when users check a checkbox to indicate they want to provide more details:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252Fb5632ad6983b28da53dd11e1d7e6e8dfb1a50fc4.png%3Fgeneration=1604383139739351%26alt=media\&width=768\&dpr=4\&quality=100\&sign=413e3942f7656bb67a4ff0032716bd77ac4209a6952feaf25195c3fe01daa688)

Textarea shown if checkbox is checked

1. Add a Single Checkbox control, open its Control Settings, name it `provide-additional-details`.
2. Add a Plain Text Area control, open its Control Settings, open the Formulas tab, set the Visibility to Formula, an enter `$provide-additional-details/string() = 'true'`.
