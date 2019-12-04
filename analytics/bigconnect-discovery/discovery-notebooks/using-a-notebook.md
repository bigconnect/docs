# Using a notebook

In a newly created notebook, you can write a script and serve it through a REST API. A notebook can be used as follows:

### Detailed notebook page

On the workspace screen, select the notebook you want to use as an analytics tool. Then, the following screen with detailed information appears. You’ll see basic information on the notebook: data type, data source name, development language, and analytic code, etc.

![](../../../.gitbook/assets/image%20%2868%29.png)

### Notebook coding

Click **Detail** on the notebook page to pop up a new window for coding in the notebook. At the top of this window, a code to load a dataset is inserted; executing this cell loads a JSON dataset as the dataset object.

![](../../../.gitbook/assets/image%20%2864%29.png)

The screen above appears when Zeppelin is selected and includes a cell for loading the data selected when the notebook was created. After coding the program starting from the third cell, click **Save** when you are finished.

### Register a notebook API

Once you write a notebook code, you can return the results by calling a REST API. Select a **Return type** by referring to the descriptions below, and enter a **Name** and **Description**.

![](../../../.gitbook/assets/image%20%2883%29.png)

* **HTML**: The results of running the notebook script are returned in HTML.
* **JSON**: The results of running the notebook script are returned in a custom JSON format. In this case, the `response.write(...)` function provided by Metatron Discovery will be used. The following is an example code for using the response.write function:

  > * R-based notebook: `response.write(list(coefficient = 2, intercept = 0))`
  > * Python-based notebook: `response.write({'coefficient' : 2.5, 'intercept' : 0})`

* **None**: Runs the notebook script but does not provide returns.

Once you enter API information and click **Done**, the API is created to provide a REST API URL as shown below. Click **Result** to view the URL execution results in a popup window.

![](../../../.gitbook/assets/image%20%28109%29.png)

