# Image

The `fr:image` component shows an image. Unlike fr:image-attachment, `fr:image` doesn't have an associated upload control and cannot be changed by the end-user of a form.

### Basic usage <a href="#basic-usage" id="basic-usage"></a>

```
<fr:image bind="my-image-bind">
    <xf:label ref="$form-resources/my-image/label"/>
</fr:image>
```

The URL of the image is stored in instance data via a single-node binding, here using `bind`.

### Form Runner usage <a href="#form-runner-usage" id="form-runner-usage"></a>

When in the Form Runner context, `fr:image` retrieves the resource while passing the `Forms-Form-Definition-Version` HTTP header when available.

