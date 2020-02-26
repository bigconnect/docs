# Dataflow Rules

The key task in data preparation is to create rules for data transformation \(usually refinement\). The transformation rules and input/output specifications are combined to be applied to actual data or other similar data, or scheduling is performed for such tasks.

Below are instructions on creating rules, checking the results, and modifying or deleting rules.

The Edit Rules page consists of the following:

![](../../../../.gitbook/assets/image%20%28247%29.png)

1. Column type, name, and menu button
2. Menu for simple rule creation
3. Rule list and insert button \(appears when cursor is placed in between rules\)
4. Enabled when undo and redo are available
5. Panel to enter rule details
6. Column value distribution, distinct count, type mismatch, null value, etc.

## Create a rule

### Using the column header menu

1. Select a target column by clicking the column header.

* Press the function key to select multiple columns.
* Depending on your OS, click while holding the ^ or ⌘ key to select/deselect a column \(toggle\).
* Click while holding the Shift key to select a range.

![](../../../../.gitbook/assets/image%20%2843%29.png)

2. Click the ![header\_menu](https://metatron-app.github.io/metatron-doc-discovery/en/_images/header_menu.png) icon in the header of a selected column to open the header menu, and select a transformation command. Among the commands, **drop** and **settype** are performed upon clicking.

![](../../../../.gitbook/assets/image%20%2879%29.png)

3. To add details, fill out the command input panel below, and click the **Add** button.

4. Some commands can be performed by selecting a distribution bar.

* Click a distribution bar to filter the data based on the selected range \(toggle\).
* Click the type mismatch or null value graph to set conditions for those values.

![](../../../../.gitbook/assets/image%20%28436%29.png)

### Using the command input panel

1. Select a transformation rule \(command\) in the command input panel.

![](../../../../.gitbook/assets/image%20%28109%29.png)

2. Add details as needed, and click the **Add** button. Target columns can be selected using the input panel. You can also designate a column by clicking the column header.

![](../../../../.gitbook/assets/image%20%28453%29.png)

### Inserting into a rule list

1. In the list of rules of the right, place the cursor over the boundary where you wish to insert a new rule. The **+ Insert rule** button appears. Press this button.

![](../../../../.gitbook/assets/image%20%28157%29.png)

2. Select a transformation rule \(command\) in the command input panel. Add details as needed, and click the **Add** button.

* When a rule is inserted in this manner, all subsequent rules are affected.
* Rules that cannot be normally executed are displayed in red. In this case, they will revert to the results obtained in the previous step.

![](../../../../.gitbook/assets/image%20%28539%29.png)

## Edit a created rule

### Editing a rule

1. In the list of rules on the right, place the cursor over the rule to be edited. The ![edit](https://metatron-app.github.io/metatron-doc-discovery/en/_images/icon_edit1.png) button appears. Press this button.

![](../../../../.gitbook/assets/image%20%28542%29.png)

2. Edit the rule in the command input panel and press the **Done** button.

![](../../../../.gitbook/assets/image%20%28457%29.png)

### Deleting a rule

In the list of rules on the right, place the cursor over the rule to be deleted. The delete button appears. Press this button. 

When a rule is deleted in this manner, all subsequent rules are affected.

![](../../../../.gitbook/assets/image%20%2827%29.png)

### Undo and redo

On the upper right of the rule list are icons to perform **undo** and **redo**.

![](../../../../.gitbook/assets/image%20%28299%29.png)

To revert to a state before executing a command, press the ![undo](https://metatron-app.github.io/metatron-doc-discovery/en/_images/icon_undo.png) button.

* The dataset reverts to the state before the last transformation \(including rule creation, modification, and deletion\).
* All rules that were affected also revert to their previous states.

To perform the same command again, press the ![redo](https://metatron-app.github.io/metatron-doc-discovery/en/_images/icon_redo.png) button.

* Pressing ![redo](https://metatron-app.github.io/metatron-doc-discovery/en/_images/icon_redo.png) is faster than following the steps to perform the same command again. It is because the transformation results are stored in memory.

