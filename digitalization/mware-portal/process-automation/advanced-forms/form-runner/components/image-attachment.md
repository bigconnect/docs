# Image attachment

This component represents an attachment to a form which has to contain an image. The image is displayed by the component when attached.

### Accepted mediatypes <a href="#accepted-mediatypes" id="accepted-mediatypes"></a>

By default the component accepts `image/*` mediatypes.

\[SINCE Orbeon Forms 2017.1]

If an [`upload-mediatypes` custom constraint](https://doc.orbeon.com/xforms/xpath/extension-functions/extension-validation#xxfupload-mediatypes) is present and not blank, it is used applied to the enclosed upload control. If missing, the default is `image/*`.

### Events <a href="#events" id="events"></a>

\[SINCE Orbeon Forms 2017.1]

This component dispatches the following events following the enclosed upload control:

* `xxforms-upload-start`
* `xxforms-upload-cancel`
* `xxforms-upload-done`
* `xxforms-upload-error`

### Image transformation <a href="#image-transformation" id="image-transformation"></a>

\[SINCE Orbeon Forms 2022.1]

The component can transform the uploaded image. The following attributes control the transformation:

* `transform-max-width`:
  * no attribute: use the default (form-level then property)
  * blank attribute value: don't constrain the image width
  * positive integer: scale the image width to the given value if the image is larger
  * the image's aspect ratio is preserved when used in conjunction with `transform-max-height`
* `transform-max-height`:
  * no attribute: use the default (form-level then property)
  * blank attribute value: don't constrain the image width
  * positive integer: scale the image hight to the given value if the image is larger
  * the image's aspect ratio is preserved when used in conjunction with `transform-max-width`
* `transform-format`:
  * no attribute: use the default (form-level then property)
  * blank attribute value: preserve the input format
  * `jpg`: force the result to JPEG
  * `png`: force the result to PNG
* `transform-quality`:
  * no attribute: use the default (form-level then property)
  * blank attribute value: use the default compression quality (80)
  * for JPEG compression only
  * value between 1 (worst) and 100 (best), but typically between 50 and 90

The following matching properties allow setting global defaults. For example:

Copy

```
<property 
    as="xs:string"
    name="oxf.xforms.xbl.fr.image-attachment.transform-max-width.*.*"
    value="1024"/>
<property 
    as="xs:string"
    name="oxf.xforms.xbl.fr.image-attachment.transform-max-height.*.*"
    value="768"/>
<property 
    as="xs:string"
    name="oxf.xforms.xbl.fr.image-attachment.transform-format.*.*"
    value="jpg"/>
<property 
    as="xs:string"
    name="oxf.xforms.xbl.fr.image-attachment.transform-quality.*.*"
    value="90"/>
```

### Form Builder customer settings <a href="#form-builder-customer-settings" id="form-builder-customer-settings"></a>

When used in Form Builder, the following custom settings for image transformation are available:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-7212ee532dfe67b3ee3a539f70e342a2d9487c9c%252Fcustom-settings-image-attachment.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=49579cc4cbe65d0a26988894a9bf6d08ce3e71497e1c04d8452eab0befc5327f)

Custom settings
