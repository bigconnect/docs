# Attachments

### Maximum attachment size <a href="#maximum-attachment-size" id="maximum-attachment-size"></a>

The following property sets the maximum size in bytes of an uploaded attachment. For example, if you set it to `1000000` (1 MB), and the user attempts to upload a larger attachment, an error is reported.

```
<property 
    as="xs:string"  
    name="oxf.fr.detail.attachment.max-size-per-file.*.*"                      
    value="1000000"/>
```

If the value is blank (the default), then the value of the following backward compatibility property is used:

```
<property
    as="xs:integer" 
    processor-name="oxf:request"   
    name="max-upload-size"          
    value="100000000"/>
```

The value of `oxf.fr.detail.attachment.max-size-per-file` can be overridden:

* for a specific form, from the Form Builder "Form Settings" dialog
* for a specific control, using a common constraint in the Form Builder "Control Settings" dialog

This property was previously named `oxf.fr.detail.attachment.max-size`. The old name is still supported for backward compatibility.

### Maximum aggregate attachment size (forms) <a href="#maximum-aggregate-attachment-size-forms" id="maximum-aggregate-attachment-size-forms"></a>

The following property sets the maximum aggregate size in bytes of all uploaded attachments for a given instance of form data. For example, if you set it to `1000000` (1 MB), and the form has two attachment controls, and you upload a 600 KB attachment using the first control, then only 400 KB can be uploaded using the second control, even if a larger maximum size per control was set using the `oxf.fr.detail.attachment.max-size-per-file` property or a Form Builder setting. If you attempt to upload a larger attachment, an error is reported.

Copy

```
<property 
    as="xs:string"  
    name="oxf.fr.detail.attachment.max-size-aggregate-per-form.*.*"                      
    value="10000000"/>
```

The value of `oxf.fr.detail.attachment.max-size-aggregate-per-form` can be overridden:

* for a specific form, from the Form Builder "Form Settings" dialog

This property was previously named `oxf.fr.detail.attachment.max-size-aggregate`. The old name is still supported for backward compatibility.

### Maximum aggregate attachment size (controls) <a href="#maximum-aggregate-attachment-size-controls" id="maximum-aggregate-attachment-size-controls"></a>

The following property sets the maximum aggregate size in bytes of all uploaded attachments for each individual attachment control. This property will typically be used to limit the total size of attachments for multiple attachment controls, although it will also be checked for single attachment controls. If you attempt to upload a larger attachment, an error is reported.

```
<property 
    as="xs:string"  
    name="oxf.fr.detail.attachment.max-size-aggregate-per-control.*.*"                      
    value="10000000"/>
```

The value of `oxf.fr.detail.attachment.max-size-aggregate-per-control` can be overridden:

* for a specific control, from the Form Builder "Control Settings" dialog

### Allowed file types <a href="#allowed-file-types" id="allowed-file-types"></a>

The following property specifies which file types (also known as "mediatypes") are allowed for attachments. For example, the following values of `image/png image/jpeg` specify that JPEG images and PDF files are allowed but no other files.

```
<property 
    as="xs:string"  
    name="oxf.fr.detail.attachment.mediatypes.*.*"                    
    value="image/jpeg application/pdf"/>
```

The format is as follows:

* the value is a list of space- or comma-separated mediatype ranges
* a mediatype range is one of:
  * `*/*`: all mediatypes allowed
  * `type/*`: all mediatypes with prefix `type` are allowed (for example `image/*`)
  * `type/subtype`: specific mediatype such as `image/jpeg`, `application/atom+xml`, `video/mp4`, etc.

The value of `oxf.fr.detail.attachment.mediatypes` can be overridden:

* for a specific form, from the Form Builder "Form Settings} dialog
* for a specific control, using a common constraint the Form Builder "Control Settings" dialog
