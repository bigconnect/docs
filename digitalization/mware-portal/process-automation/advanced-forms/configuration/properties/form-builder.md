# Form Builder

### Default values <a href="#default-values" id="default-values"></a>

For the latest default values of Form Builder properties, see `properties-form-builder.xml`.

### Toolbox <a href="#toolbox" id="toolbox"></a>

#### Groups of controls <a href="#groups-of-controls" id="groups-of-controls"></a>

You configure the contents of the toolbox by configuring properties in this format:

```
<property as="xs:string"  name="oxf.fb.toolbox.group.$GROUPNAME.uri.*.*">
    $URLS
</property>
```

In that property, the value of `$GROUPNAME` determines a grouping of the controls in the toolbox. The controls are defined by a list of XBL file URLs specified by `$URLS`. For example:

```
<property as="xs:string" name="oxf.fb.toolbox.group.text.uri.*.*">
    oxf:/forms/forms/builder/xbl/text-controls.xbl
    oxf:/xbl/forms/tinymce/tinymce.xbl
    oxf:/xbl/forms/explanation/explanation.xbl
</property>
```

```
<property
    as="xs:string"
    name="oxf.fb.toolbox.group.text.uri.*.*"
    value="oxf:/forms/forms/builder/xbl/text-controls.xbl
           oxf:/xbl/forms/tinymce/tinymce.xbl
           oxf:/xbl/forms/explanation/explanation.xbl"/>
```

To properly show up in the toolbox, XBL files need to include the appropriate component metadata.

#### Other toolbox features <a href="#other-toolbox-features" id="other-toolbox-features"></a>

In addition to controls, the toolbox has other features which you can enable or disable with the following properties:

```
<property as="xs:boolean" name="oxf.fb.menu.schema"                  value="true"/>
<property as="xs:boolean" name="oxf.fb.menu.pdf"                     value="true"/>
<property as="xs:boolean" name="oxf.fb.menu.edit-source"             value="true"/>
<property as="xs:boolean" name="oxf.fb.menu.permissions"             value="true"/>
<property as="xs:boolean" name="oxf.fb.menu.permissions.owner-group" value="true"/>
<property as="xs:boolean" name="oxf.fb.menu.services.http"           value="true"/>
<property as="xs:boolean" name="oxf.fb.menu.services.database"       value="true"/>
<property as="xs:boolean" name="oxf.fb.menu.actions"                 value="true"/>
```

* `oxf.fb.menu.permissions.owner-group` allows you, by setting the value of the property to `false`, to disable the two lines in the permissions dialog related to owner and group permissions. This is useful if you're always using a custom implementation of the persistence API that doesn't support owner and group permissions and you prefer to hide those options from form authors to avoid possible confusions.

### Publish dialog <a href="#publish-dialog" id="publish-dialog"></a>

#### Links to new and summary pages <a href="#links-to-new-and-summary-pages" id="links-to-new-and-summary-pages"></a>

By default, the Publish dialog proposes, upon successful publication of a form definition, shortcuts to navigate to the published form's New or Summary pages.

![Publish dialog](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-3cee6d5d512ac59cdfd98eb1ce52b2639ce9d2b6%252Fpublish-initial-after.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=7db798e79a79137fa2329c9eabd9b11ace74f813808add8db6db434c6087accc)

This property allows specifying which of these actions are available:

```
<property
    as="xs:string"
    name="oxf.fb.publish.buttons"
    value="new summary"/>
```

#### The Version dropdown <a href="#the-version-dropdown" id="the-version-dropdown"></a>

When versioning is supported, you can use the following property to control the behavior of the Version dropdown. If versioning isn't supported, this property has no effect. Also, the property has no effect if no version of this form has ever been published: in that case, version 1 will be published. The property can be set to:

* `default-to-select` – The dropdown is enabled, and users need to select whether they want to create a new version or overwrite an existing version. This is the default.
* `default-to-next` – The dropdown is enabled, and defaults to the choice to create a new version.
* `default-to-latest` – The dropdown is enabled, and defaults to overwriting the latest version.
* `force-next` – The dropdown is read-only, and a new version will be created on publish.
* `force-latest` – The dropdown is read-only, and the latest published version will be overwritten on publish.

```
<property
    as="xs:string"
    name="oxf.fb.publish.version"
    value="default-to-select"/>
```

### Maximum number of columns <a href="#maximum-number-of-columns" id="maximum-number-of-columns"></a>

```
<property
    as="xs:integer"
    name="oxf.fb.grid.max-columns"
    value="4"/>
```

Use this property to change the default maximum number of grid columns form authors can create. The more columns there are, the more narrow each column is, and when columns become too narrow, some less "elastic" controls might not have enough space to render properly. You want to set this property to a "reasonable" value to reduce the chance of form authors ending up with columns that are too narrow to accommodate certain controls.

### Closing sections <a href="#closing-sections" id="closing-sections"></a>

```
<property
    as="xs:integer"
    name="oxf.fb.section.close"
    value="100"/>
```

Closing sections in Form Builder can improve responsiveness. This property sets the number of controls after which Form Builder will close all sections except the first one when loading a form. Below that number of controls, all sections are open by default.

