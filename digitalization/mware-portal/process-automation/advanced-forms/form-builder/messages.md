# Messages

Some of the [Form Runner actions](https://doc.orbeon.com/form-runner/advanced/buttons-and-processes/actions-form-runner) display messages to users. For instance, when a form is saved, a message is displayed to confirm that the form was saved successfully. The Messages dialog allows you to customize those messages directly in Form Builder, instead of using properties such as `oxf.fr.resource.*.*.en.detail.messages.save-success`.

### Usage <a href="#usage" id="usage"></a>

You can open the Messages dialog by clicking on the "Messages" button, under the "Advanced" tab, in the toolbox.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-6792fc8450886ca68dd55af58966e8f411c03d84%252Fadvanced-menu.png%3Falt=media\&width=300\&dpr=4\&quality=100\&sign=c039813767652bbadfcfb4099a826517472a86b8f50e0350b4ca02727157c4d0)

Each message must have a language, a name, and a value.

The message name can be one of the pre-defined names present in [Form Runner's `resources.xml`](https://github.com/orbeon/orbeon-forms/blob/master/form-runner/jvm/src/main/resources/apps/fr/i18n/resources.xml) (e.g. `save-success` or `upload-error`) or a custom name, which can be entered by selecting "Other".

When a message is defined in the Messages dialog, it overrides any message defined in a property.
