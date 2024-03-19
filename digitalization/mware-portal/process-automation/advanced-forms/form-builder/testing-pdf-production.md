# Testing PDF production

### How to test PDF production <a href="#how-to-test-pdf-production" id="how-to-test-pdf-production"></a>

There are two ways to access the "Test PDF" function:

1. directly from the Form Builder's main window's "Test" menu
2. from the "Test Form" window

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-58fe3ecb09ea09eec71fb11e920f09821d93224f%252Ftest-pdf-button.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=eb59ffb5989c6001b269dfc4f7615f6253c0a07b29fb0040fea74ee6c6e0a5d7)

The "Test" menu

When accessed directly, "Test PDF" shows the following dialog:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-22f91a576d96a0c6f7e57ae6ce9e01ddfa25c0ba%252Ftest-pdf-no-data.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=5c2f9aa3be173e6eacb461a4f055f98a8ae7319f6629f49027ffd231df7d4287)

The "Test PDF Production" dialog

When accessed from the "Test Form" window, "Test PDF" shows the following dialog:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-3845b1af1f332253a6800ccb8990ad750939e8f5%252Ftest-pdf-use-data.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=fec99b6c44c6a6936448b01788efeaaee8263c1901e13d11c2ee11e12875ac95)

The "Test PDF Production" dialog

You have access to the following options:

* **Use entered form data**
  * This option only shows when accessed from the "Test Form" window.
  * When selected, any data currently entered in the form will be used to produce the PDF.
* **Disable the evaluation of "Initial Values" formulas**
  * If "Use entered form data" is selected, this option is disabled as the form data takes precedence.
  * When selected, all "Initial Value" formulas are disabled, as if they were not present.
* **Disable the evaluation of "Initial Values" formulas**
  * When selected, all "Initial Value" formulas are disabled, as if they were not present.
* **Disable the evaluation of "Calculated Values" formulas**
  * When selected, all "Calculated Value" formulas are disabled, as if they were not present.
* **Show controls, grids, and sections with dynamic visibility**
  * When selected, all controls, grids, and sections that can be dynamically shown are shown.
  * This means that those with visibility "Yes" or "Formula" are always shown. Controls with visibility "No" remain not visible.
  * Use cases include:
    * Verifying that every control, grid, or section that can appear in the PDF appears correctly.
    * Exporting the PDF file for printing and manual filling.
* **Use PDF template**
  * This only shows if the form has attached PDF templates.
  * See below for details.

Selecting "Continue" opens the resulting PDF, which will show in your browser window.

### PDF templates <a href="#pdf-templates" id="pdf-templates"></a>

The "Test PDF" dialog lets you choose between the [automatic PDF](https://github.com/orbeon/orbeon-forms-doc/blob/master/form-builder/pdf-automatic.md) or one of the available [PDF templates](https://github.com/orbeon/orbeon-forms-doc/blob/master/form-builder/pdf-templates.md) when testing.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-4da1f9c88b689882fda7eef8bf3ad3eac3ecde60%252Ftest-pdf-use-pdf-template.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=6bfa3ef5f8cd81830ac727a52e7e941a2b9d4afff2b9ed30c0b3f448d075c179)

Selecting a PDF template for PDF testing

### Showing hints and alerts <a href="#showing-hints-and-alerts" id="showing-hints-and-alerts"></a>

[\[SINCE Orbeon Forms 2023.1\]](https://doc.orbeon.com/release-notes/orbeon-forms-2023.1)

Two options allow you to show controls hints and alerts in the resulting PDF:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-791de5d222e0da4b993bd9696e7fb7b1785839c1%252Ftest-pdf-hints-alerts-required.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=0d2404a4d370df2e8acf4e7b88df636cfb0be5311478a7570b632ad4bcfc609f)

Options to show hints, alerts, and required

This is an example of the resulting PDF with hints and alerts:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-1dccacef237de95b91062177c68931f8ad6c6436%252Ftest-pdf-hints-alerts-result.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=ad5a75076abf7aa470f94d40354243ae4b3cb9b6eba791e64990c7473b86e3e6)

PDF output with alerts and hints

See also [The `open-rendered-format()` action](https://doc.orbeon.com/form-runner/advanced/buttons-and-processes/actions-form-runner#open-rendered-format).
