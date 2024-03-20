# Video attachment

This component represents an attachment to a form which has to contain a video. The video is displayed by the component when attached.

<figure><img src="../../../../../../.gitbook/assets/image (1).jpg" alt=""><figcaption><p>Video attachment component</p></figcaption></figure>

### Accepted mediatypes <a href="#accepted-mediatypes" id="accepted-mediatypes"></a>

By default the component accepts `video/*` mediatypes.

If an `upload-mediatypes` custom constraint is present and not blank, it is used applied to the enclosed upload control. If missing, the default is `video/*`.

### Events <a href="#events" id="events"></a>

This component dispatches the following events following the enclosed upload control:

* `xxforms-upload-start`
* `xxforms-upload-cancel`
* `xxforms-upload-done`
* `xxforms-upload-error`
