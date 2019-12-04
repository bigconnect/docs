# Discovery Data Preparation

**Data Preparation** is a tool that creates transformation rules to transform files and tables for more convenient analysis of datasets, and saves the results into HDFS or Hive.

## **Advantages of data preparation**

![](../../../.gitbook/assets/image%20%28201%29.png)

Users can create transformation rules by following the step-by-step process as shown in the above GUI. Since the transformation results from each step are stored in memory together with the data distribution, users can easily check the results through the simple click of a button and perform **undo** and **redo** just like using a text editor.

Based on these characteristics, the data preparation tool offers the following advantages:

* Users unfamiliar with programming or data processing can obtain the desired results.
* Adding a transformation rule usually involves programming or writing an SQL query. However, Metatron Discovery’s Data Preparation provides a GUI for **exploratory transformation** that enables the creation of transformation rules simply by clicking a button or typing.
* Basic data transformation is conducted automatically. For instance, a type cast is automatically applied to columns comprised of numerals. This is made possible by the **undo** and rule deletion functions.
* Data of different forms can be combined as desired \(e.g. reference file + fact table\).
* The results of data refinement can be shared with others, thus reducing the burden of exchanging physical data.
* Storage space is saved and **information life cycle \(ILM\)** shortened by deleting the actual data and retaining only the transformation rules involved. The actual data can be easily created whenever needed.

## Structure of data preparation

![](../../../.gitbook/assets/image%20%289%29.png)

As shown in the above figure, data preparation is comprised of a **dataset** built from the target data, a **dataflow** that defines transformation rules for the designated dataset, and a **data snapshot** that shows the transformation results.



