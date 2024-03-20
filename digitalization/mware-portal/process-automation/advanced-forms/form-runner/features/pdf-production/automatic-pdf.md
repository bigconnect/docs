# Automatic PDF

The PDF is produced based on the appearance of the form in your web browser. This is similar to printing a read-only version of your form. As a form author, you do not need to take any action to enable this mode. In addition, a header and footer is provided by default.

### Header and footer configuration <a href="#header-and-footer-configuration" id="header-and-footer-configuration"></a>

See Header and footer configuration.

### Font configuration and embedding <a href="#font-configuration-and-embedding" id="font-configuration-and-embedding"></a>

#### Overview <a href="#overview" id="overview"></a>

These properties allow specifying fonts to embed in PDF files. You do this with 2 properties, which are tied together by a name you choose, and which doesn't have any significance outside the `properties-local.xml`. For instance, say you want to use the [Roboto](https://fonts.google.com/specimen/Roboto) font. You first map the family name, which you use in your CSS (here `Roboto`, with an uppercase `R`) to the name you chose (here `roboto`, with a lowercase `r`):

```
<property
    as="xs:string"
    name="oxf.fr.pdf.font.family.roboto"
    value="Roboto"/>
```

Then you indicate where the TTF file for the name you chose is located on disk, using either one of the following two properties. Use the first if you want to place your font inside the Advanced Forms `WIB-INF/resources` directory, or use the second if you want to place it somewhere else, referring to the file using an absolute path.

```
<property
     as="xs:string"  
     name="oxf.fr.pdf.font.resource.roboto"                                  
     value="/forms/resources/Roboto-Medium.ttf"/>
<property
    as="xs:string"
    name="oxf.fr.pdf.font.path.roboto"
    value="/absolute/path/to/Roboto-Medium.ttf"/>
```

#### Changing the default font <a href="#changing-the-default-font" id="changing-the-default-font"></a>

To change the main font, you must map your TTF file to the `Helvetica Neue` family. You often need to do this if you are using characters that are not included in the default font used by the PDF engine, for instance to have Chinese characters show in the PDF. To change this, you need to obtain a Unicode font, and configure it as follows, here assuming the file is named `arialuni.ttf`:

```
<property
     as="xs:string"  
     name="oxf.fr.pdf.font.resource.arial-unicode"                                  
     value="/absolute/path/to/arialuni.ttf"/>
<property 
    as="xs:string"  
    name="oxf.fr.pdf.font.family.arial-unicode"                            
    value="Helvetica Neue"/>
```

#### New default font <a href="#new-default-font" id="new-default-font"></a>

Advanced Forms now includes the [Inter font](https://rsms.me/inter/) by default. This provides more characters out of the box (see [Language support](https://rsms.me/inter/#languages)).

### PDF color mode <a href="#pdf-color-mode" id="pdf-color-mode"></a>

By default, the PDF is output with most colors removed and a black-and-white scheme. With the `oxf.fr.detail.pdf.color-mode`, you can change that default to allow field colors to be used. This impacts the following form controls:

* Explanatory Text
* Formatted Text
* Calculated Value (if the value is in HTML format)

```
<property
    as="xs:string"
    name="oxf.fr.detail.pdf.color-mode.*.*"
    value="keep-field-colors"/>
```

### Accessibility and PDF/A support <a href="#accessibility-and-pdf-a-support" id="accessibility-and-pdf-a-support"></a>

Experimentally, the following properties allow enabling accessibility and PDF/A support:

```
<property as="xs:boolean" name="oxf.fr.pdf.accessibility" value="true"/>
<property as="xs:string"  name="oxf.fr.pdf.pdf/a"         value="3u"/>
```

* `oxf.fr.pdf.accessibility`
  * enable or disable accessibility support
  * values: `true` or `false`
  * default: `false`
* `oxf.fr.pdf.pdf/a`
  * enable or disable PDF/A support
  * values:
    * `none`: disabled
    * `1a`: PDF/A-1a – Level A (accessible) conformance
    * `1b`: PDF/A-1b – Level B (basic) conformance
    * `2a`: PDF/A-2a
    * `2b`: PDF/A-2b
    * `2u`: PDF/A-2b plus Unicode
    * `3a`: PDF/A-3a
    * `3b`: PDF/A-3b
    * `3u`: PDF/A-3b plus Unicode
  * default: `none`

Note that the values above are simply passed to the PDF renderer library used by Advanced Forms. There is currently no guarantee of compliance with the PDF/A standard.
