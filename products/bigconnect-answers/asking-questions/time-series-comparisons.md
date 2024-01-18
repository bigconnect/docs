# Time series comparisons

A powerful but perhaps not obvious thing that custom expressions let us do is to create time series comparisons. For example, if we wanted to compare our revenue per month or users per day in 2019 vs. 2018, overlaid on top of each other, we could use `SumIf` and `CountIf` aggregation functions.

### Step 1: start a new question

In the upper right of the navigation bar, we’ll click on the **+ New** button, then select **Question**. We’ll select **Raw data** > **Sample Database**, and choose the `Orders` table.

![Fig. 1. For your starting data, select the Orders table.](https://www.metabase.com/learn/images/time-series-comparisons/pick-data.png)

### Step 2: add your first metric

In our example, we’re going to sum the `Subtotal` column in our `Orders` table to get our total revenue, and then break that number out by month. But the one different thing we’re going to do is create a separate metric for each year that we want to see, using the `SumIf` function. (You could just as easily use the `CountIf` function if you’re interested in counting the rows in your table over time instead of summing a column.)

To add our first metric, in the **Summarize** area of the **Notebook Editor** we’ll click the button that says `Pick the metric you want to see` and scroll down to the **Custom Expression** option.

![Fig. 2. Create your first expression and call it: Revenue in 2019.](https://www.metabase.com/learn/images/time-series-comparisons/first-expression.png)

In the box that pops up, we’ll write the following:

`SumIf([Subtotal], between([Created At], "2019-01-01", "2019-12-31"))`

What this is saying in effect is, “sum up the `Subtotal` column for all rows where the `Created At` column is between January 1, 2019, and December 31, 2019.” Let’s look at the specific parts of this expression individually:

* The first part of the `SumIf` function is where we enter the column we want to sum, wrapped in square braces: `[Subtotal]`.
* The second part of the `SumIf` function is where we write the condition to specify which rows should be summed up. What we’re doing here is using the `between` function to specify that we only want to add up the `Subtotal` for orders that were placed within 2019. The reason we’re doing this here instead of in a filter is so that each additional metric we add can specify a _different_ time range. In the `between` function, `[Created At]` is the time column in our table that we’re using to specify the time range. The first date in quotes is the start date of our range, and the second is the end date.
* In the `between` function, we have to write the dates in this format: `YYYY-MM-DD`. I.e., four characters for the year, then two for the month, then two for the day, all separated by dashes. Note that you always need to use two characters for the month and the day: you have to write “2019-01-01” not “2019-1-1”.

Then we’ll click **Done** to add our metric.

### Step 3: repeat step 2 for each additional year you want to compare

To add additional years to our comparison, we’ll click the `+` button and add an additional custom expression for each year. The easy way to do this is to copy the first expression we added in step 2, then paste that into each new expression and modify the year part of the `between` function.

Or just copy from here:

2018:

```
SumIf([Subtotal], between([Created At], "2018-01-01", "2018-12-31"))
```

2017:

```
SumIf([Subtotal], between([Created At], "2017-01-01", "2017-12-31"))
```

![Fig. 3. Using custom expressions to add more metrics to the Summarize section.](https://www.metabase.com/learn/images/time-series-comparisons/more-expressions.png)

### Step 4: break out by month-of-year

![Fig. 4. Picking a column to group by.](https://www.metabase.com/learn/images/time-series-comparisons/pick-a-grouping.png)

Next we need to click `Pick a column to group by` to choose our breakout column. In most situations we’d pick a time column and choose the `By month` granularity option. But in this case, in order to overlay the series on top of each other, we’re going to choose the `Month of year` option. (If you were to choose `Month` instead of `Month of year`, you’d see the series next to each other, one after the other.)

To do that, we’ll hover over the `Created At` date column and click `By month` to show the rest of the granularity options, then pick `Month of year`.

![Fig. 5. Group by Orders -> Created At and select Month of year to group by month.](https://www.metabase.com/learn/images/time-series-comparisons/month-of-year.png)

### Step 5: visualize the result!

To see our results, we click on the **Visualize button** at the bottom of the notebook. Answers will automatically show our results as a bar chart, with a different series for each year, broken out by each month of the year.

![Fig. 6. Three metrics grouped by month displayed as a bar chart.](https://www.metabase.com/learn/images/time-series-comparisons/visualization.png)

If you’d rather see this as a line chart, you can click the **Visualization button** in the bottom left of the screen to see your options.

### Taking this further

You’re not limited to doing year-over-year comparisons with this pattern. You can compare any metric over an arbitrary period of time to another, just by changing the dates within the `between` function, and the granularity of the date grouping. For example, we could count the rows in our `People` table during March of 2019 and compare that to users in March of 2018 by writing two expressions like this:

* `countif( between( [Created At], "2018-03-01", "2018-03-31") )`
* `countif( between( [Created At], "2019-03-01", "2019-03-31") )`

We’d then break these two metrics out by `Created At` with the granularity set to “Day of Month” in order to overlay the two series on top of each other. Here’s the notebook:

![Fig. 7. The notebook with two metrics grouped by day of month.](https://www.metabase.com/learn/images/time-series-comparisons/notebook.png)

Here’s the result, visualized as an area chart:

![Fig. 8. Two metrics displayed as an area chart, grouped by day of month.](https://www.metabase.com/learn/images/time-series-comparisons/2018-vs-2019.png)

You can adjust the grouping easily with the time series chrome at the bottom of the chart:

![Fig. 9. Using the time series chrome to change the grouping granularity from day of month to day of week.](https://www.metabase.com/learn/images/time-series-comparisons/time-series-chrome.gif)
