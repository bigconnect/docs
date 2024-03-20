# Control settings

### Introduction <a href="#introduction" id="introduction"></a>

The Control Settings dialog allows controlling all the aspects the a control besides its label and hint. The dialog has several tabs, detailed below.

#### Basic Settings <a href="#basic-settings" id="basic-settings"></a>

![Basic Settings tab](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-8bd3c422083e67c8edca7f7d312c389cb179801f%252Fcontrol-settings.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=04b2e2bbce86c6e8f81778d0ac6b5399213994a2ad4e40509483df95d03b79af)

**Basic options**

The control _name_ specifies a identifier for the control, unique in the entire form (except Section Templates). The identifier is used for the following:

* to refer to the control value from formulas, using the variable notation `$foo` where `foo` is the control name
* to determine an XML element name when the form data is represented as XML

If a control name is not explicitly specified, Form Builder assigns a default name, such as `control-42`.

A control name can be changed, provided it doesn't collide with another control name (an error will show otherwise).

See Renaming of controls and formulas.

**Main options**

The following options are available:

* **Show on Summary page:**
  * When selected, the control value is visible as a Summary page column and searchable.
* **Allow bulk edit:**
  * When selected, the control value can be bulk edited from the Summary page.
* **Index control:**
  * Index the value of this control even if it's not shown or searchable from the Summary page, or bulk editable. This makes the control accessible from the search API.
* **Encrypt data at rest:**
  * See Field-level encryption for details.

**Role restriction**

It is possible to restrict the "Show on Summary page" and "Allow bulk edit" options to one or more specific roles (i.e. group of users). When multiple roles are specified, the logic that applies can be chosen as well. If "All" is selected, then all specified roles must be present. If "Any" is selected, just one of the specified roles must be present.

![Role restriction](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-7da53ae6f2cc4e5399c2b46e23a0d4889040294a%252Fcontrol-settings-restrict-to-role.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=2fe47bedb31fcb29ab1ad0503d5d7486d83bec2bdd13e6ebd61a9c9aa66db2d0)

Role restriction

**Bulk action**

By default, bulk edition of form data on the Summary Page will simply save the updated form data. You can also specify a custom process to run by selecting "Run process" instead of "Save data".

![Bulk process](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-6764c579736d2124aea75c9ec054aa78f1d9852d%252Fcontrol-settings-bulk-process.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=655c67d169c51a776147741cf45f26ac5d189b2048bfdaa3aeca676a0dd82d45)

Bulk process

The selected process is responsible for saving the data.

It's possible to specify a list of processes available for selection in the Control Settings dialog. This is done by specifying the following property:

```
<property
    as="xs:string"
    name="oxf.fr.summary.bulk-processes.*.*"
    value="process1 process2"/>
```

The value is a space-separated list of process names. By default, this property is empty.

**Email options**

The following email options are available:

* **Email recipient:**
  * When selected, the control is used to determine an email recipient ("To:") when the form data is sent by email.
  * If more than one non-blank email addresses is found, they are all included as email recipients. In addition, the `oxf.fr.email.to` property is used.
  * A single control value can contain more than one email address, separated by commas (`,`) or spaces.
* **Email carbon copy recipient:**
  * When selected, the control is used to determine a carbon copy email recipient ("Cc:") when the form data is sent by email.
  * If more than one non-blank email addresses is found, they are all included as email recipients. In addition, the `oxf.fr.email.cc` property is used.
  * A single control value can contain more than one email address, separated by commas (`,`) or spaces.
* **Email blind carbon copy recipient:**
  * When selected, the control is used to determine a blind carbon copy email recipient ("Bcc:") when the form data is sent by email.
  * If more than one non-blank email addresses is found, they are all included as email recipients. In addition, the `oxf.fr.email.bcc` property is used.
  * A single control value can contain more than one email address, separated by commas (`,`) or spaces.
* **Email sender:**
  * When selected, the control is used to determine an email sender ("From:") when the form data is sent by email.
  * Only _one_ "From:" email address is used, specifically the first non-blank address selected in the form. If no such address is found the `oxf.fr.email.from` property is used.
* **Email reply-to:**
  * When selected, the control is used to determine a reply-to address ("Reply-To:") when the form data is sent by email.
  * Only _one_ "Reply-To:" email address is used, specifically the first non-blank address selected in the form. If no such address is found the `oxf.fr.email.reply-to` property is used.