### Action buttons in Form Builder <a href="#action-buttons-in-form-builder" id="action-buttons-in-form-builder"></a>

#### Action buttons on the Form Builder summary page <a href="#action-buttons-on-the-form-builder-summary-page" id="action-buttons-on-the-form-builder-summary-page"></a>

This controls which buttons appear on the Form Builder summary page.

```
<property as="xs:string"  name="oxf.fr.summary.buttons.forms.builder">
    home delete duplicate new
</property>
```

#### Action buttons on the Form Builder detail page <a href="#action-buttons-on-the-form-builder-detail-page" id="action-buttons-on-the-form-builder-detail-page"></a>

This controls which buttons appear on the Form Builder detail page.

```
<property as="xs:string"  name="oxf.fr.detail.buttons.forms.builder">
    summary new test publish save
</property>
```

### Available languages <a href="#available-languages" id="available-languages"></a>

```
<property
    as="xs:string"
    name="oxf.fr.available-languages.forms.builder"
    value="en fr es it de"/>
```

This controls which Form Builder user interface languages appear in the language selector.

### Permissions dialog <a href="#permissions-dialog" id="permissions-dialog"></a>

The permissions dialog allows form authors, amongst other things, to assign permissions to users having a specific roles. In that dialog, form authors can type in a text field the name of the role(s) they want to assign those permissions to. If you're systematically using certain roles, want to save form authors from having to type them, and prevent possible mistakes in the process, you can use the following property to list the role names you want the Form Builder permissions dialog to always show in that dialog:

```
<property as="xs:string" name="oxf.fb.permissions.role.always-show">
    ["Organization Owner"]
</property>
```

The value of this property is an array of strings in the JSON format. For instance, the following screenshot shows the dialog with the above property set (see the line for "Organization owner"), and where the form author added a line for an "admin" role.

![Permissions dialog with Organization Owner role](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-64f93296abf942a3187e71811dfb8c3d1fc87ccb%252Ffb-permissions-organization-owner.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=451d77818d8d3d78ff904ce2fc9140ac4c3585cbd191d68930da51d228de495c)

### Access control <a href="#access-control" id="access-control"></a>

If you'd like to have multiple classes of Form Builder users where some can edit, say, forms in the `hr` app, while others can edit forms in the `sales` app, see Access control for editing forms.

### Formatted text configuration <a href="#formatted-text-configuration" id="formatted-text-configuration"></a>

You can configure the TinyMCE editor used for Formatted Text (AKA Rich Text Editor) component for the following:

* Explanatory Text content in the form area
* control labels with "Use HTML" in Form Builder dialogs
* control hints with "Use HTML" in Form Builder dialogs
* control help with "Use HTML" texts in Form Builder dialogs

Use the following property:

Copy

```
<property as="xs:string"  name="oxf.xforms.xbl.fr.tinymce.config.forms.builder">
    {{
        "inline"             : false,
        "hidden_input"       : false,
        "language"           : "en",
        "statusbar"          : false,
        "menubar"            : false,
        "plugins"            : "lists link fullscreen",
        "toolbar"            : "bold italic | bullist numlist outdent indent | link fullscreen",
        "browser_spellcheck" : true,
        "doctype"            : "&lt;!DOCTYPE html PUBLIC \"-//W3C//DTD XHTML 1.0 Transitional//EN\" \"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd\">",
        "encoding"           : "xml",
        "entity_encoding"    : "raw",
        "forced_root_block"  : "div",
        "verify_html"        : true,
        "visual_table_class" : "fr-tinymce-table",
        "skin"               : false,
        "convert_urls"       : false,
        "content_css"        : "default"
    }}
</property>
```

The example above switches to the `iframe` mode (instead of the `inline` mode) and adds the fullscreen plugin.

For backward compatibility, if a non-blank `oxf.fb.tinymce.config` property is present, it will be used.

Note that `oxf.xforms.xbl.fr.tinymce.config.forms.builder` is evaluated as an AVT, while `oxf.fb.tinymce.config` is not.

### Explanatory Text TinyMCE configuration <a href="#explanatory-text-tinymce-configuration" id="explanatory-text-tinymce-configuration"></a>

Prefer the `oxf.xforms.xbl.fr.tinymce.config.forms.builder` property above.

Note that `oxf.xforms.xbl.fr.tinymce.config.forms.builder` controls the appearance of the Explanatory Text, as well as labels, hints, and help texts in Form Builder dialogs, while `oxf.fb.tinymce.config` only controls the appearance of the Explanatory Text.

When form authors edit an Explanatory Text, Form Builder utilizes the TinyMCE component. You can [configure TinyMCE](https://www.tiny.cloud/docs/configure/) by supplying your own configuration in JSON as the value for the `oxf.fb.tinymce.config` property. If you do not set the `oxf.fb.tinymce.config` property, Form Builder defaults to a configuration, which can be found at the bottom of `TinyMce.scala`.

Copy

```
<property as="xs:string"  name="oxf.fb.tinymce.config">
    {
        "mode"              : "exact",
        "language"          : "en",
        ...
    }
</property>
```
