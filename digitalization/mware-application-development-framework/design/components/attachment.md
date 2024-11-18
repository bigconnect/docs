# Attachment

The `Attachment` component allows your users to upload files to your app.

Uploaded files are stored in MinIO.

<figure><img src="https://files.readme.io/27adfae-attachment.png" alt="" width="375"><figcaption></figcaption></figure>

The `Attachment` component works especially well with DB's Attachment data type.

> ### 📘Filesize
>
> Remember the maximum file size you can store in your database of choice

### Adding an Attachment component

The `Attachment component` requires a [Form](https://docs.budibase.com/docs/forms) component as a parent.

To add the Attachment component, click `Form > Attachment`.

<figure><img src="https://files.readme.io/59f9171-Screenshot_2022-09-12_at_15.53.36.png" alt=""><figcaption></figcaption></figure>

After the `Attachment` component is added you can immediately start using it, no further configuration is required.

<figure><img src="https://files.readme.io/3b9c6eb-extensions.png" alt=""><figcaption></figcaption></figure>

### Manage access

If you want to specify user roles such as allowing public file uploads, then you'll need to adjust the write access levels of your table.

### Validation

There are few Form validation options for the attachment field.

| Constraint                 | Description                                                                       |
| -------------------------- | --------------------------------------------------------------------------------- |
| Required                   | At least one file must be uploaded.                                               |
| Max file size (MB)         | No uploaded file can exceed the specified limit in megabytes.                     |
| Max total upload size (MB) | The total size of all uploaded files cannot exceed the specified megabytes limit. |

<figure><img src="https://files.readme.io/5c1fbee-image.png" alt=""><figcaption></figcaption></figure>

### Specifying acceptable file extensions

By default any file, with any file extension, will be accepted by the Attachment component for upload. However, in certain scenarios, a particular set of file extensions may preferred. For example, you may only want to accept PDF files. In other situations, you may only want image files.

The _extensions_ property accepts an **array** of file extensions, e.g. `[".webp",".png"]`. When Using Handlebars , make sure to split the comma separated array. If Using JavaScript, simply return an array of extensions as shown below.

HandlebarsJavaScript

```html
{{ split ".png,.jpg,.jpeg,.svg,.gif,.webp" }}
```

This will result in the operating system's native File Open Dialog having a corresponding custom filter:

<figure><img src="https://files.readme.io/9883a2e-extensions.png" alt="Customised file extensions filter in the Attachment component" width="400"><figcaption><p>Customised file extensions filter in the Attachment component</p></figcaption></figure>
