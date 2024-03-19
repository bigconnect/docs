# Buttons bar

### Introduction <a href="#introduction" id="introduction"></a>

The buttons bar sits at the bottom of the Form Builder screen.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-df5236e735daf2d06d21f39758e0b9f4dc86a11f%252Fbuttons-bar.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=cb921a0da6f72d19378145326731e5dbc6a23221f1baf9f6ce932b9373d92a02)

The buttons bar

By default, the bar includes the following buttons:

* **"Home"**:
  * Go back to the Form Runner landing page.
  * If the current form is not saved, you will be prompted to save it.
* **Summary"**:
  * Go back to the [Form Builder Summary page](https://doc.orbeon.com/form-builder/summary-page).
  * If the current form is not saved, you will be prompted to save it.
* **New"**:
  * Close the current form and create a new one.
  * If the current form is not saved, you will be prompted to save it.
* **Export buttons"**:
  * Excel Export: export the form in Excel format
    * This is an Orbeon Forms PE feature.
  * XML Export: export the form in "XML with metadata" format
    * This is an Orbeon Forms PE feature.
  * XHTML Export
    * [\[SINCE Orbeon Forms 2023.1\]](https://doc.orbeon.com/release-notes/orbeon-forms-2023.1)
    * Export the form definition in XHTML+XForms format (the Orbeon Forms form definition format).
    * This produces a file with the same content shown by the ["Edit Source" dialog](https://doc.orbeon.com/form-builder/advanced/edit-source).
* **Test functions"**:
  * Test: open the form in test mode
  * Test PDF: open the "Test PDF" dialog for testing PDF output
    * This is an Orbeon Forms PE feature.
  * Test Offline: open the form in test mode, but using the "offline" (JavaScript) runtime (experimental)
    * This is an Orbeon Forms PE feature.
  * Formula Inspector: observe formulas and their dependencies (experimental)
* **Publish"**:
  * Open the "Publish" dialog to publish the form definition
  * This automatically saves the form definition as well.
* **Save"**:
  * Save, but don't publish, the form definition

The buttons are configurable with the `oxf.fr.detail.buttons.orbeon.builder` property. The default value is, for Orbeon Forms 2023.1 PE:

Copy

```
<property as="xs:string" name="oxf.fr.detail.buttons.orbeon.builder">
    home
    summary
    new
    form-definition-xhtml-export|excel-export|xml-export|form-definition-xhtml-export
    test|test-pdf|test-offline|test-formulas
    publish
    save
</property>
```

Buttons combined with the `|` character show as a combined "drop-up" button.

### Messages area <a href="#messages-area" id="messages-area"></a>

The buttons bar also includes transient messages, which appear in the messages area. Here is an example:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-e088710f689fa621a4831a03e36cd343249d5c20%252Fbuttons-bar-message.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=d406cd543e6a5eef1922e22b1c4b4b234221abaa5ec308455a2ece8cdd2c959d)

The buttons bar with a status message
