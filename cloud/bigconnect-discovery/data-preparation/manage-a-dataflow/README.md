# Manage a Dataflow

A **dataflow** is the unit of processing a **dataset**. A single dataflow can be associated with multiple datasets to perform transformations. That is, a dataset must belong to a dataflow for transformation rules to be applied. It forms a relationship such as a “join” or “union” with other datasets.

As shown below, the dataflow details page shows the dependency among all datasets in a dataflow, and the transformation rules applied to each dataset.

![](../../../../.gitbook/assets/add\_datasets\_1.jpg)

The following subsections cover the processes involved in defining a dataflow, such as **adding a dataset**, **editing transformation rules**, and **creating a data snapshot with transformation results**.

The Dataflow menu can be accessed under **MANAGEMENT** > **Data Preparation** > **Dataflow** on the left-hand panel of the main screen.

![](../../../../.gitbook/assets/create\_a\_dataset\_1.jpg)

## Adding a Dataset

The first step in defining a dataflow is to add a dataset. This can be conducted using the two methods described below.

## Adding a Dataset in an empty Dataflow

1 Click Add a dataflow on the upper right of the Dataflow page.

2\. Enter the **Name** and **Description** for the dataflow, and click Done to create an empty dataflow.

![](../../../../.gitbook/assets/add\_datasets\_2.jpg)

3\. Click the **Add dataset to this dataflow** button on the center of the page.

![](../../../../.gitbook/assets/add\_datasets\_3.jpg)

4\. Select the datasets to be added.

![](<../../../../.gitbook/assets/image (8).png>)

5\. When an imported dataset and its corresponding wrangled dataset are created, click the **Edit rules** button to edit rules (see **Edit rules** for a detailed procedure).

![](../../../../.gitbook/assets/add\_datasets\_5.jpg)

## Creating a Dataflow in the dataset details page

In the dataset details page, click the **Create dataflow with this dataset** button to create a dataflow, and proceed until the step before **Edit rules**.

![](../../../../.gitbook/assets/add\_datasets\_6.jpg)

{% hint style="info" %}
The dataflow is named based on the name of the dataset.
{% endhint %}

