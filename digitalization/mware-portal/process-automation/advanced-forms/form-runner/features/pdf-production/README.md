# PDF production

Advanced Forms can produce PDF output in two ways:

* **Automatically**
  * The PDF is produced based on the appearance of the form in your web browser. This is similar to printing a read-only version of your form.
  * As a form author, you do not need to take any action to enable this mode.
* **From one or more PDF templates**
  * You upload one or more PDF files using the "Attach PDF Templates" dialog. At runtime, Form Runner fills-out Acrobat fields in the template.
  * This mode is automatically enabled for a form if a PDF template is attached.

### Setup <a href="#setup" id="setup"></a>

#### Using buttons <a href="#using-buttons" id="using-buttons"></a>

PDF files can be accessed directly from the Form Runner Summary Page and Detail Page using the `pdf` button.

Example for the Detail Page:

```
<property as="xs:string" name="oxf.fr.detail.buttons.acme.order">
    summary wizard-prev wizard-next pdf tiff save review
</property>
```

Example for the Summary Page:

```
<property as="xs:string"  name="oxf.fr.summary.buttons.acme.order">
    home review pdf tiff delete duplicate new
</property>
```

#### Sending <a href="#sending" id="sending"></a>

You can _send_ the URL of a PDF file using the `content = "pdf-url"` parameter.&#x20;

#### Email <a href="#email" id="email"></a>

You can _email_ the PDF file using the following property:

```
<property
    as="xs:boolean"
    name="oxf.fr.email.attach-pdf.*.*"
    value="true"/>
```

### Production of TIFF images <a href="#production-of-tiff-images" id="production-of-tiff-images"></a>

PDF files can be converted to TIFF images.&#x20;

### Third-party software requirements <a href="#third-party-software-requirements" id="third-party-software-requirements"></a>

Advanced Forms produces PDF files without the need for any third-party software installation.

All modern web browsers are able to read and render PDF files, which means that you don't need any third-party software at all to read PDF files either.

If you wish so, however, you can use [Adobe Acrobat Reader](https://www.adobe.com/acrobat/pdf-reader.html) to read PDF files. If you are on a Mac, the built-in [Preview](https://support.apple.com/guide/preview/welcome/mac) app also handles PDF files very well without the need for Adobe software.

If you wish to create or update PDF files for PDF templates, then you need Adobe [Acrobat Pro DC](https://www.adobe.com/acrobat/free-trial-download.html?promoid=TY6XKZHL) or an alternative like [PDFpenPro](https://pdfpen.com/pdfpenpro/). However, if you use automatic PDFs then you don't need PDF authoring software.
