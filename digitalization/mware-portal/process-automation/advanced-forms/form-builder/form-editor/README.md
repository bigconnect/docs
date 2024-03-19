# Form editor

### Creating your first form definition <a href="#creating-your-first-form-definition" id="creating-your-first-form-definition"></a>

After pressing the "New" button on the [summary page](https://doc.orbeon.com/form-builder/summary-page), you reach the form editor (also known as form designer) proper. Form Builder requires that you provide two small pieces of information before starting:

* **Application Name.** Each form definition has an _application name_ which identifies a group of forms that belong together. In practice, an application name might correspond to a project, or a department, or a company. For example, all forms built by Orbeon have the "orbeon" application name. _NOTE: Depending on your permissions, the application name might have been picked for you already, or you might have the choice of a restricted set of application names._
* **Form Name.** Each form definition also has a _form name_, which identifies a unique form name within a particular application.

![Form Settings for a New Form Definition](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-8e76c9bd5d6ae10888cd4faef69feade4d12682e%252Fform-settings-new.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=4376f15ce5e86fa2f8d17422b5b284555a3e4359a859676de441f9bac692fe3e)

Both application name and form name must respect a certain format:

* Use letters and optionally numbers and separators like "-" and "\_"
* Start with a letter
* Do not use spaces

_NOTE: One reason for these restrictions is that the names will eventually appear in your browser's URL bar when the form is deployed and it is better to have clean-looking URLs._

For your first form, don't worry too much about those: just enter any name! You can always change these settings later!

You also may enter at this time a form title and description.

### General Form Builder layout <a href="#general-form-builder-layout" id="general-form-builder-layout"></a>

Form Builder's form editor is organized in 4 areas:

* **Top:** title area with status information and selection of the Form Builder user interface language.
* **Left:** the [toolbox](https://doc.orbeon.com/form-builder/form-editor/toolbox), which might scroll vertically depending on your browser or monitor size.
* **Bottom:** status icons and buttons.
* **Center:** WYSIWYG [form area](https://doc.orbeon.com/form-builder/form-editor/form-area) where you actually edit your form definition. This area might scroll vertically depending on your browser or monitor size or the size of the form you are editing.

![General Layout of Form Builder](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-8b4b08f855737f5484d27c2a7355c15d4edaa826%252Fgeneral-layout.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=bcdb5a80d51bb97cc989bd3ffc4a21fcabdfa001d8b17fe6bbc0f84e954311de)

### The top bar <a href="#the-top-bar" id="the-top-bar"></a>

On the top right corner of the top bar, you can change the Form Builder user interface language by clicking on one of the languages available. Currently, English and French are available. More languages may be added in the future.

Changing the language immediately updates the Form Builder user interface: you do not even need to save your form prior to using this feature.

_NOTE: This feature controls the language of the Form Builder user interface only. For localization of the form definition you are currently editing, see documentation below._

![Form Builder's Top Bar](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBtUMUx124SONFYs\_%252Ftop.png%3Fgeneration=1528743308704132%26alt=media\&width=768\&dpr=4\&quality=100\&sign=2c09381f347bb568061a8c5557002df7cf7cd12c642f88fa72abc774f43ee710)

### The buttons bar <a href="#the-buttons-bar" id="the-buttons-bar"></a>

See [Buttons bar](https://doc.orbeon.com/form-builder/form-editor/buttons-bar).

### Keyboard shortcuts <a href="#keyboard-shortcuts" id="keyboard-shortcuts"></a>

You can use the following keyboard shortcuts:

macOS:

* `⌘-S`: save the form definition (like the "Save" button)

All operating systems:

* `⌃-S`: save the form definition (like the "Save" button)

#### Other functions <a href="#other-functions" id="other-functions"></a>

In addition, the bottom bar displays status information, such as whether the current form definition was successfully saved.
