# Toolbox

The Form Builder toolbox provides the following:

* **Global icons:**
  * Form Settings
  * Cut/copy/paste
  * Undo/redo
  * "Reload toolbox"
* **Form structure:**
  * New Section
  * New Grid
  * New Repeated Grid
* **Form controls:**
  * Form control (also known as form fields) you can add to your form.
* **Metadata:**
  * Allows you to modify the application name and form name.
* **Advanced:**
  * Includes advanced features like XML Schema, PDF and source code view.
* **Services and Actions:**
  * Editors for simple services and actions.

Depending on your monitor or browser size, you can use the scrollbar to the right of the toolbox to see more toolbox content.

### Configuration of the toolbox <a href="#configuration-of-the-toolbox" id="configuration-of-the-toolbox"></a>

The toolbox can be configured via:

* Properties: for selecting and ordering the components that show in the toolbox.
* Component metadata: for the component names.

### Cut, copy and paste <a href="#cut-copy-and-paste" id="cut-copy-and-paste"></a>

See Cut, copy and paste.

### Undo and redo <a href="#undo-and-redo" id="undo-and-redo"></a>

See Undo and redo.

### Reloading the toolbox <a href="#reloading-the-toolbox" id="reloading-the-toolbox"></a>

#### Function <a href="#function" id="function"></a>

The "Reload Toolbox" icon or button reloads all the components in the toolbox. After reloading:

* The list of controls in the toolbox updates to reflect changes to the `oxf.fb.toolbox.group` properties.
* The section templates libraries update.
* Section templates used in your form also update to their latest published version.
* The list of all available (published) _versions_ of section templates is updated.
* The currently-selected versions of section templates, if still available, are updated to reflect their latest published.

The "Reload Toolbox" button is located at the top of the "Advanced" tab.

<figure><img src="https://doc.orbeon.com/~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-6792fc8450886ca68dd55af58966e8f411c03d84%252Fadvanced-menu.png%3Falt=media&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=c039813767652bbadfcfb4099a826517472a86b8f50e0350b4ca02727157c4d0" alt=""><figcaption></figcaption></figure>

#### New Section button <a href="#new-section-button" id="new-section-button"></a>

Pressing this button inserts a new section into the form. The section is inserted after the currently selected section, that is the section containing the currently selected control.

After insertion, the new section has an empty title. You can change the section title by clicking on it.

#### New Grid button <a href="#new-grid-button" id="new-grid-button"></a>

Pressing this button inserts a new grid into the form. The grid is inserted after the currently selected grid within the currently selected section, that is the section and grid containing the currently selected control.

#### New Repeated Grid button <a href="#new-repeated-grid-button" id="new-repeated-grid-button"></a>

Pressing this button inserts a new repeated grid into the form. The grid is inserted after the currently selected grid within the currently selected section (the section and grid containing the currently selected control).

See also Repeated grids.

You can also switch a regular grid to be repeated. See Repeat settings.

### Form controls <a href="#form-controls" id="form-controls"></a>

The toolbox contains the user interface controls you can insert into your form, grouped by category:

| Category               | Description                                                                |
| ---------------------- | -------------------------------------------------------------------------- |
| **Text Controls**      | for capturing text                                                         |
| **Utility Controls**   | for descriptions and calculations                                          |
| **Typed Controls**     | for capturing other data like numbers, emails, phone numbers, etc.         |
| **Date and Time**      | for capturing a date, a time or both                                       |
| **Selection Controls** | for selecting one or more values, like dropdown menus, radio buttons, etc. |
| **Attachments**        | for attaching images or files                                              |
| **Buttons**            | for action buttons                                                         |

To add a control to your form, simply click on the control. The following insertion logic is implemented:

* If the currently selected grid cell is empty, the control is inserted there.
* Otherwise, if the cell to the right of the currently selected grid cell is empty, the control is inserted there.
* Otherwise, if the control is the last control of the grid, a new grid row is inserted and the control is inserted in the first cell of the new row.
* Otherwise, the controls in the toolbox are disabled and you cannot insert a new control.

### Drag and drop of controls <a href="#drag-and-drop-of-controls" id="drag-and-drop-of-controls"></a>

Once a form control is inserted into the form, you can drag it to a different grid cell in the form: simply click on the arrow icon that shows when you hover over the grid cell, and drag it to the new location.

You can also drag a control directly from the toolbox to a grid cell in the form.

#### Text Controls <a href="#text-controls" id="text-controls"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252F6f4bb66969e178aa725df0d000ea2fa8351c051b.png%3Fgeneration=1591201828554727%26alt=media\&width=768\&dpr=4\&quality=100\&sign=54942a8f272bdaae8dfdbdfb0052d1dcd4cebfb1e2cd57e2228461bca75e9f26)

| Name                    | Description                                    |
| ----------------------- | ---------------------------------------------- |
| **Input Field**         | single-line input field                        |
| **Plain Text Area**     | multi-line plain text area                     |
| **Formatted Text Area** | formatted text area (AKA Rich Text Editor)     |
| **Password Field**      | input field that hides the characters you type |