* **Exclude from email body:**
  * When using "All Control Values" in an email body template, controls selected with this checkbox will be _excluded_ and omitted from the email body.
* **Include as email attachment**:
  * this option only shows for file and image attachments
  * when the property `oxf.fr.email.attach-files` is set to `selected`, only file and image attachments with this option checked are attached to the email
* **Show in email subject:**
  * When selected, the control value is used as part of the subject of the email when the form data is sent by email.
  * If more than one non-blank values are found, they are all included in the email subject, comma-separated.

Email addresses can now contain a name, following the standard syntax:

```
John Smith <john@example.org>
```

Or:

```
"John Smith" <john@example.org>
```

When more than one email is present, if you specify names, you must use commas exclusively as a separator (with whitespace allowed around commas):

```
John Smith <john@example.org>, Alice <alice@acme.org>
```

Or:

```
"John Smith" <john@example.org>, "Alice" <alice@acme.org>
```

**Name of the PDF field**

This informational field shows the name of the field to use when using a PDF template.

**Custom CSS classes**

The "Custom CSS Classes" field allows adding CSS classes which will be placed on the control in the resulting HTML. This can be used for custom styling.

CSS classes don't directly control the style. Instead, they are space-separated tokens, which can then be referred to in your own CSS files.

You could have for example:

```
my-name-field my-highlight
```

In your custom CSS file, you could refer to these classes with a selector, for example:

```
.forms .my-name-field.my-highlight input {
  border-color: blue;
}
```

_NOTE: The above uses example `my-` prefixes as it is a good idea to prefix CSS classes to avoid clashes with other CSS classes on the page._

**Autocomplete attribute**

The "Autocomplete attribute" dropdown allows specifying an [HTML autocomplete value](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete) for controls for which it makes sense to do so. Such a value tells the browser that it can suggest values for the field or, in the case of the `off` value, that it shouldn't suggest any value.

**Control appearance**

Some controls support more than one appearance. For example, a single selection control can appear as a dropdown menu, or as radio buttons. When available, the "Control Appearance" selector allows selecting and changing the appearance of the control.

**Custom control settings**

Some controls have custom settings. For example:

![Custom Control Settings](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-3ec7aa5bea960986f7ee9d28ba2073e20dc7ab43%252Fcontrol-settings-custom-properties.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=00eb483faa88a938960b88647708b8fcac67f283df00e2bb77f9404a2527f761)

The following controls have custom settings:

* Number
* Currency
* Date
* Time
* Date and Time
* Dropdown Date
* Dropdown
* Dropdown with Search
* File Attachment
* Image Attachment

**Automatic PDF options**

When producing a PDF file, single-selection controls like the "Static Dropdown" normally just output the selected value. However, in some cases, it is useful for the user to see all the available options.

You can achieve this with the "Automatic PDF" options in the "Control Settings" dialog for single-selection controls. This option is hierarchical: you can set it at the field level, form level, or globally. They are:

* **Use default**: use the form settings, and if they also have "Use default" use the property (see below)
* **Show the selected value only**: the label for the selected value shows
* **Show all values as radio buttons**: all possible values show as radio buttons and the selected value, if any, shows selected

![Automatic PDF options](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-a7f2d3bba239c966168b78dc5aa06fa8d88c60dc%252Fcontrol-settings-pdf-options.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=94752f8b751d6853f59ced14dcaaa25049756c2d760bd07d63d56868a3bf29e8)

The supported properties are:

* `oxf.xforms.xbl.fr.dropdown-select1.pdf-appearance.*.*`
* `oxf.xforms.xbl.fr.'dropdown-select1-search'.pdf-appearance.*.*`

The possible values for each property are:

* empty: show the selected value only (default)
* `full`: show all values as radio buttons

Example to change the default the "Static Dropdown" control to showing all values in the automatic PDF:

```
<property 
    as="xs:string" 
    name="oxf.xforms.xbl.fr.dropdown-select1.pdf-appearance.*.*"
    value="full"/>
```

The global default is "Show the selected value only".

#### Validations and alerts <a href="#validations-and-alerts" id="validations-and-alerts"></a>

