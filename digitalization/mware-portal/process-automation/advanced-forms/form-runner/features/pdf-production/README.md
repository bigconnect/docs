# PDF production

Orbeon Forms can produce PDF output in two ways:

* **Automatically**
  * The PDF is produced based on the appearance of the form in your web browser. This is similar to printing a read-only version of your form.
  * As a form author, you do not need to take any action to enable this mode.
  * For more, see [Automatic PDF](https://doc.orbeon.com/form-runner/features/pdf-production/pdf-automatic).
* **From one or more PDF templates**
  * You upload one or more PDF files using the "Attach PDF Templates" dialog. At runtime, Form Runner fills-out Acrobat fields in the template.
  * This mode is automatically enabled for a form if a PDF template is attached.
  * For more, see [PDF Templates](https://doc.orbeon.com/form-runner/features/pdf-production/pdf-templates).

### Availability <a href="#availability" id="availability"></a>

* Automatic PDF generation is available in Orbeon Forms CE and PE
* PDF Templates are an [Orbeon Forms PE](https://www.orbeon.com/download) feature.

### Setup <a href="#setup" id="setup"></a>

#### Using buttons <a href="#using-buttons" id="using-buttons"></a>

PDF files can be accessed directly from the Form Runner Summary Page and Detail Page using the `pdf` button.

Example for the Detail Page:

Copy

```
<property as="xs:string" name="oxf.fr.detail.buttons.acme.order">
    summary wizard-prev wizard-next pdf tiff save review
</property>
```

Example for the Summary Page:

Copy

```
<property as="xs:string"  name="oxf.fr.summary.buttons.acme.order">
    home review pdf tiff delete duplicate new
</property>
```

See also \[Predefined buttons]\(/form-runner/advanced/buttons-and-processes/README.md#predefined buttons).

#### Sending <a href="#sending" id="sending"></a>

You can _send_ the URL of a PDF file using the `content = "pdf-url"` parameter. See [Sending a PDF URL](https://doc.orbeon.com/form-runner/advanced/buttons-and-processes/actions-form-runner#sending-a-pdf-url).

#### Email <a href="#email" id="email"></a>

You can _email_ the PDF file using the following property:

Copy

```
<property
    as="xs:boolean"
    name="oxf.fr.email.attach-pdf.*.*"
    value="true"/>
```

See also [Email properties](https://github.com/orbeon/orbeon-forms-doc/blob/master/form-runner/configuration/properties/form-runner.md#email-settings).

### Production of TIFF images <a href="#production-of-tiff-images" id="production-of-tiff-images"></a>

\[SINCE Orbeon Forms 2016.1]

PDF files can be converted to TIFF images. See [TIFF Production](https://doc.orbeon.com/form-runner/features/tiff-production).

### Third-party software requirements <a href="#third-party-software-requirements" id="third-party-software-requirements"></a>

Orbeon Forms produces PDF files without the need for any third-party software installation.

All modern web browsers are able to read and render PDF files, which means that you don't need any third-party software at all to read PDF files either.

If you wish so, however, you can use [Adobe Acrobat Reader](https://www.adobe.com/acrobat/pdf-reader.html) to read PDF files. If you are on a Mac, the built-in [Preview](https://support.apple.com/guide/preview/welcome/mac) app also handles PDF files very well without the need for Adobe software.

If you wish to create or update PDF files for [PDF templates](https://doc.orbeon.com/form-runner/features/pdf-production/pdf-templates), then you need Adobe [Acrobat Pro DC](https://www.adobe.com/acrobat/free-trial-download.html?promoid=TY6XKZHL) or an alternative like [PDFpenPro](https://pdfpen.com/pdfpenpro/). However, if you use [automatic PDFs](https://doc.orbeon.com/form-runner/features/pdf-production/pdf-automatic) then you don't need PDF authoring software.
