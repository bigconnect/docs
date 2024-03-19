# Form area

### Introduction <a href="#introduction" id="introduction"></a>

The Form Builder form area is meant to look as much as possible like the published form.

Form Builder is built around as simple layout concept: _sections_ and _grids_. This is a good alternative to:

* absolute positioning, which is rarely appropriate for web forms
* complex dynamic layouts, which often confuse form authors

Starting with Orbeon Forms 2017.2, Form Builder grids are more flexible then with earlier versions and follow a 12-column layout.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-752b250f286a121532ebb2bf37940cb2c7f1549f%252Fform-area.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=1a075d75fbb7731fbea9a673b7e8ea25087ab8d38ddbd068f42b79ebb7b09b56)

Form Builder and the main form area

### Sections <a href="#sections" id="sections"></a>

#### Introduction <a href="#introduction-1" id="introduction-1"></a>

Form Builder represents every form as a series of _sections_. A section is a logical grouping of form controls which has a _title_ and an optional _help_. For example, your form may have an "Address" section, and a "Personal Details" section.

Sections are by default presented in order, on top of each other in the page. But when using the [wizard view](https://doc.orbeon.com/form-runner/features/wizard-view), each top-level section, and optionally the next level of subsections, becomes a wizard page at runtime.

In addition:

* Every form has at least one section.
* There is no maximum number of sections within a form.
* Sections can have _subsections_, which are sections nested within other sections.

#### Operations on sections <a href="#operations-on-sections" id="operations-on-sections"></a>

You can perform the following operations on sections.

* Set the section's localizable title by clicking on it. To select a different language, use the language selector at the top right of the form area.
* Delete the section by clicking on the trashcan icon. If the section contains controls, a confirmation dialog appears.
* Set an optional help message for the section by clicking on the help icon. The help is localizable.
* Set other [section settings](https://doc.orbeon.com/form-builder/form-editor/section-settings).
* Open or collapse the section by clicking on the arrow to the left of the section title.
* Move the section up or down by clicking on the up/down arrows. These appear as needed if there is more than one section.
* Move the section left or right by clicking on the left/right arrows. These appear as needed if there is more than one section and if it is possible to move a section _into_ another section ("right"), or out of its container section ("left"). This allows making sections _subsections_ of other sections.

### Grids <a href="#grids" id="grids"></a>

Each section contains one or more _grids_. A grid is a logical grouping of form controls organized in rows and columns of cells. Unlike sections, a grid doesn't have a title or help.

A grid may be a regular grid or a [repeated grid](https://doc.orbeon.com/form-builder/form-editor/repeated-grids), which repeats a group of one or more rows. A repeated grid may have from no iteration (or repetition) to any number of repetitions. A repeated grid also has settings, but regular grids do not have settings as of Orbeon Forms 2018.2.

In addition:

* A grid may have any number of rows (but at least one).
* Each grid cell may contain a single form control, or remain empty.
* A grid cell might span multiple rows.
* Unlike a section, a grid does not have a title or properties, except for _repeated grids_ which do have properties.

When your mouse pointer hovers over a grid, the grid boundaries, cells and icons are highlighted.

#### The 12-column layout <a href="#the-12-column-layout" id="the-12-column-layout"></a>

**Introduction**

\[SINCE Orbeon Forms 2017.2]

A grid always has 12 columns when shown on a wide display. Note that on narrower displays, grids still collapse to a vertical layout (see [Responsive design](https://doc.orbeon.com/form-runner/features/responsive-design)).

See also [the blog post](https://blog.orbeon.com/2018/05/the-12-column-layout.html).

**Operations**

You can perform the following operations on grids:

* Add grid rows by clicking on the up and down arrows on the left of each row.
* Delete a row by clicking on the "minus" icon on the left of a row.
* Expand/shrink grid cells horizontally or vertically.
* Using icons to the left of the grid
  * Open the Grid Settings dialog (cogwheel icon)
    * _NOTE: Until Orbeon Forms 2018.2, this was only available for repeated grids._
  * Delete the entire grid (minus icon)
  * Moving a grid relative to its sibling grids (arrow icons)
    * _NOTE: Until Orbeon Forms 2018.2, this was only available for repeated grids._
  * Cut or copy the entire grid to the clipboard (scissors or copy icons)
    * _NOTE: Until Orbeon Forms 2018.2, this was only available for repeated grids._

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-Lq2Rnq4KH-7JwFXZzpf%252F-Lq2RyYc3QyNB6Hh-7tf%252Fgrid-icons.png%3Fgeneration=1569868477316378%26alt=media\&width=768\&dpr=4\&quality=100\&sign=a4db10f68c05ec539e6beaa5b2467d4310d136fdc25a1006d840195f549514e7)

Grid icons

Expanding/shrinking cells horizontally makes grid cells span _columns_. This allows grid cells to have a width of 1, 2, and up to 12 columns.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC4wEEAqlZHqs5Wsq%252Fruntime-grid-cell-widths.png%3Fgeneration=1528743356324559%26alt=media\&width=768\&dpr=4\&quality=100\&sign=ec5f3d856283cbde5db508dd71d5ef70b00b4ca6aeb3aea8ae6c1557383dc219)

Grid cell widths at runtime

With Orbeon Forms 2017.2, you use the arrow icons within the grid to expand into empty adjacent cells.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LKcq-YMkwXyvv0CEx2Q%252F-LKcq6n4BbtzaQ\_3ieq-%252Fgrid-icons-cell-boundaries.png%3Fgeneration=1535062344398193%26alt=media\&width=768\&dpr=4\&quality=100\&sign=d410944323d16dc094e6a986470171fa99b71ed5440ff5ae09c3381a4f2c724f)

Expanding cell boundaries

With Orbeon Forms 2018.1 and newer, you drag the cell boundary to expand into empty adjacent cells. See also the [blog post](https://blog.orbeon.com/2018/10/resizing-cells-with-drag-drop-in-form.html).

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LKcq-YMkwXyvv0CEx2Q%252F-LKcq6n7tP15sSlbQa49%252Fgrid-dragging-cell-boundaries-horizontally.png%3Fgeneration=1535062344508003%26alt=media\&width=768\&dpr=4\&quality=100\&sign=1f5e0818bad66505a18c3ecedef1226a4bda6ab6111c9fcd4eb548eb905d94ea)

Dragging cell boundaries horizontally

Expanding/shrinking cells vertically makes grid cells span _rows_. This allows making some cells taller when next to other cells, for example to allow for taller radio buttons lists.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LKcq-YMkwXyvv0CEx2Q%252F-LKcq6n9byLLvbf9D10q%252Fgrid-dragging-cell-boundaries-vertically.png%3Fgeneration=1535062345031192%26alt=media\&width=768\&dpr=4\&quality=100\&sign=cfe1cca1ab0f8b72196c128c938bc30ef6e963b9005b3328ee2eb2c6d8202be6)

Dragging cell boundaries vertically

**Undoing operations**

Destructive operations do not ask for confirmation but add the operation to the Undo stack, allowing you to undo the operation from the toolbox. See [Undo and redo](https://doc.orbeon.com/form-builder/form-editor/undo-redo).

#### Until Orbeon Forms 2017.1 <a href="#until-orbeon-forms-2017.1" id="until-orbeon-forms-2017.1"></a>

A grid has between 1 and 4 columns, but it is possible to make Form Builder support more than 4 via a configuration property.

You can perform the following operations on grids:

* Add grid columns by clicking on the left and right arrows at the top of each column.
* Add grid rows by clicking on the up and down arrows on the left of each row.
* Delete a column by clicking on the trashcan icon at the top of a column.
* Delete a row by clicking on the trashcan icon on the left of a row.
* Delete the entire grid by clicking on the trashcan icon on the top left corner of the grid.
* Expand/shrink grid cells vertically only.

For delete operations, a warning dialog shows if controls will be deleted as a result.

![Column confirmation dialog](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0Y6w2nfWSwfkSTM%252Fxbl-alert-dialog.png%3Fgeneration=1528743338462999%26alt=media\&width=300\&dpr=4\&quality=100\&sign=8358f08fe256f305ec9551c1ffde1c5ec9de0780f914ea198e9028bf891e9501)

### Grid cells and controls <a href="#grid-cells-and-controls" id="grid-cells-and-controls"></a>

Each grid cell can contain a single form control, or no control at all. Some controls, by default, take the entire width of the cell. This includes text fields and text areas. In such cases, to size the control, you can make the cell wider or narrower.

If a control is present, the following actions related to the control are possible:

* **Set control label:**
  * Click on the label.
  * A control's label appears on top of the control. It is intended to provide a descriptive label for the form control.
  * Examples: "First Name", "Street", "Phone Number".
* **Set control hint:**
  * Click on the hint.
  * A control's hint usually appears under the control. It is intended to provide a short indication to the form user of how to fill-out the form control.
  * Examples: "Your first name", "Date in mm/dd/yyyy format".
* **Set control default value:**
  * Simply enter text or select a value.
  * You can also set a calculated initial value from the ["Control Settings" dialog](https://doc.orbeon.com/form-builder/form-editor/control-settings).
* **Edit the control's items:**
  * Click on the "Edit Items" icon or test to open the [Choices editor](https://doc.orbeon.com/form-builder/form-editor/itemset-editor) (for selection controls only).

When your mouse pointer hovers over a grid cell containing a control, some icons allowing for further actions appear:

* **Move or Copy Control icon:**
  * \[SINCE Orbeon Forms 2017.2]
  * This handle lets you drag and drop the control to another grid cell. Press the _shift_ key before you start the drag & drop motion to create a copy of the current control in the target cell, instead of moving it there.
* **Delete Control icon:**
  * This deletes the control, removing all the information associated with the control including validation properties.
* **Control Settings icon:**
  * This opens the ["Control Settings" dialog](https://doc.orbeon.com/form-builder/form-editor/control-settings).
* **Expand/shrink icons:**
  * These expand and shrink the cell horizontally or vertically if possible.

### In-place editing <a href="#in-place-editing" id="in-place-editing"></a>

Form Builder allows you to edit certain text information in-place. This just means that text appears as it would in the published form, and is editable when you click on it.

To edit such text:

* Click on the text (or placeholder text).
* A text field appears.
* Enter the text.
* Use the "Enter" key or click outside the text field to commit the change.

To indicate that the text is editable, it highlights when the mouse hovers over it.

This mechanism is how you edit:

* section titles
* control labels
* control hints
* explanation texts
