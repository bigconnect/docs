# PDF

### Header and footer configuration <a href="#header-and-footer-configuration" id="header-and-footer-configuration"></a>

The following property controls the header and footer configuration for automatic PDF files. The property is in JSON format.

```
<property as="xs:string" name="oxf.fr.detail.pdf.header-footer.*.*">
    {
      ...
    }
</property>
```

### Custom PDF filename <a href="#custom-pdf-filename" id="custom-pdf-filename"></a>

The following property dynamically controls the name of the PDF file produced on the Detail Page. By default, if the property value is blank, the PDF filename is a random id assigned to the current form session.

```
<property
    as="xs:string"
    name="oxf.fr.detail.pdf.filename.*.*"
    value=""/>
```

The value of the property, if not empty, is an XPath expression which runs in the context of the root element of the XML document containing form data. The trimmed string value of the result of the expression is used to determine the filename.

Example:

```
<property
    as="xs:string"
    name="oxf.fr.detail.pdf.filename.*.*"
    value="//customer-id"/>
```

If the form contains a `customer-id` field, the PDF filename will be the value of that field followed by `.pdf`. If the field is blank, the default, random id filename is used, as if the property had not been specified.

The property has the following default:

```
<property as="xs:string" name="oxf.fr.detail.pdf.filename.*.*">
    concat(
        fr:form-title(),
        ' - ',
        substring(fr:document-id(), 1, 16),
        '.pdf'
    )
</property>
```

A similar default is present for other types of exports (TIFF, Excel, XML, etc.).

### Hyperlinks in automatic mode <a href="#hyperlinks-in-automatic-mode" id="hyperlinks-in-automatic-mode"></a>

The following property controls whether hyperlinks are enabled in the generated PDF. By default, they are enabled:

```
<property
    as="xs:boolean"
    name="oxf.fr.detail.pdf.hyperlinks.*.*"
    value="true"/>
```

When set to `true`:

* HTTP and HTTPS URLs in input field and text areas are automatically hyperlinked.
* Hyperlinks in rich text controls are preserved.
* Hyperlinks in the rest of the form, if any, are preserved.

When set to `false`:

* HTTP and HTTPS URLs in input field and text areas are not hyperlinked, but placeholders are added.
* Hyperlinks in rich text controls are removed and placeholders are left.
* Hyperlinks in the rest of the form, if any, are removed and placeholders are left.
* Placeholders consist of an HTML `<a>` without an `href` attribute. This helps with CSS styling.

For example, the default style for hyperlinks only highlights and underlines `<a>` elements with an `href` attribute:

Copy

```
a[href] {
    text-decoration: underline;
    &:link, &:visited {
        color: @linkColor !important;
    }
}
```

### Avoiding fields being cut across pages <a href="#avoiding-fields-being-cut-across-pages" id="avoiding-fields-being-cut-across-pages"></a>

When a field can be partially, but not entirely, shown at the bottom of a page, instead of showing part of the field on one page and part on the other page ("cutting" the field), it is often better to move that field along with its label to the next page. However, in some cases, when the field contains a lot of text, moving the whole field to the next page means that the "previous page" ends up with a lot of blank space, which is a suboptimal use of the page, and can even be confusing.

Advanced Forms implements the following heuristic in cases where the whole field can't fit on the current page: if the field's content is "short", the whole field is pushed to the next page, and if the content is "longer" the field is "cut", with part of the field on one page, and part on the next page. The threshold separating what is considered to be "short" and "longer" is defined with the following property, in the number of characters, and the default value is 512.

Copy

```
<property
    as="xs:integer"
    name="oxf.fr.detail.pdf.long-content-threshold.*.*"
    value="512"/>
```

### Barcode <a href="#barcode" id="barcode"></a>

The following property specifies whether a barcode must be included on PDF files produced from a PDF template. Adding a barcode to a PDF produced without a PDF template isn't supported at this point.

```
<property
    as="xs:boolean"
    name="oxf.fr.detail.pdf.barcode.*.*"
    value="false"/>
```

### Disabling the PDF button when form is invalid <a href="#disabling-the-pdf-button-when-form-is-invalid" id="disabling-the-pdf-button-when-form-is-invalid"></a>

The PDF button is always enabled, allowing users to generate a PDF for the current form, even if some data in the form is invalid. If instead, you wish to disable the PDF button when the form is invalid, set the following property to `true` (it is set to `false` by default):

```
<property
    as="xs:boolean"
    name="oxf.fr.detail.pdf.disable-if-invalid.*.*"
    value="false"/>
```

### Automatic PDF page size and orientation <a href="#automatic-pdf-page-size-and-orientation" id="automatic-pdf-page-size-and-orientation"></a>

Available page orientations:

* `portrait` (default)
* `landscape`

```
<property 
    as="xs:string" 
    name="oxf.fr.detail.rendered-page-orientation.*.*" 
    value="portrait"/>
```

Available page sizes:

* `letter` (default)
* `a4`
* `legal`

```
<property 
    as="xs:string" 
    name="oxf.fr.detail.rendered-page-size.*.*" 
    value="letter"/>
```

These can also be configured for a particular form in Form Builder's Form Settings dialog.

#### Calculations in readonly modes <a href="#calculations-in-readonly-modes" id="calculations-in-readonly-modes"></a>

See the `oxf.fr.detail.readonly.disable-calculate` property.