The controls appear like this at design time:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252F5c46f02528d0a1906d9910b154c82377d5721680.png%3Fgeneration=1591206906041788%26alt=media\&width=768\&dpr=4\&quality=100\&sign=be917667bc9163ae2d1656b288e179b8dd7fc7c36c67374dd5635ea433595060)

#### Utility Controls <a href="#utility-controls" id="utility-controls"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252F93b2ff833079fbcd3d2192175f8fceaa34a67fdd.png%3Fgeneration=1591201837384598%26alt=media\&width=768\&dpr=4\&quality=100\&sign=83fb39b5226730e02c45ac022618949aad2bb47dd0a8941f0cd3105900d3ff16)

| Name                 | Description                                                                                                                                                                                         |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Explanatory Text** | Explanatory text which does not capture a value                                                                                                                                                     |
| **Calculated Value** | Like most other controls, this control has a label and hint, but unlike most controls it is not used to enter a value. Instead, it just _shows_ a value, either fixed or calculated with a formula. |
| **Hidden Field**     | This control supports most features of regular controls, but doesn't show in the form at runtime.                                                                                                   |

#### Typed Controls <a href="#typed-controls" id="typed-controls"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252F12e1dd0e664b9dd3bb38df3a874073696ab7a826.png%3Fgeneration=1591201834516677%26alt=media\&width=768\&dpr=4\&quality=100\&sign=e591295ce5a4d865dd57667ef237e57a482c408844c060d93781e3e89532c4fc)

| Name                | Description                                                     |
| ------------------- | --------------------------------------------------------------- |
| **Number**          | number field with formatting, validation and rounding           |
| **Email Address**   | text field which validates that the content is an email address |
| **Currency**        | US dollar currency                                              |
| **US Phone Number** | US phone number with automatic formatting                       |
| **US State**        | US state selector                                               |

The controls appear like this at design time:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252Fe2acc43c59308d62e80fa5c38426cf0393e5974b.png%3Fgeneration=1591206906139236%26alt=media\&width=768\&dpr=4\&quality=100\&sign=40c3c43ab5f4789ad5563d6f3046f1e94568529f3e8b62f28e93e717ce352ab3)

#### Date and time Controls <a href="#date-and-time-controls" id="date-and-time-controls"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252F3e7433835354833328b051e1243e59acb1b1dafc.png%3Fgeneration=1591201831503206%26alt=media\&width=768\&dpr=4\&quality=100\&sign=ba013383bf16fef5e3044bc1d650f6721b9cebbcfefdfc4bed8c719541dc25b4)

| Name              | Description                                  |
| ----------------- | -------------------------------------------- |
| **Date**          | date field with date picker                  |
| **Time**          | time field                                   |
| **Date and Time** | combined date and time field                 |
| **Dropdown Date** | date chooser which uses dropdown menus       |
| **Fields Date**   | date chooser which uses separate text fields |

The controls appear like this at design time:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252Ff7eae634a5e7e78b621099320ec2aaeecdaec4a7.png%3Fgeneration=1591206906209087%26alt=media\&width=768\&dpr=4\&quality=100\&sign=cf12bdecbcc2beb726ece9d15287530151aaaf6a7c09a6b895ced4074512ad3a)

#### Selection Controls <a href="#selection-controls" id="selection-controls"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252Fe39fe48604de3108b8fe2bae79652d913c474fd7.png%3Fgeneration=1591201832705330%26alt=media\&width=768\&dpr=4\&quality=100\&sign=6e7304e11de21c76097f0bee8629bc0ec1e98de97aee4c0f739cb9f2a3e0e855)

| Name                           | Selection | Description                                                  |
| ------------------------------ | --------- | ------------------------------------------------------------ |
| **Dropdown Menu**              | single    | dropdown menu                                                |
| **Radio Buttons**              | single    | radio buttons                                                |
| **Radio Buttons with "Other"** | single    | radio buttons with "Other" option to a enter a custom option |
| **Checkboxes**                 | multiple  | checkboxes                                                   |
| **Scrollable Checkboxes**      | multiple  | scrollable checkboxes                                        |
| **Yes/No Answer**              | single    | a simple "Yes" or "No" choice                                |
| **Dynamic Data Dropdown**      | single    | dynamic dropdown bound to a REST service                     |
| **Autocomplete**               | single    | autocompletion                                               |

You edit the choices of most selection controls with the Choices editor.

The controls appear like this at design time:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252Fe09957e0cb20ceb0a70db1f1f7aa7f2b707f986e.png%3Fgeneration=1591206903283281%26alt=media\&width=768\&dpr=4\&quality=100\&sign=de46d75739f48a04f6faa8cf01649a0d0448654cd4c85cb1bd1521a6a46e39bd)

#### Attachment Controls <a href="#attachment-controls" id="attachment-controls"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252F95e112086e63dd75ad37e018c2a5e406f245a756.png%3Fgeneration=1591201833651775%26alt=media\&width=768\&dpr=4\&quality=100\&sign=1e0da93c4c4b5f46586fe85d02e681bbde24d8d930db6b15a4f0c77cb6305db7)

