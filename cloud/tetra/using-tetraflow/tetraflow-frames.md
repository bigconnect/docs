# Frames

An Tetra frame represents a 2D array of data. The data may be local or it may be distributed in an Tetra cluster.

## Creating Frames

To create a frame with a large amount of random data \(for example, to use for testing\), click the drop-down **Admin** menu, then select **Create Synthetic Frame**. Customize the frame as needed, then click the **Create** button to create the frame.

![](../../../.gitbook/assets/image%20%28180%29.png)

## Viewing Frames

To view a specific frame, click the “Key” link for the specified frame, or enter `getFrameSummary "FrameName"` in a cell in CS mode \(where `FrameName` is the name of a frame, such as `allyears2k.hex`\).

![](../../../.gitbook/assets/image%20%28128%29.png)

From the `getFrameSummary` cell, you can:

* View a truncated list of the rows in the data frame by clicking the **View Data** button
* Split the dataset by clicking the **Split…** button
* View the columns, data, and factors in more detail or plot a graph by clicking the **Inspect** button
* Create a model by clicking the **Build Model** button
* Make a prediction based on the data by clicking the **Predict** button
* Download the data as a .csv file by clicking the **Download** button
* View the characteristics or domain of a specific column by clicking the **Summary** link

When you view a frame, you can “drill-down” to the necessary level of detail \(such as a specific column or row\) using the **Inspect** button or by clicking the links. The following screenshot displays the results of clicking the **Inspect** button for a frame.

![](../../../.gitbook/assets/image%20%28582%29.png)

This screenshot displays the results of clicking the **columns** link.

![](../../../.gitbook/assets/image%20%28371%29.png)

To view all frames, click the **Assist Me!** button, then click the **getFrames** link, or enter `getFrames` in the cell in CS mode and press **Ctrl+Enter**. You can also view all current frames by clicking the drop-down **Data** menu and selecting **List All Frames**.

A list of the current frames in Tetra displays that includes the following information for each frame:

* Link to the frame \(the “key”\)
* Number of rows and columns
* Size

For parsed data, the following information displays:

* Link to the .hex file
* The **Build Model**, **Predict**, and **Inspect** buttons

![](../../../.gitbook/assets/image%20%28217%29.png)

To make a prediction, check the checkboxes for the frames you want to use to make the prediction, then click the **Predict on Selected Frames** button.

## Splitting Frames

Datasets can be split within Flow for use in model training and testing.

![](../../../.gitbook/assets/image%20%28639%29.png)

1. To split a frame, click the **Assist Me** button, then click **splitFrame**.

2. From the drop-down **Frame:** list, select the frame to split.

3. In the second **Ratio** entry field, specify the fractional value to determine the split. The first **Ratio** field is automatically calculated based on the values entered in the second **Ratio** field.

4. In the **Key** entry field, specify a name for the new frame.

5. \(Optional\) To add another split, click the **Add a split** link. To remove a split, click the `X` to the right of the **Key** entry field.

6. Click the **Create** button.

## Plotting Frames

To create a plot from a frame, click the **Inspect** button, then click the **Plot** button for columns or factors. Note that from this section, you can also inspect the **Chunk compression summary** and the **Frame distribution summary**.

![](../../../.gitbook/assets/image%20%28137%29.png)

1. Select the type of plot from the **Type** menu

* plot: Creates a graph with a series of plot points.
* path: Creates a line graph connecting plot points.
* rect: Creates a bar graph. Note that with rect graphs, you cannot specify values of the same type. You will receive an error if you attempt to specify, for example, two String columns or two Number columns.

2. Specify the information that you want to view on the X axis and on the Y axis. Select from the following options below. These options correspond to the parsed data file.

* label: Plots the column headings
* type: Plots real vs. enum values
* Missing: Plots missing values
* Zeros: Plots `0` values
* +Inf: Plots positiive `inf` values
* -Inf: Plots negative `inf` values
* min: Plots the min value
* max: Plots the max value
* mean: Plots the mean value
* sigma: Plots the sigma value
* cardinality: Plots the cardinality. Used with enum values.
* Actions: Plots actions \(for example, “convert to numeric”.\)

3. Select one of the above options from the drop-down **Color** menu to display the specified data in color.

4. click the **Plot** button to plot the data.

![](../../../.gitbook/assets/image%20%28325%29.png)

