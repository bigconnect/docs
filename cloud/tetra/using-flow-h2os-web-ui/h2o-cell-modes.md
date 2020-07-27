# Cell Modes

There are two modes for cells: Edit and Command.

## Using Edit Mode

In edit mode, the cell is yellow with a blinking bar to indicate where text can be entered and there is an orange flag to the left of the cell.

![](../../../.gitbook/assets/image%20%28130%29.png)

## Using Command Mode

In command mode, the flag is yellow. The flag also indicates the cell’s format:

* **MD**: Markdown

> **Note**: Markdown formatting is not applied until you run the cell by:
>
> * clicking the **Run** button ![Flow - Run Button](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/_images/Flow_RunButton.png) or
> * pressing **Ctrl+Enter**

![](../../../.gitbook/assets/image%20%2846%29.png)

* **CS**: Code \(default\)

![](../../../.gitbook/assets/image%20%28335%29.png)

* **RAW**: Raw format \(for code comments\)

![](../../../.gitbook/assets/image%20%28257%29.png)

* **H\[1-6\]**: Heading level \(where 1 is a first-level heading\)

![](../../../.gitbook/assets/image%20%28322%29.png)

**NOTE**: If there is an error in the cell, the flag is red.

![](../../../.gitbook/assets/image%20%2866%29.png)

If the cell is executing commands, the flag is teal. The flag returns to yellow when the task is complete.

![](../../../.gitbook/assets/image%20%28171%29.png)

## Changing Cell Formats

To change the cell’s format \(for example, from code to Markdown\), make sure you are in command \(not edit\) mode and that the cell you want to change is selected. The easiest way to do this is to click on the flag to the left of the cell. Enter the keyboard shortcut for the format you want to use. The flag’s text changes to display the current format.

| Cell Mode | Keyboard Shortcut |
| :--- | :--- |
| Code | `y` |
| Markdown | `m` |
| Raw text | `r` |
| Heading 1 | `1` |
| Heading 2 | `2` |
| Heading 3 | `3` |
| Heading 4 | `4` |
| Heading 5 | `5` |
| Heading 6 | `6` |

## Running Cells

The series of buttons at the top of the page below the menus run cells in a flow.

![](../../../.gitbook/assets/image%20%28627%29.png)

* To run all cells in the flow, click the **Flow** menu, then click **Run All Cells**.
* To run the current cell and all subsequent cells, click the **Flow** menu, then click **Run All Cells Below**.
* To run an individual cell in a flow, confirm the cell is in Edit Mode \(refer to [Using Edit Mode](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/flow.html#using-edit-mode)\), then:

  * press **Ctrl+Enter**

  \*\*\*\*

  > or

  * click the **Run** button ![Flow - Run Button](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/_images/Flow_RunButton.png)

## Running Flows

When you run the flow, a progress bar indicates the current status of the flow. You can cancel the currently running flow by clicking the **Stop** button in the progress bar.

![](../../../.gitbook/assets/image%20%28177%29.png)

When the flow is complete, a message displays in the upper right.

![](../../../.gitbook/assets/image%20%28261%29.png)

![](../../../.gitbook/assets/image%20%28620%29.png)

{% hint style="info" %}
If there is an error in the flow, H2O Flow stops at the cell that contains the error.
{% endhint %}



