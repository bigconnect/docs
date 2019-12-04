# Using Flows

You can use and modify flows in a variety of ways:

* Clips allow you to save single cells
* Outlines display summaries of your workflow
* Flows can be saved, duplicated, loaded, or downloaded

## Using Clips

Clips enable you to save cells containing your workflow for later reuse. To save a cell as a clip, click the paperclip icon to the right of the cell \(highlighted in the red box in the following screenshot\). 

![](../../../../.gitbook/assets/image%20%28178%29.png)

To use a clip in a workflow, click the “Clips” tab in the sidebar on the right.

![](../../../../.gitbook/assets/image%20%2880%29.png)

All saved clips, including the default system clips \(such as `assist`, `importFiles`, and `predict`\), are listed. Clips you have created are listed under the “My Clips” heading. To select a clip to insert, click the circular button to the left of the clip name. To delete a clip, click the trashcan icon to right of the clip name.

**NOTE**: The default clips listed under “System” cannot be deleted.

Deleted clips are stored in the trash. To permanently delete all clips in the trash, click the **Empty Trash** button.

**NOTE**: Saved data, including flows and clips, are persistent as long as the same IP address is used for the cluster. If a new IP is used, previously saved flows and clips are not available.

## Viewing Outlines

The **Outline** tab in the sidebar displays a brief summary of the cells currently used in your flow; essentially, a command history.

* To jump to a specific cell, click the cell description.
* To delete a cell, select it and press the X key on your keyboard.

![](../../../../.gitbook/assets/image%20%28344%29.png)

## Saving Flows

You can save your flow for later reuse. After a Flow is saved, you can load it by clicking on the **Flows** tab in the right sidebar. Then in the pop-up confirmation window that appears, select **Load Notebook**. Refer to **Loading Flows** for more information.

To save your flow as a notebook, click the “Save” button \(the first button in the row of buttons below the flow name\), or click the drop-down “Flow” menu and select “Save Flow.” To enter a custom name for the flow, click the default flow name \(“Untitled Flow”\) and type the desired flow name. A pencil icon indicates where to enter the desired name.

![](../../../../.gitbook/assets/image%20%2855%29.png)

To confirm the name, click the checkmark to the right of the name field.

![](../../../../.gitbook/assets/image%20%28118%29.png)

To reuse a saved flow, click the “Flows” tab in the sidebar, then click the flow name. To delete a saved flow, click the trashcan icon to the right of the flow name.

![](../../../../.gitbook/assets/image%20%2864%29.png)

## Finding Saved Flows on Your Disk

By default, flows are saved to the `h2oflows` directory underneath your home directory. The directory where flows are saved is printed to stdout:

```text
03-20 14:54:20.945 172.16.2.39:54323     95667  main      INFO: Flow dir: '/Users/[YOUR_USER_NAME]/h2oflows'
```

To back up saved flows, copy this directory to your preferred backup location.

To specify a different location for saved flows, use the command-line argument `-flow_dir` when launching H2O:

`java -jar h2o.jar -flow_dir /[ENTER_PATH_TO_FLOW_DIRECTORY_HERE]`

If the directory that you enter in place of `[ENTER_PATH_TO_FLOW_DIRECTORY_HERE]` does not exist, it will be created the first time you save a flow.

## Saving Flows on a Hadoop Cluster

If you are running H2O Flow on a Hadoop cluster, H2O will try to find the HDFS home directory to use as the default directory for flows. If the HDFS home directory is not found, flows cannot be saved unless a directory is specified while launching using `-flow_dir`:

`hadoop jar h2odriver.jar -nodes 1 -mapperXmx 6g -output hdfsOutputDirName -flow_dir hdfs://[HOST]:[PORT_NUMBER]/[PATH_TO_DIRECTORY_HERE]`

The location specified in `-flow_dir` may be either an hdfs or regular filesystem directory. If the directory does not exist, it will be created the first time you save a flow.

## Loading Flows

To load a saved flow, click the **Flows** tab in the sidebar at the right. In the pop-up confirmation window that appears, select **Load Notebook**, or click **Cancel** to return to the current flow.

![](../../../../.gitbook/assets/image%20%2836%29.png)

After clicking **Load Notebook**, the saved flow is loaded.

To load an exported flow, click the **Flow** menu and select **Open Flow…**. In the pop-up window that appears, click the **Choose File** button and select the exported flow, then click the **Open** button.

![](../../../../.gitbook/assets/image%20%28298%29.png)

**Notes**:

> * Only exported flows using the default .flow filetype are supported. Other filetypes will not open.
> * If the current notebook has the same name as the selected file, a pop-up confirmation appears to confirm that the current notebook should be overwritten.

