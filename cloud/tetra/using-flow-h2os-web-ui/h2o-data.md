# Data

## Importing Files

There are multiple ways to import data in H2O flow:

* Click the **Assist Me!** button in the row of buttons below the menus, then click the **importFiles** link. Enter the file path in the auto-completing **Search** entry field and press **Enter**. Select the file from the search results and confirm it by clicking the **Add All** link.

![](../../../.gitbook/assets/image%20%28605%29.png)

* In a blank cell, select the CS format, then enter `importFiles ["path/filename.format"]` \(where `path/filename.format` represents the complete file path to the file, including the full file name. The file path can be a local file path or a website address. **Note**: For S3 file locations, use the format `importFiles [ "s3:/path/to/bucket/file/file.tab.gz" ]`

After selecting the file to import, the file path displays in the “Search Results” section. To import a single file, click the plus sign next to the file. To import all files in the search results, click the **Add all** link. The files selected for import display in the “Selected Files” section.

![](../../../.gitbook/assets/image%20%28379%29.png)

{% hint style="info" %}
If the file is compressed, it will only be read using a single thread. For best performance, we recommend uncompressing the file before importing, as this will allow use of the faster multithreaded distributed parallel reader during import. Please note that .zip files containing multiple files are not currently supported.
{% endhint %}

* To import the selected file\(s\), click the **Import** button.
* To remove all files from the “Selected Files” list, click the **Clear All** link.
* To remove a specific file, click the **X** next to the file path.

After you click the **Import** button, the raw code for the current job displays. A summary displays the results of the file import, including the number of imported files and their Network File System \(nfs\) locations.

![](../../../.gitbook/assets/image%20%28316%29.png)

## Uploading Files

To upload a local file, click the **Data** menu and select **Upload File…**. Click the **Choose File** button, select the file, click the **Choose** button, then click the **Upload** button.

![](../../../.gitbook/assets/image%20%28259%29.png)

When the file has uploaded successfully, a message displays in the upper right and the **Setup Parse** cell displays.

Ok, now that your data is available in H2O Flow, let’s move on to the next step: parsing. Click the **Parse these files** button to continue.

## Parsing Data

After you have imported your data, parse the data.

![](../../../.gitbook/assets/image%20%28445%29.png)

The read-only **Sources** field shows the file path for the imported data selected for parsing. The **ID** contains the auto-generated name for the parsed data \(by default, the file name of the imported file uses `.hex` as the file extension\). Use the default name or enter a custom name in this field.

1. Select the parser type \(if necessary\) from the drop-down **Parser** list. For most data parsing, H2O automatically recognizes the data type, so the default settings typically do not need to be changed. The following options are available:

* AUTO
* ARFF
* XLS \(BIFF 8 only\)
* XLSX \(BIFF 8 only\)
* CSV
* SVMLight
* ORC
* AVRO
* PARQUET

2. If a separator or delimiter is used, select it from the **Separator** list.

3. Select a column header option, if applicable:

* **Auto**: Automatically detect header types.
* **First row contains column names**: Specify heading as column names.
* **First row contains data**: Specify heading as data. This option is selected by default.

4. Select any necessary additional options:

* **Enable single quotes as a field quotation character**: Treat single quote marks \(also known as apostrophes\) in the data as a character, rather than an enum. This option is not selected by default.
* **Delete on done**: Check this checkbox to delete the imported data after parsing. This option is selected by default.

A preview of the data displays in the “Edit Column Names and Types” section. To change or add a column name, edit or enter the text in the column’s entry field. In the screenshot below, the entry field for column 16 is highlighted in red.

![](../../../.gitbook/assets/image%20%28198%29.png)

To change the column type, select the drop-down list to the right of the column name entry field and select the data type. The options are:

* Unknown
* Numeric
* Enum
* Time
* UUID
* String
* Invalid

You can search for a column by entering it in the _Search by column name…_ entry field above the first column name entry field. As you type, H2O displays the columns that match the specified search terms.

After making your selections, click the **Parse** button. The code for the current job displays.

![](../../../.gitbook/assets/image%20%28429%29.png)

Since we’ve submitted a couple of jobs \(data import & parse\) to H2O now, let’s take a moment to learn more about jobs in H2O.

## Imputing Data

To impute data in a dataset, click the **Data** menu and select **Impute..**. The **Impute** option allows you to perform in-place imputation by filling missing values with aggregates computed on the “na.rm’d” vector. Additionally, you can also perform imputation based on groupings of columns from within the dataset. These columns can be passed by index or by column name using the Group By option. Note that if a factor column is supplied, then the method must be Mode.

The following options can be specified when imputing dataset:

* **Frame**: The dataset containing the column to impute
* **Column**: A specific column to impute.
* **Method**: The type of imputation to perform. Mean replaces NAs with the column mean; Median replaces NAs with the column median; Mode replaces with the most common factor \(for factor columns only\).
* **Group By**: If the **Method** is either Mean or Mode, then choose the column or columns to group by.
* **Combine Method**: If the **Method** is Median, then choose how to combine quantiles on even sample sizes. Available **Combine Method** options include Interpolate, Average, Low, and High.

![](../../../.gitbook/assets/image%20%28614%29.png)

