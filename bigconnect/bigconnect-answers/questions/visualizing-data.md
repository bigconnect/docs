# Visualizing data

While tables are useful for looking up information or finding specific numbers, it’s usually easier to see trends and make sense of data using charts.

To change how the answer to your question is displayed, click on the **Visualization** button in the bottom-left of the screen to open the visualization sidebar.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/VisualizeChoices.png" alt=""><figcaption></figcaption></figure>

If a particular visualization doesn’t really make sense for your answer, that option will appear in the “Other charts” section. You can still select one of these other charts, though you might need to fiddle with the chart options to make the chart work with your data.

## Visualization options

<figure><img src="https://www.metabase.com/docs/latest/questions/images/viz-options.png" alt=""><figcaption></figcaption></figure>

Each visualization type has its own advanced options.

To change the settings for a specific chart, for example a row chart, you could either:

* Click on the gear icon in the bottom left of the chart (next to the **Visualization** button, or
* Click on **Visualization** in the bottom left of the chart, then hover over the currently selected chart and click on the **gear** icon that pops up.

## Visualization types

Answers ships with a bunch of different visualizations types:

### Numbers

The Numbers option is for displaying a single number, nice and big.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/number.png" alt=""><figcaption></figcaption></figure>

### Trends

The Trend visualization is great for displaying how a single number has changed between two time periods.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/trend-settings.png" alt=""><figcaption></figcaption></figure>

### Detail

The Detail visualization shows a single result record (row) in an easy-to-read, two-column display.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Progress bars

Progress bars are for comparing a single number to a goal value that you set.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/progress.png" alt=""><figcaption></figcaption></figure>

### Gauges

Gauges allow you to show a single number in the context of a set of colored ranges that you can specify.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/gauge.png" alt=""><figcaption></figcaption></figure>

### Tables

The Table option is good for looking at tabular data (duh), or for lists of things like users or orders.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/conditional-formatting.png" alt=""><figcaption></figcaption></figure>

### Pivot tables

Pivot tables allow you swap rows and columns, group data, and include subtotals in your table. You can group one or more metrics by one or more dimensions.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/pivot-table-options.png" alt=""><figcaption></figcaption></figure>

### Line charts

Line charts are best for displaying the trend of a number over time, especially when you have lots of x-axis values.&#x20;

<figure><img src="https://www.metabase.com/docs/latest/questions/images/trend-lines.png" alt=""><figcaption></figcaption></figure>

### Bar charts

Bar charts are great for displaying a number grouped by a category (e.g., the number of users you have by country).

<figure><img src="https://www.metabase.com/docs/latest/questions/images/bar.png" alt=""><figcaption></figcaption></figure>

### Area charts

Area charts are useful when comparing the proportions of two metrics over time. Both bar and area charts can be stacked.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/area.png" alt=""><figcaption></figcaption></figure>

### Combo charts

Combo charts let you combine bars and lines (or areas) on the same chart.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/combo-chart.png" alt=""><figcaption></figcaption></figure>

### Histograms

If you have a bar chart like Count of Users by Age, where the x-axis is a number, you’ll get a special kind of bar chart called a histogram where each bar represents a range of values (called a “bin”).

<figure><img src="https://www.metabase.com/docs/latest/questions/images/histogram.png" alt=""><figcaption></figcaption></figure>

### Row charts

Row charts are good for visualizing data grouped by a column that has a lot of possible values, like a Vendor or Product Title field.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/row.png" alt=""><figcaption></figcaption></figure>

### Waterfall charts

Waterfall charts are a kind of bar chart useful for visualizing results that contain both positive and negative values.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/waterfall-chart.png" alt=""><figcaption></figcaption></figure>

### Scatterplots and bubble charts

Scatterplots are useful for visualizing the correlation between two variables, like comparing the age of your people using your app vs. how many dollars they’ve spent on your products.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/scatter.png" alt=""><figcaption></figcaption></figure>

### Pie chart or donut charts

A pie chart or donut chart can be used when breaking out a metric by a single dimension, especially when the number of possible breakouts is small, like users by gender.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/donut.png" alt=""><figcaption></figcaption></figure>

### Funnel charts

Funnels are commonly used in e-commerce or sales to visualize how many customers are present within each step of a checkout flow or sales cycle. At their most general, funnels show you values broken out by steps, and the percent decrease between each successive step.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/funnel.png" alt=""><figcaption></figcaption></figure>

### Maps

When you select the Map visualization, Answers will automatically try and pick the best kind of map to use based on the table or result set.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/map.png" alt=""><figcaption></figcaption></figure>

## Formatting data in charts

While we’re talking about formatting, we thought you should also know that you can access formatting options for the columns used in a chart. Just open the visualization settings and select the `Data` tab:

<figure><img src="https://www.metabase.com/docs/latest/questions/images/chart-formatting.png" alt=""><figcaption></figcaption></figure>

Then click on the gear icon next to the column that you want to format. Dates, numbers, and currencies tend to have the most useful formatting options.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/chart-formatting-options.png" alt=""><figcaption></figcaption></figure>
