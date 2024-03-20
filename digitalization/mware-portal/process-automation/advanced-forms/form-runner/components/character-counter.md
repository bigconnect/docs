# Character counter

The `fr:character-counter` component encapsulates an existing text control and adds a character counter next to it.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-41fae014a8f9ebf378f26fe59db7709d15de994e%252Fcharacter-counter-control.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=2dc9ad27f07a1f38ae4545d5f2cb3bf94130bd1e9b09add96a30029f7b5efc82)

Input field with character counter

It supports the following controls:

* `xf:input`
* `xf:textarea`
* `xf:secret`
* `fr:tinymce`

### Basic usage <a href="#basic-usage" id="basic-usage"></a>

The component is generally activated using the `appearance` attribute:

```
<fr:input bind="message-bind" appearance="character-counter">
    <xf:label>Message</xf:label>
</fr:input>
```

The component automatically encapsulates the specified control, here `xf:input`.

The component displays its counter in different ways:

* if no `max-length` validation is present on the bound node
  * the counter value is the current number of characters
  * a CSS class `fr-charcounter-current` is present on the counter
* if a `max-length` validation is present on the bound node
  * if the current length is smaller than `max-length`
    * the counter value is the number of remaining characters allowed
    * a CSS class `fr-charcounter-remaining` is present on the counter
  * if the current length is larger than `max-length`
    * the counter value is the number of extra characters allowed
    * a CSS class `fr-charcounter-over` is present on the counter

By default, `fr-charcounter-over` places a minus sign in front of the number of characters over the limit.

### Form Builder support <a href="#form-builder-support" id="form-builder-support"></a>

The CSS bindings allow Form Builder to propose the "Character Counter" appearance to the user automatically.