![Validations and alerts tab](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LImOuyX6v8jYBpAvh\_S%252F-LEkBwwzQBuFcIscjgrv%252Fcontrol-settings-validations.png%3Fgeneration=1533075263475905%26alt=media\&width=768\&dpr=4\&quality=100\&sign=0ab7df4237f0b7dbd2c129ba14edbfb3929f864b6d101b429e32690a79e82515)

#### Formulas <a href="#formulas" id="formulas"></a>

![Formulas tab](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LQ5z6SJ1C6JNSNI51lo%252F-LEkBwx0uppKKLVgHDn6%252Fcontrol-settings-formulas.png%3Fgeneration=1540936764793083%26alt=media\&width=768\&dpr=4\&quality=100\&sign=be764afc0484307d03f12cdd6577db34d9d159f38d6f3da1564aa4485d453360)

The "Yes" and "No" options have their own radio button. Select "Formula" to enter a dynamic "Visibility" or "Read-Only" formula.

#### Explanatory Text <a href="#explanatory-text" id="explanatory-text"></a>

For the Explanatory Text control only, you can modify the text in this location, including making the text dynamic using templates as is the case for the Label, Hint and Help Message.

![Explanatory Text tab](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LVyWCiGJ-WewsAMnHM5%252F-LVyWLvUjl8PnTrNFelc%252Fcontrol-settings-explanatory-text.png%3Fgeneration=1547237160959376%26alt=media\&width=768\&dpr=4\&quality=100\&sign=385d32e8de0378ee7c58d81cdcb327554b4ebc58c7d1548bd62e3af01ed0c8d9)

#### Label and Hint <a href="#label-and-hint" id="label-and-hint"></a>

In addition to setting a control's label and hint in place in the form area, you can also set and update them in this tab. You can switch between plain text and HTML text as well. The "Previous" and "Next" buttons allow quick navigation between controls.

![Label tab](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-14ef587c8d8bd5e0eedf303d4ff395c48816fe71%252Fcontrol-settings-label-hint.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=e6c46714e0d39d87637cc23a6e684bdd27048f5e59e1666a6c65b77b81cc8dff)

**Automatic hints**

For attachment controls, in addition to the hint you provide for the control, Advanced Forms can add an automatic hint informing the user about the maximum allowed file size and the accepted file types, if you have specified such validations in the Validations and Alerts tab.

Automatic hints are disabled by default, and you can enable them for a specific control in the Control Settings dialog, for all controls in a form in the Form Settings dialog, or globally using the property below. A setting other than "Default" in the Control Settings dialog box overrides a setting other than "Use property" in the Form Settings dialog box, which overrides the value of the property below.

```
<property 
    as="xs:boolean"
    name="oxf.fr.detail.hint.automatic.*.*"                           
    value="true"/>
```

#### Help Message <a href="#help-message" id="help-message"></a>

![Help tab](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LImOuyX6v8jYBpAvh\_S%252F-LEkBwx4SaqLJdT4KLKd%252Fcontrol-settings-help.png%3Fgeneration=1533075264162272%26alt=media\&width=768\&dpr=4\&quality=100\&sign=23b17c43cebab79e16ca174ec95663db37e56acd7c9fb3764e22f150a93503ec)

This allows specifying some help text, which can be plain text or rich text when the "Use HTML" checkbox is selected.

The help message is available at runtime through a help icon positioned next to the control. By default, the icon opens a pop-up containing the help text.

The help text is localizable.

#### Dynamic labels, hints and help messages <a href="#dynamic-labels-hints-and-help-messages" id="dynamic-labels-hints-and-help-messages"></a>

Controls support _dynamic_ labels, hints, and help messages. This means that, instead of being specified once and for all at form design time, labels, hints and help messages can incorporate dynamic parts such as control values and other custom expressions.

#### Language selector <a href="#language-selector" id="language-selector"></a>

When multiple languages are available for the form, a language selector allows selecting the language for which localizable texts are being edited (e.g. Validations and Alerts, Label, Hint, and Help Message) directly from the Control Settings dialog.

![Language selector](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-c96646a56a5b8277cd67208599e64d5b3e8d26e3%252Fcontrol-settings-language.selector.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=072627058d253b4f4d42b71703df2d6eb9c8ddfae4112308073457a7e2197b17)

