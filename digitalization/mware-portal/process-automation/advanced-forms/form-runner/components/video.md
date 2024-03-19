# Video

The `fr:video` component shows a video. Like [fr:image](https://doc.orbeon.com/form-runner/component/image), `fr:video` doesn't have an associated upload control and cannot be changed by the end-user of a form.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-ad707c2954011154ebc9ffab64fcd39192b0e6be%252Fxbl-video.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=655e4eaefa7f0ac78d1c0fa9d26f0ff6d9bbdc6e390a7983ed4cd7930b91addf)

Video component

### Basic usage <a href="#basic-usage" id="basic-usage"></a>

Copy

```
<fr:video bind="my-video-bind">
    <xf:label ref="$form-resources/my-video/label"/>
</fr:video>
```

The URL of the video is stored in instance data via a single-node binding, here using `bind`.

### Form Runner usage <a href="#form-runner-usage" id="form-runner-usage"></a>

When in the Form Runner context, `fr:video` retrieves the resource while passing the `Orbeon-Form-Definition-Version` HTTP header when available.

### Limitations <a href="#limitations" id="limitations"></a>

In PDF output, the Video component currently doesn't show any content. Ideally, it should show a still of the video.
