# Attachment

This component represents an attachment to a form.

![The attachment control with multiple selection enabled](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-0d61b78a407ed523a9e255426c98c428fcb53e59%252Fxbl-attachment-multiple.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=06fc0b528a15704d47b52587caf2ac3b12477a1589859fd2ffb50d29ce226fa8)

### Accepted mediatypes <a href="#accepted-mediatypes" id="accepted-mediatypes"></a>

By default the component accepts any mediatype.

If an `upload-mediatypes` custom constraint is present and not blank, it is applied to the enclosed upload control.

Otherwise, if an `accept` attribute or associated `oxf.xforms.xbl.fr.attachment.accept` property is defined and not blank, it is used. Example of system-wide property:

```
<property
    as="xs:string"  
    name="oxf.xforms.xbl.fr.attachment.accept"
    value="application/pdf"/>
```

Example of attribute on the control:

```
<fr:attachment
    bind="my-file"
    accept="application/pdf">
```

Otherwise, any mediatype is allowed.

### Support for multiple files <a href="#support-for-multiple-files" id="support-for-multiple-files"></a>

The `multiple="true"` attribute enables support for multiple files:

* The file selector allows multiple selection.
* The drop area (see below) allows multiple file being dropped at the same time or serially.
* A reorderable list of multiple files shows.

### Drag and drop of files <a href="#drag-and-drop-of-files" id="drag-and-drop-of-files"></a>

On supported browsers, you can drag and drop files from other applications (typically an operating system file explorer) to the drop area instead of selecting files with the file selector button.

### Disallowing attachment download <a href="#disallowing-attachment-download" id="disallowing-attachment-download"></a>

By default, the user can download back the attached files. In some cases, this is not desirable. The `allow-download="false"` attribute allows disabling downloads.

```
<fr:attachment
    id="attachments-control"
    bind="attachments-bind"
    multiple="true"
    allow-download="false"
    class="fr-attachment">
    ...
</fr:attachment>
```

Form Builder offers an option in the "Control Settings" dialog.

![Allow download option](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252Fe3b3b27281ff12859e9aa853f6feb3e3da85ac86.png%3Fgeneration=1594231957180258%26alt=media\&width=768\&dpr=4\&quality=100\&sign=e180df777a761cf97139db2a75e3348b96344713db21737387ba47f0cb8ea25a)



![With download allowed](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-0d61b78a407ed523a9e255426c98c428fcb53e59%252Fxbl-attachment-multiple.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=06fc0b528a15704d47b52587caf2ac3b12477a1589859fd2ffb50d29ce226fa8)



![With download disallowed](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-88b3efa311ff3ae3fa9426332e96fccc5a298d0f%252Fxbl-attachment-multiple-nodownload.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=13aaee8d1c20bfc55a9c92d69f0f304ec38e66b84ca55dcf24c9993775dc8390)



### Mediatype determination <a href="#mediatype-determination" id="mediatype-determination"></a>

Web browsers determine a mediatype (AKA content type or MIME type) for uploaded files. This determination is platform-dependent, not necessarily precise, and sometimes missing.

In all cases, the mediatype provided by the browser cannot be trusted and is indicative only. You can use the file scan API to reject files that do not satisfy the indicated mediatype.

When the browser does not provide a mediatype, or provides a generic mediatype such as `application/octet-stream`, Advanced Forms uses the file extension, when present, to guess a mediatype.

This means for example that a `.docx` file will map to `application/vnd.openxmlformats-officedocument.wordprocessingml.document` even if the browser is unaware of Microsoft Word files.

### Events <a href="#events" id="events"></a>

This component dispatches the following events following the enclosed upload control:

* `xxforms-upload-start`
* `xxforms-upload-cancel`
* `xxforms-upload-done`
* `xxforms-upload-error`
