# BigConnect Discovery Basics

This tutorial will teach you the basics of data preparation and visualisation with BigConnect Discovery. The main points addressed below are:

* Importing and Preparing a dataset
* Creating dashboards and visualisations

## Preparing data

1.Login to BigConnect Discovery by going to [http://localhost:8180](http://localhost:8180) and use **admin**/**admin** for username and password. You will be taken to the _Admin Workspace_.

![](../../.gitbook/assets/image%20%28466%29.png)

2.Open the menu and choose **MANAGEMENT / Data Preparation / Datasets.** On this page, click on the **Generate new dataset** link.

![](../../.gitbook/assets/image%20%28191%29.png)

3.Choose **My file** to upload your data

![](../../.gitbook/assets/image%20%28544%29.png)

4. Download the sample sales.csv dataset from [https://github.com/bigconnect/demos/raw/master/discovery/sales.csv](https://github.com/bigconnect/demos/raw/master/discovery/sales.csv) , upload it and choose Next. Leave the other options as default.

![](../../.gitbook/assets/image%20%28166%29.png)

5. Review the dataset and choose **Next** to continue.

![](../../.gitbook/assets/image%20%2887%29.png)

6. On the next screen, accept the defaults and click **Done**.

![](../../.gitbook/assets/image%20%28398%29.png)

7. After import, a new Data Preparation Flow will be created and you will be taken to the flow screen.

![](../../.gitbook/assets/image%20%28101%29.png)

8. Let's do some changes to the dataset:

8.1 Change the date format for OrderDate to yyyy-MM-dd. Click the three dots icon next to the **OrderDate** column and choose **Alter / Set Format**:

![](../../.gitbook/assets/image%20%28627%29.png)

8.2 Set **yyyy-MM-dd** as the format in the bottom pane and click **Add**.

![](../../.gitbook/assets/image%20%28148%29.png)

8.3 Delete the **Country**, **orderprofitable** and **SalesAboveTarget** columns. For each one of the, click on the the three dots icon next to the column name and choose **Drop**.

![](../../.gitbook/assets/image%20%28151%29.png)

8.4 Set the date format for **ShipDate** to **yyyy-MM-dd**. Follow the same instructions in step 8.1 to do so.

8.5 Combine the **latitude** and **longitude** columns in a new column of type Point. The Point datatype expects data to be in WKT format, eg. _POINT\(lat lon\)._ Without selecting a colum, choose **derive** from the **Add rule** section in the bottom pane:

```text
concat('POINT (', `longitude`, ' ', `latitude`, ')')
```

Enter **Geolocation** as the new column name and click **Add**.

![](../../.gitbook/assets/image%20%28246%29.png)

The new column is added to the dataset:

![](../../.gitbook/assets/image%20%28559%29.png)

9. Let's generate a new dataset based on the rules we set. Click on the **Snapshot** button in the top pane. 

![](../../.gitbook/assets/image%20%2810%29.png)

10. Leave all options to their defaults and click **Done**.

![](../../.gitbook/assets/image%20%28113%29.png)

11. The snapshot will be created and will be displayed in the **SNAPSHOT** section of the Flow. 

![](../../.gitbook/assets/image%20%28162%29.png)

12. Click on the created snapshot to see rule execution results:

![](../../.gitbook/assets/image%20%28269%29.png)

Everything went smooth, we don't have any errors. 

13. Now let's create a Datasource from this snapshot. Click on the **Create datasource** button and you will be taken to the Datasource Schema configuration screen.

![](../../.gitbook/assets/image%20%28169%29.png)

14. BigConnect Discovery makes some guesses as to how the columns will be mapped, but we need to review them. Let's make the following changes:

14.1 Click on the **PostalCode** column and change its **Role** to **Dimension** and **Type** to **String**.

![](../../.gitbook/assets/image%20%2892%29.png)

14.2 Click on the **latitude** column and change its **Role** to **Dimension** and **Type** to **Latitude**

14.3 Click on the **longitude** column and change its **Role** to **Dimension** and **Type** to **Longitude**

14.4 Click on the **Geolocation** column and chang its Type to **Point**.

![](../../.gitbook/assets/image%20%28557%29.png)

14.3 Make sure that **OrderDate** is selected as the **Time-Type column** and hit **Next**.

![](../../.gitbook/assets/image%20%28292%29.png)

15. On the next screen, choose **Day** for **Query Granularit**y and **Month** for **Segment Granularity**. Choose **false** for **Rollup** if enabled and click **Next**.

![](../../.gitbook/assets/image%20%28232%29.png)

16. On the next screen, enter **salesdata** as the Datasource **Name** and hit **Done**.

![](../../.gitbook/assets/image%20%28278%29.png)

17. The system will start to ingest the data in the Druid engine and the progress is reported. After finishing, you will see something like this:

![](../../.gitbook/assets/image%20%282%29.png)

## Visualizing data

Now that we have ingested the prepared data into our Druid engine, it's time to create some dashboards and visualizations.

1.Open the menu and choose **WORKSPACE / Admin Workspace** to go to our personal workspace. On this screen click on **Workbook.**

![](../../.gitbook/assets/image%20%28609%29.png)

2.Give a name to our new Workbook \(eg. **Demo**\) and click on **Done**.

![](../../.gitbook/assets/image%20%28139%29.png)

3. Now let's add our newly created datasource to the Workbook. Click on the **Add datasource** baloon, choose **salesdata** and hit **Done**.

![](../../.gitbook/assets/image%20%28107%29.png)

4. Click **Next** to move to the next screen. Enter **Sales Dashboard** as the Name for our new dashboard and click **Done** to finally open our new Workbook and Sales Dashboard.

![](../../.gitbook/assets/image%20%28199%29.png)

5. Now let's add some charts. Click on the **Chart** dropdown button and select **Chart**.

![](../../.gitbook/assets/image%20%28419%29.png)

6. Choose **Pie Chart**. Click on the **Sub\_Category** Dimension and **Sales** Measure from the left pane to add them to our chart.

![](../../.gitbook/assets/image.png)

7. Click on the color palette and choose the blue color scheme for our pie chart:

![](../../.gitbook/assets/image%20%28294%29.png)

8. Now let's add some labels to our chart. Click on the Data Label settings option, enable **Show Data Label**, select **Series Name** and **Series Percent** and enable **Use Outside Label:**

![](../../.gitbook/assets/image%20%2829%29.png)

9. Finally, click on the Common Settings button, change the **Chart Type** to **Donut** and toggle the chart **Legend**:

![](../../.gitbook/assets/image%20%28393%29.png)

10. **Save** the chart and the system will place it automatically on the Dashboard.

![](../../.gitbook/assets/image%20%28179%29.png)

11. Now let's add a new chart. Click on the plus icon next to the Chart section in the right pane. 

![](../../.gitbook/assets/image%20%28395%29.png)

12. Click on the **OrderDate** dimension, **Sales** and **Profit** measures and select **Line Chart** as the type of chart to be created.

![](../../.gitbook/assets/image%20%2848%29.png)

13. Change the aggregation type for the **Sales** and **Profit** measures to **AVG**:

![](../../.gitbook/assets/image%20%281%29.png)

14. Change the **Granularity** for **OrderDate** to **Month**:

![](../../.gitbook/assets/image%20%28103%29.png)

15. Now let's add data labels to our line chart. Click on the Data Label settings option, enable **Show Data Label**, select **Series Value** and choose Above for **Position:**

![](../../.gitbook/assets/image%20%28341%29.png)

16. Click on **Save** and drag the new chart below the first pie chart.

![](../../.gitbook/assets/image%20%28234%29.png)

17. Now let's add another one. Click on the **plus** icon next to the **Chart** section title. Select **Text Table** as the chart type.

18. Drag the **City** column to the second dimension slot and the **Sales** column to the measure slot:

![](../../.gitbook/assets/image%20%28568%29.png)

19. Change the order of **Sales** items to **Descending**:

![](../../.gitbook/assets/image%20%287%29.png)

20. Let's format our numbers now to look more friendly. Click on the Number Format button, change **Format** to **Currency** and choose **USD** as the currency. Select **Automation** for **Number Abbreviations**.

![](../../.gitbook/assets/image%20%28502%29.png)

21. Let's also color the numbers based on their highest value. Click on the Color Setting button and choose **Measure** from the dropdown. Click on the color selector and choose the **red scheme** from the **9 steps** category.

![](../../.gitbook/assets/image%20%28290%29.png)

22. Click on the Common Setting icon and set **Color Style** to background:

![](../../.gitbook/assets/image%20%28304%29.png)

24. Finally, **Save** the chart and drag it on the right side of the dashboard.

![](../../.gitbook/assets/image%20%28509%29.png)

25. Click on the **Done** button on the top ribbon to save our dashboard.

![](../../.gitbook/assets/image%20%28385%29.png)



{% hint style="info" %}
While viewing a dashboard, you can drill-down into various items by clicking on them into each widget. 

For example, to see what happened in New York City, click on the New York City row in the right table widget and the dashboard will refresh to show only information related to New York City.
{% endhint %}

26. Now let's create a new dashboard with the **salesdata** datasource and name it "**Sales Geography**". Add a new chart to the dashboard and choose **Map View** for the chart type. Add the **Geolocation** dimension and **Sales** measure to the chart:

![](../../.gitbook/assets/image%20%28552%29.png)

27. Click on the Layer Settings button and change the **Layer Type** to **Heatmap, Hexagon** or **Cluster** to see the different type of spatial aggregations.

![](../../.gitbook/assets/image%20%28201%29.png)

28.  Thank you for your patience and keep playing around to discovery other BigConnect Discovery features like the Workbench, Notebooks, Data Catalogue, Collaboration features and more.





