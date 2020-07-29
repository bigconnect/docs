# Using Flows

You can use and modify flows in a variety of ways:

* Clips allow you to save single cells
* Outlines display summaries of your workflow
* Flows can be saved, duplicated, loaded, or downloaded

## Using Clips

Clips enable you to save cells containing your workflow for later reuse. To save a cell as a clip, click the paperclip icon to the right of the cell \(highlighted in the red box in the following screenshot\). 

![](../../../.gitbook/assets/image%20%28339%29.png)

To use a clip in a workflow, click the “Clips” tab in the sidebar on the right.

![](../../../.gitbook/assets/image%20%28147%29.png)

All saved clips, including the default system clips \(such as `assist`, `importFiles`, and `predict`\), are listed. Clips you have created are listed under the “My Clips” heading. To select a clip to insert, click the circular button to the left of the clip name. To delete a clip, click the trashcan icon to right of the clip name.

**NOTE**: The default clips listed under “System” cannot be deleted.

Deleted clips are stored in the trash. To permanently delete all clips in the trash, click the **Empty Trash** button.

**NOTE**: Saved data, including flows and clips, are persistent as long as the same IP address is used for the cluster. If a new IP is used, previously saved flows and clips are not available.

## Viewing Outlines

The **Outline** tab in the sidebar displays a brief summary of the cells currently used in your flow; essentially, a command history.

* To jump to a specific cell, click the cell description.
* To delete a cell, select it and press the X key on your keyboard.

![](../../../.gitbook/assets/image%20%28633%29.png)

## Saving Flows

You can save your flow for later reuse. After a Flow is saved, you can load it by clicking on the **Flows** tab in the right sidebar. Then in the pop-up confirmation window that appears, select **Load Notebook**. Refer to **Loading Flows** for more information.

To save your flow as a notebook, click the “Save” button \(the first button in the row of buttons below the flow name\), or click the drop-down “Flow” menu and select “Save Flow.” To enter a custom name for the flow, click the default flow name \(“Untitled Flow”\) and type the desired flow name. A pencil icon indicates where to enter the desired name.

![](../../../.gitbook/assets/image%20%2898%29.png)

To confirm the name, click the checkmark to the right of the name field.

To reuse a saved flow, click the “Flows” tab in the sidebar, then click the flow name. To delete a saved flow, click the trashcan icon to the right of the flow name.

![](../../../.gitbook/assets/image%20%28121%29.png)

## Loading Flows

To load a saved flow, click the **Flows** tab in the sidebar at the right. In the pop-up confirmation window that appears, select **Load Notebook**, or click **Cancel** to return to the current flow.

![](../../../.gitbook/assets/image%20%2856%29.png)

After clicking **Load Notebook**, the saved flow is loaded.

To load an exported flow, click the **Flow** menu and select **Open Flow…**. In the pop-up window that appears, click the **Choose File** button and select the exported flow, then click the **Open** button.

![](../../../.gitbook/assets/image%20%28561%29.png)

**Notes**:

> * Only exported flows using the default .flow filetype are supported. Other filetypes will not open.
> * If the current notebook has the same name as the selected file, a pop-up confirmation appears to confirm that the current notebook should be overwritten.