| Name                          | Description                                                                                                                         |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| **Single File Attachment**    | - file which can be attached to the form at design time or at runtime - the file can also be replaced or downloaded once attached   |
| **Multiple File Attachments** | - files which can be attached to the form at design time or at runtime - the files can also be replaced or downloaded once attached |
| **Image Attachment**          | - image which can be attached to the form at design time or at runtime - the image can also be changed at runtime once attached     |
| **Image**                     | - image attached to the form at design time - it is not possible to change the image at runtime                                     |
| **Image Annotation**          | - image which can be attached to the form at design time or at runtime - the image can then be annotated with graphic objects       |
| **Handwritten Signature**     | signature drawn with a mouse or touch screen                                                                                        |

The controls appear like this at design time:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252Fffb5b5f5515e477288a2e2f70821ab92807ebf4f.png%3Fgeneration=1591206896706960%26alt=media\&width=768\&dpr=4\&quality=100\&sign=b01086f8048a58633f1114044db506820af278d4e5e7f078280cff106a66af2b)

This is the Handwritten Signature control (which of course you usually leave blank at design time):

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252F60a502a9818cca66011933dee21945f66b1bd061.png%3Fgeneration=1591206905518763%26alt=media\&width=768\&dpr=4\&quality=100\&sign=f260d6b93fbfb5abf41189f0d65cafcef421fdc480a2f9fa4c777420558f92d9)

#### Button Controls <a href="#button-controls" id="button-controls"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252Ff06f994a2066512aa044106e7508332bd2d6b685.png%3Fgeneration=1591201831881130%26alt=media\&width=768\&dpr=4\&quality=100\&sign=fb413dee2c58d236a542c9c8883b6d5b09a3b2fb76e206874c6ce63e73e5808c)

| Name            | Description                |
| --------------- | -------------------------- |
| **Button**      | standard button            |
| **Link Button** | button appearing as a link |

Buttons do not allow entering data, and by default do nothing significant, but they can be used to trigger actions with the Actions Editor.

_NOTE: Buttons do not appear at all in preview mode._

The controls appear like this at design time:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBvkY0kfVHvXAtbos%252Fbutton-controls-design-time.png%3Fgeneration=1528743343301240%26alt=media\&width=768\&dpr=4\&quality=100\&sign=fb1c0644823809f467e3526355d7b86353b6a7ea892226f0edf5c2bb20e4f49c)

#### Choices <a href="#choices" id="choices"></a>

Choices (sometimes called options) refer to the items which are part of a selection control such as a dropdown menu or checkboxes.

#### Data dropdown <a href="#data-dropdown" id="data-dropdown"></a>

From the perspective of people who will be filling out your form, the data dropdown works just like a regular dropdown. However, the data in the dropdown comes from a service. For instance, imagine you have a list to select a state and that you don't want to hard code the list of states in the form, either for convenience, or because the subset of selectable states is dynamic:

1. Insert a data dropdown field.
2. Click on cogwheel to bring up a _Control Settings_ dialog, similar to the one shown to the right.
3. In the _Resource URI_, enter the address of an HTTP service that returns the data you want to use to populate the dropdown. In most cases, the URL will look like `http://your-host/your-service`. If the address you specify start with a `/`, it is relative to the Advanced Forms web app, which allows you to access a service you might have implemented in Advanced Forms with XPL. For this example, let's assuming your service returns a list of states that looks like:

```
<states>
    <state abbreviation="AK" name="Alaska"/>
    <state abbreviation="AL" name="Alabama"/>
    <state abbreviation="AR" name="Arkansas"/>
    ...
</states>
```

1. In the _Items_ field, enter an XPath expression that returns one node per state. In this case, it will be: `/states/state`.
2. For each state (_item_), specify an expression relative to the node returning the label (shown to users in the dropdown) and the value (stored in the data). In this case, those expressions will be, respectively: `@name` and `@abbreviation`.

If the data in the dropdown depends on a value entered by users in another form field, you can pass that value to the service as a request parameter. For instance, let's say that in addition to the _State_ dropdown, you have a _City_ dropdown where you want to list all the cities in the currently selected state. If the service is at `/xforms-sandbox/service/zip-cities` and takes a request parameter `state-abbreviation`, assuming you named your _State_ field `state`, in the _Resource URI_ enter:

```
/xforms-sandbox/service/zip-cities?state-abbreviation={$state}
```

_Limitation: you can't yet use a variable as shown in the above example, to refer to another fields value. Instead, if the control is in the same section use `{../state}`. If in a different section with name other-section, use `{../../other-section/state}`. See_ [_#431_](https://github.com/orbeon/orbeon-forms/issues/431)_._

#### Autocomplete <a href="#autocomplete" id="autocomplete"></a>

The autocomplete control is a single item selection control that loads a list of suggestions from a service. It takes the same _Resource URI_, _Items_, _Label_, and _Value_ configuration parameters as the \[Data dropdown control]\[46]. You may want to pass the value of other controls to the service, but you'll always want to pass the currently typed value, as the suggestions should depend on what users typed so far. You access to the currently typed by value with `$fr-search-value`, as in the following example:

Copy

```
/xforms-controls/services/countries?country-name={`encode-for-uri($fr-search-value)}
```
