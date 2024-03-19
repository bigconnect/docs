# Video attachment

This component represents an attachment to a form which has to contain a video. The video is displayed by the component when attached.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-40f4247e26210bdaf836ca8f4bebf7d7ec5154c2%252Fxbl-video-attachment.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=1a9c9b8e00639fbf0ed8025de741b816bc5a793d8f8767ece5c08f729cf5d7ef)

Video attachment component

### Accepted mediatypes <a href="#accepted-mediatypes" id="accepted-mediatypes"></a>

By default the component accepts `video/*` mediatypes.

If an [`upload-mediatypes` custom constraint](https://doc.orbeon.com/xforms/xpath/extension-functions/extension-validation#xxfupload-mediatypes) is present and not blank, it is used applied to the enclosed upload control. If missing, the default is `video/*`.

### Events <a href="#events" id="events"></a>

This component dispatches the following events following the enclosed upload control:

* `xxforms-upload-start`
* `xxforms-upload-cancel`
* `xxforms-upload-done`
* `xxforms-upload-error`
