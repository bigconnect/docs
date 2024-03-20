# Undo and redo

### Undo and redo icons <a href="#undo-and-redo-icons" id="undo-and-redo-icons"></a>

The toolbox shows familiar undo and redo icons.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBzFc-dec6GwBva7\_%252Fxcv.png%3Fgeneration=1528743349628429%26alt=media\&width=768\&dpr=4\&quality=100\&sign=1f14dc249494e1003ea05ee35e8162d78e7b665c525726a4188eb6850adb10c5)

Undo and redo icons

### Using undo and redo <a href="#using-undo-and-redo" id="using-undo-and-redo"></a>

When hovering over the icons, a tooltip tells you which operation is undone or redone.

You can undo most operations which change important aspects of the form:

### Keyboard shortcuts <a href="#keyboard-shortcuts" id="keyboard-shortcuts"></a>

You can use the following keyboard shortcuts:

macOS:

* `⌘-Z`: undo the last operation
* `⌘-⇧-Z`: redo the last operation

All operating systems:

* `⌃-Z`: undo the last operation
* `⌃-Y`: redo the last operation

### Supported undo/redo actions <a href="#supported-undo-redo-actions" id="supported-undo-redo-actions"></a>

* insertion and deletion
  * control, grid, grid row, section and section template
* moving
  * control (via drag and drop)
  * section (up/down/right/left arrows)
* settings
  * rename control, grid or section
  * other control, grid or section settings, including itemsets, labels, hint, and validations
* section template merging

### Limitations <a href="#limitations" id="limitations"></a>

In this initial version, it is not yet possible to undo changes to the form definition's source code with Edit Source, or changes to actions, services, and other global form settings. We hope to support undoing these operations in the future.
