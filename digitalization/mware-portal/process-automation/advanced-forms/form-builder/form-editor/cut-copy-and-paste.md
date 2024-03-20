# Cut, copy and paste

### Cut, copy and paste from the toolbox <a href="#cut-copy-and-paste-from-the-toolbox" id="cut-copy-and-paste-from-the-toolbox"></a>

The top of the toolbox has the usual cut, copy, and paste icons:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBzFc-dec6GwBva7\_%252Fxcv.png%3Fgeneration=1528743349628429%26alt=media\&width=768\&dpr=4\&quality=100\&sign=1f14dc249494e1003ea05ee35e8162d78e7b665c525726a4188eb6850adb10c5)

Cut, copy and paste icons

They allow performing the usual cut/copy/paste operations on form controls.

* **Cut:**
  * Copies the control in the currently selected grid cell to the Form Builder clipboard.
  * Then removes the control from the current grid cell.
  * If the cell is empty, nothing happens.
* **Copy:**
  * Copies the control in the currently selected grid cell to the Form Builder clipboard.
  * The copied control is left as is in its cell.
  * If the cell is empty, nothing happens.
* **Paste:**
  * Pastes the contents of the Form Builder clipboard.
  * If the clipboard contains a control, it is inserted in the next available grid cell.
  * If the clipboard contains a grid or section (see below), it is inserted in the next available position.
  * If the clipboard is empty, nothing happens.

The following control information is copied and pasted:

* type and appearance
* name
* label, hint, and help
* default value
* itemset
* validations, including datatype, constraints, and alerts
* formulas
* any associated localized resources

When a control is pasted, if the control name of the clipboard control is currently not in use in the form, it is used. Otherwise, a new automatically-generated name is chosen by Form Builder.

### Keyboard shortcuts <a href="#keyboard-shortcuts" id="keyboard-shortcuts"></a>

You can use the following keyboard shortcuts:

macOS:

* `⌘-X`: cut the current control
* `⌘-C`: copy the current control
* `⌘-V`: paste from the toolbox

All operating systems:

* `⌃-X`: cut the current control
* `⌃-C`: copy the current control
* `⌃-V`: paste from the toolbox

### Copying and pasting between forms <a href="#copying-and-pasting-between-forms" id="copying-and-pasting-between-forms"></a>

1. You can copy an item, go back to the Form Builder Summary page, reopen the same form or open a different form, and paste the item just copied.
2. You can open several forms with Form Builder in different tabs or windows of the same browser (and the same browser user), and copy/paste items between these forms.

_NOTE: It is for now not possible to copy and paste items between different browsers, different users, or different user sessions (for example after logging out and logging back in)._

### Cut, copy and paste of grids and sections <a href="#cut-copy-and-paste-of-grids-and-sections" id="cut-copy-and-paste-of-grids-and-sections"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC7J9AvQ\_1aTR7ZEL%252Fcut-copy-grids-sections.png%3Fgeneration=1528743365775022%26alt=media\&width=768\&dpr=4\&quality=100\&sign=e6b9431877f9e836452d9fec640069d2495337609760a5885c07c0efff69c560)

"Cut" and "Copy" icons for grids and sections

_NOTE: The "Cut" icon is not available if the section or grid is the last one in its container._

* **Cut:**
  * Copies the given grid or section to the Form Builder clipboard.
  * Then removes the grid or section from the form.
* **Copy:**
  * Copies the grid or section to the Form Builder clipboard.
  * The copied grid or section is left as is in the form.
  * If the cell is empty, nothing happens.

You paste a cut or copied grid or section using the toolbox's "Paste" icon (see above).

If pasting the grid or section can cause name conflicts, a dialog shows:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC7JENCEiLVjcrrF9%252Fsection-template-merge-dialog.png%3Fgeneration=1528743365736319%26alt=media\&width=768\&dpr=4\&quality=100\&sign=ddf16f73b7a3f1bdabe86fd7497e32263fa70b3392dc3cc30cb71608b8fd7962)

Control Names dialog

The dialog shows a list of all control names within the grid or section and how they will be changed after pasting. Since control names are unique within a form definition, two controls cannot have the same name. Names that are available show in green. Names that conflict show in yellow and an automatic name is generated.

You can optionally set a prefix and/or a suffix for all names. This can help prevent automatic generation of names. For example, you could paste a US Address twice:

* first, with all control names prefixed by `shipping-`
* second, with all control names prefixed by `billing-`

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC7JH1q3eWq5l0ZFd%252Fsection-template-merge-dialog-prefix.png%3Fgeneration=1528743365717919%26alt=media\&width=768\&dpr=4\&quality=100\&sign=d2a3f1e3a9b39af35a623e1e6ac83886be5608c36713e128d59f431aabfa3622)

Control Names dialog with prefix

### Undoing and redoing cut and paste operations <a href="#undoing-and-redoing-cut-and-paste-operations" id="undoing-and-redoing-cut-and-paste-operations"></a>

The "Cut" and "Paste" operations are undoable in the following way:

*   Undoing a "cut" of a control, grid or section puts the item back to its original location. However, the clipboard retains

    the item.
*   Undoing a "paste" of a control, grid or section removes the newly-inserted item from the form. The clipboard remains

    untouched.
