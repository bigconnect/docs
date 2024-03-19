# Section and grid settings

### Basic settings and formulas <a href="#basic-settings-and-formulas" id="basic-settings-and-formulas"></a>

#### Overview <a href="#overview" id="overview"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-11604b458c173c3982e89da1966b210e98fb0056%252Fsection-settings.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=3cfd2923875262a890477ab9073a5c426f73737e127d37de5bcac707d088b68e)

#### Section or grid name <a href="#section-or-grid-name" id="section-or-grid-name"></a>

Each section or grid has a _name_ or identifier, which determines how data is represented in XML. The name name specifies an identifier for the section or grid which is unique in the entire form. If no name is explicitly specified, Form Builder assigns a default name, such as "section-1" or "grid-1".

A section or grid name can be changed, provided it doesn't collide with another control name (an error will show otherwise).

\[SINCE Orbeon Forms 2019.1]

See [Renaming of controls and formulas](https://doc.orbeon.com/form-builder/formulas#renaming-of-controls-and-formulas).

#### Page breaks <a href="#page-breaks" id="page-breaks"></a>

When producing PDF automatically, the "Page break before" checkbox, when enabled, ensures that the given section starts at the top of a new page.

#### Custom CSS classes <a href="#custom-css-classes" id="custom-css-classes"></a>

The "Custom CSS classes" field allows adding CSS classes which will be placed on the control in the resulting HTML. This can be used for custom styling.

See [Control Settings](https://doc.orbeon.com/form-builder/form-editor/control-settings#custom-css-classes) for more about custom CSS classes.

#### Formulas <a href="#formulas" id="formulas"></a>

* **Visibility:** Boolean expression specifying whether the section or grid is visible.
  * If this field is left blank, then the section or grid is always visible, unless an enclosing section is not visible.
  * Otherwise, it is visible only if the result of the Boolean expression is `true()`.
* **Read-Only:** Boolean expression specifying whether the section or grid is read-only (not editable).
  * If this field is left blank, then the section or grid is editable unless an enclosing section is read-only.
  * Otherwise, it is editable only if the result of the Boolean expression is `false()`.

XPath expressions are described in more details in [Formulas](https://doc.orbeon.com/form-builder/formulas).

### Repeat settings <a href="#repeat-settings" id="repeat-settings"></a>

See [Repeat settings](https://doc.orbeon.com/form-builder/form-editor/repeat-settings).

### Settings specific to sections <a href="#settings-specific-to-sections" id="settings-specific-to-sections"></a>

See [Section settings](https://doc.orbeon.com/form-builder/form-editor/section-settings).
