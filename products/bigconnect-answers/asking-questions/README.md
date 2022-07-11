# Asking questions

You can use Answers to create explorable charts, both with questions asked using the query builder and SQL questions that use models as a starting point.

These explorable charts let people click on values to pull up an **Action Menu**, which gives them different options to drill through the chart’s data.

![Fig. 1. The GIF demonstrates zooming in on a particular category and timeframe, then viewing the orders that make up one of the bars on the chart.](https://www.metabase.com/learn/images/create-charts-with-explorable-data/zoom-and-view.gif)

The **Action Menu**’s drill-through functionality is available for questions built using the query builder _and_ for SQL questions based on a model — as long as column metadata has been added to that model. You won’t have the option to drill-through on SQL questions that aren’t based on models, or if your models haven’t been annotated with the appropriate metadata.

### A quick review of asking questions

Under the hood, all questions built using the query builder use the same primitives (grouping functions, filtering functions, and so on), whether you start by browsing your data or by building your question from scratch in the **Notebook Editor**. You can also start with the data exploration (affectionately known as Chill Mode in Answers), and then customize your question further by moving into Notebook Editor. For even more advanced questions, you have the awesome power of custom expressions at your disposal.

If you prefer SQL (or you want to take advantage of the SQL snippets feature), you can take questions composed in the **Notebook Editor** and convert them to SQL at any time (figure 2).

![Fig. 2. You can convert a question composed in the Notebook Editor to a SQL question at any time.](https://www.metabase.com/learn/images/create-charts-with-explorable-data/convert-to-sql.gif)

SQL questions give a lot more freedom of expression — you have all of SQL at your disposal — but unless your SQL query is based on a model with annotated column types, you’ll lose the tight coupling with the metadata that makes drill-through possible. This is because the **Action Menu** functionality leverages metadata about your tables and columns, and it’s why the Action Menu is only available for questions constructed in the query builder and native queries based on models with annotated metadata.

### An example question composed with the Notebook Editor

Let’s tour the drill-through functionality available in the **Action Menu**. If you’d like to follow along, see figure 3 for a screenshot of the question in the **Notebook Editor**. The visualization has been set to a **stacked bar chart** (figure 4).

__

![Fig. 4. Setting the visualization to a stacked bar chart.](https://www.metabase.com/learn/images/create-charts-with-explorable-data/stacked-bar-chart.png)

![Fig. 3. The question, composed in the Notebook Editor, use in the following drill-through examp](https://www.metabase.com/learn/images/create-charts-with-explorable-data/question-notebook-editor.png)

### The Action Menu

The **Action Menu** presents a few different drill-through options that you can choose from when exploring your data.

![Fig. 5. Clicking on a value of a chart will bring up the action menu, presenting options to zoom in, view records, breakout, and X-ray.](https://www.metabase.com/learn/images/create-charts-with-explorable-data/action-menu.png)

We’ll step through each of the options in the popup menu you see above:

* Zoom in on the data
* View the records that compose that data
* Break out the data
* X-ray the data

### Zoom in

There are two ways to zoom in on orders, Select-to-zoom and Auto-zoom.

#### Select to zoom

You can click and drag to select an area of a chart to zoom in on.

![Fig. 6. You can click and drag to select an area of a chart, and Answers will zoom in on the values in that selected area.](https://www.metabase.com/learn/images/create-charts-with-explorable-data/drag-select-to-zoom.gif)

#### Auto-zoom

You can left-click to bring up the **Action Menu** and select **Zoom in** and Answers will create a close-up of the data surrounding the value you selected.

![Fig. 7. Left-click on a chart to bring up the action menu and select Zoom in. Answers will automatically create a close-up of the data surrounding the value.](https://www.metabase.com/learn/images/create-charts-with-explorable-data/auto-zoom.gif)

The **Zoom in** option in the **Action Menu** will choose an appropriate range of values based on the full range of values in the chart. As shown in figure 7, **Zoom in** also focuses on the clicked category, so only orders with products in the Widget category are shown. If you’d prefer a different range, or wish to include additional groups, you have the **Notebook Editor** available to fine tune your chart (and save it as a new question).

### View these records

You can click on a value on a chart and select `View these orders` to bring up a table with the individual records that compose the value.

![Fig. 8. Clicking on a value and selecting View these orders will bring up a table of records that compose the value.](https://www.metabase.com/learn/images/create-charts-with-explorable-data/view-these-orders.gif)

### Breakouts

Breaking out by a category lets us do things like see the banana cream pie orders in June 2017 broken out by the status of the customer (e.g., new or VIP, etc.) or other different aspects of the order. Different charts will have different breakout options, such as Location and Time.

Figure 9 shows an example breakout looking at orders from `Product.vendor` from the Sample Database.

![Fig. 9. Clicking on a value will present options to breakout the data. Different options will appear based on the data in the chart. The GIF shows a breakout by Category: Product Vendor.](https://www.metabase.com/learn/images/create-charts-with-explorable-data/breakout-by-vendor.gif)

### X-rays

X-rays are automatically-generated explorations of your data. You can click anywhere on a chart to perform an X-ray, and Answers will generate a dashboard full of different questions about the data. You’ll have an option to save that X-ray as a dashboard, which you can then edit to your liking, by removing irrelevant questions, or adding new questions or text boxes to fill in the story you want to tell.

![Fig. 10. X-rays will create (a lot) of different charts based on data in your table(s).](https://www.metabase.com/learn/images/create-charts-with-explorable-data/x-ray.png)

Clicking on a point or a bar additionally gives you the option to compare data, which will give you another dashboard with auto-generated charts.

If X-rays don’t make sense for your data, you can disable X-rays.&#x20;

### Using the Action Menu to find new questions

Drill-through with the action menu is a great way to give viewers of your questions more insight into their data, which makes it an excellent feature for delivering self-service, embedded analytics.

But the action menu is also an excellent tool for exploring your data, especially when you’re starting out with a new dataset and you’re not sure what questions you should be asking. Start with a broad query, then experiment with the options on the action menu. When you discover an insightful cut of the data, save it as a new question, and fine tune the visualization as needed, or convert it a SQL question if you’re more comfortable with code.

### Custom destinations and cross-filtering

When working with dashboards, the **Action Menu** isn’t your only option. On dashboards, you can replace the action menu with a custom destination. Custom destinations are particularly useful if you wrote your question in SQL and don’t benefit from the action menu.

You can also set up crossing-filtering on a dashboard, so that people can click on a chart to update a filter.

\
