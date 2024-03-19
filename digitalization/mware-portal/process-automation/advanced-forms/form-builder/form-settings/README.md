# Form settings

Some settings apply to the entire form definition. You access these settings with the "Form Settings" wrench icon on the top right of Form Builder. The General Settings also show initially when creating a new form definition.

### General Settings <a href="#general-settings" id="general-settings"></a>

General settings allow you to set the following form definition metadata:

* Application name
* Form name
* Form title in the current language
* Form description in the current language

![General Settings](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-dec788fff3d9c92a5b235740196adfe26c294da5%252Fform-settings-general.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=5d5f290dade3ab87f4e3f325f497afc3b43bc5bc6f0aca50fafdd0d707c5a116)

The description can optionally use rich text.

![Form description with rich text](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-fc99f6451838ba2cdf21b37d201aa5643f315d64%252Fform-settings-general-html.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=3f9ff2c3569f9a900abb18aa6e0e42340aa6369b62c25b69065995a291f685b0)

### Form Options <a href="#form-options" id="form-options"></a>

Form options include options which apply to the entire form definition.

![Form Options](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-88ccfbfd0d5d01bdbf602ede4bc9d1709f17c2da%252Fform-settings-form-options.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=99932be230112f9ec6adf809dae7d83a4890e5873cb276f4c05aa814ec66beff)

* Singleton Form
  * Enable or disable singleton form behavior.
* Maximum Attachment Size per File
  * Use property: use the `oxf.fr.detail.attachment.max-size-per-file` property
  * Other: the maximum size allowed in bytes
* Maximum Aggregate Attachment Size
  * Use property: use the `oxf.fr.detail.attachment.max-size-aggregate-per-form` property
  * Other: the maximum size allowed in bytes
* Allowed File Types
  * Use property: use the `oxf.fr.detail.attachment.mediatypes` property
  * Other: a space-separated list of mediatypes or wildcard mediatypes
* Appearance of Control Labels
  * Use property: use the `oxf.xforms.label.appearance` property
  * Inline: labels show inline above the control
* Use Placeholder for Text Fields and Text Areas
  * Labels show inline above the control for most fields.
  * For text, date, and time input fields, labels show as an HTML _placeholder_ within the field when the field is empty.
  * For text areas, labels show as an HTML _placeholder_ within the field when the field is empty.&#x20;
* Appearance of Control Hints
  * Use property: use the `oxf.xforms.hint.appearance` property
  * Inline: hints show inline below the control
  * Tooltips: hints show as tooltips upon mouseover
* Use automatic hints
  * Use property: use the `oxf.fr.detail.hint.automatic.*.*` property
  * Yes/No: show or don't show the automatic hints for all controls on this form
* Use Placeholder for Text Fields and Text Areas
  * Hints show inline or as tooltips upon mouseover.
  * For text, date, and time input fields, hints show as an HTML _placeholder_ within the field when the field is empty.
  * For text areas, hints show as an HTML _placeholder_ within the field when the field is empty.
* Simple Data Migration
  * For details, see Simple data migration.
* Automatic Calculations Dependencies
  * For details, see Automatic calculations dependencies.
* Grid Tab Order
  * Use property: use the `oxf.xforms.xbl.fr.grid.tab-order` property
  * For details, see Grid Tab Order in the Grid Settings dialog.

Here is how hints appear depending on the type of control they are associated with:

<figure><img src="https://doc.orbeon.com/~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBtUVPhNrIEbjNXU9%252Fplaceholder-and-inline-hints.png%3Fgeneration=1528743309737953%26alt=media&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=ff680e57205d708523f5460405b7874f3e5bc249f861c6abdfcbb77fcc35cbac" alt=""><figcaption></figcaption></figure>

### Control Settings <a href="#control-settings" id="control-settings"></a>

This tab shows settings for specific controls. Those settings apply to all control on the form except when they are overridden by individual settings in the "Control Settings" dialog.

For example, you can set a "Digits After Decimal" setting global to the form, and then override it on specific number controls as needed.

![Number Control Settings](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-2657e8a57076d86c0efaec47cb7749571a1efadd%252Fform-settings-controls-number.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=847e4454d577f86c3481bbe1713bc7b695c5aba2d9b66a367bd3f2119cf0c4cb)

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-d48c9c8fcaae7850b36eeef6a92cefcf9ce718df%252Fform-settings-controls-date.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=e87d58ed871322b91017e8e5ebf7a8c38a2947012b8226aa149c84842a4033cf)

