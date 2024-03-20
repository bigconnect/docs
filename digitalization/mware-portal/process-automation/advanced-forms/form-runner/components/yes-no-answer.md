# Yes/No answer

The `fr:yesno-input` component displays a simple choice between "Yes" and "No". It stores a value of `true` when "Yes" is selected and `false` when "No" is selected.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC36F3r1v1Dj3m87z%252Fxbl-yesno-input.png%3Fgeneration=1528743350480727%26alt=media\&width=768\&dpr=4\&quality=100\&sign=d4532b3bd0ecea4e7a1449ba058da674078d9a5ea8ace41d51fb2d6675499b09)

Appearance

This control is generally recommended over the Checkbox Input Component for binary (or boolean) selection, because:

* A "Yes/No" choice is usually clearer to the user.
* The control can be marked as "Required", in which case he user has to make a selection to pass validation. With a single checkbox, it is easier to miss making a selection.

A "Clear value" button is available in Form Builder to clear the value of the control without editing the source of the form definition or recreating the control.

### Basic usage <a href="#basic-usage" id="basic-usage"></a>

You use `fr:yesno-input` like a regular XForms controls. Example:

```
<fr:yesno-input ref="happy-camper">
    <xf:label>Are you a happy camper?</xf:label>
</fr:yesno-input>
```