Date Control Settings

See also the [blog post](https://blog.orbeon.com/2019/03/form-level-and-control-level-settings.html).

### View Options <a href="#view-options" id="view-options"></a>

These options are now in a separate tab.

![View Options](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-dcbcbf7848fd0928c4ae17ec8ba7f4eacc3cd610%252Fform-settings-view-options.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=f73cdbaffb7ac625802ac3b7fe5b0e72d63424e4ce6781a7cef6471a680d1ae7)

#### Browser page layout <a href="#browser-page-layout" id="browser-page-layout"></a>

* Use property: use the `oxf.fr.detail.html-page-layout` property
* Fixed width: the form sections and grids take a fixed and predefined width of approximately 940px for large displays (the layout becomes responsive for smaller displays sizes).
* Fluid width: the form sections and grids take the entire web browser's viewport size.

#### Wizard options <a href="#wizard-options" id="wizard-options"></a>

For details, see Wizard View.

* Wizard View:
  * Use property: use the `oxf.fr.detail.view.appearance` property
  * Always or Never: enable or disable the wizard view for this form definition, no matter how the property is configured.
* Wizard Navigation Validation Mode
  * Form Builder setting
  * Use property: use the `oxf.xforms.xbl.fr.wizard.validate` property
  * Free, Lax or Strict: use the given validation mode for this form definition, no matter how the property is configured.
* Wizard Subsections Navigation
  * Form Builder setting
  * Use property: use the `oxf.xforms.xbl.fr.wizard.subsections-nav` property
  * Always or Never: enable or disable subsection navigation for this form definition, no matter how the property is configured.
* Wizard Subsections Table of Contents
  * Form Builder setting
  * Use property: use the `oxf.xforms.xbl.fr.wizard.subsections-toc` property
  * "Show subsections for the active section only", "Show subsections for all sections", "Don't show subsections": use the given setting for this form definition, no matter how the property is configured.

### PDF Options <a href="#pdf-options" id="pdf-options"></a>

![PDF Options](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-8b01c02a117e4a9937508adb29a7ef3db092168a%252Fform-settings-pdf-options.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=b96150a7e9809a8fc45e186c03bb31691cf76bc918db7ac16664671c9637535c)

* PDF page orientation
  * Use property: use the `oxf.fr.detail.rendered-page-orientation` property
  * Portrait: use the portrait (vertical) orientation.
  * Landscape: use the landscape (horizontal) orientation.
* PDF page size
  * Use property: use the `oxf.fr.detail.rendered-page-size` property
  * Letter: US letter size.
  * A4: standard A4 size.
  * Legal: US legal size.

### Formulas <a href="#formulas" id="formulas"></a>

![Readonly formula](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-9ca6b4686f0bc30dc2043948883b1762480c83d0%252Fform-settings-formulas.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=a98c816af3bf73757085fe72f77e4de14dfe1ecbb6d8431d258a8848a973619f)

* **Read-Only:** Boolean expression specifying whether the entire form is read-only (not editable).
  * If this field is left blank, then the form is editable.
  * Otherwise, it is editable only if the result of the Boolean expression is `false()`.
* **Calculations in readonly modes (Review, PDF)**:
  * Use property: use the `oxf.fr.detail.readonly.disable-calculate` property
    * the property defaults to `false`, which means that Calculated Value formulas run in readonly modes
  * Enable: Calculated Value formulas run in readonly modes.
  * Disable: Calculated Value formulas do not run in readonly modes.

Sometimes calculations should not run in readonly modes, such as the View page. This can now be configured at the form level.

### About this Form <a href="#about-this-form" id="about-this-form"></a>

![About this Form](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-3a128b6e0a6fbf099515490a912ec5d693bbf406%252Fform-settings-about.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=0ca802f2fa39e7359688081776d5192e1964d95631fff495d0fa9c4fd75fa303)

#### Versions <a href="#versions" id="versions"></a>

This area shows relevant Advanced Forms versions:

* "Created with Version": version with which this form definition was created.
* "Updated with Versions": versions with which this form definition was updated This is updated:
  * when saving the form definition
  * when upgrading the form definition from the Form Runner Home page.
* "Current Version": the current Advanced Forms version.

#### Form Statistics <a href="#form-statistics" id="form-statistics"></a>

Form statistics show counts of various form elements.
