# Funnels

Funnel reporting can help you better analyse your goal conversion rates by reporting on individual funnel steps. This enables you to granularly optimise each step towards your goal to improve overall conversions.

#### Where to find Funnel Reports <a href="#where-to-find-funnel-reports" id="where-to-find-funnel-reports"></a>

You can access the Funnels section directly from the main navigation on the left hand side of the Matomo dashboard. Clicking **Overview** will take you to a page that lists all of your currently configured Funnels.

![Funnels Overview Menu Item](https://m-img.org/spai/w\_664+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/funnel-overview.png)

If you don’t have any funnels configured you will see the following message:

![No Active Funnels](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/no-active-funnels.png)

If this is the case, then you can [follow this guide to configure your first funnel](https://matomo.org/faq/reports/create-and-manage-funnels/).

#### Funnels Overview Page <a href="#funnels-overview-page" id="funnels-overview-page"></a>

![Funnels Overview Page](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/funnels-overview-page.png)

By default, the Funnels Overview page shows a card for each funnel you’ve defined. These cards show the funnel conversion rate and actual number of funnel conversions along with a [sparkline summary](https://matomo.org/faq/reports/graphs-and-visualisations-in-matomo/#sparkline-summary-cards). It is important to note this page will only show funnels that are currently activated.

This page is great for getting a quick overview of how your funnels are performing, especially if you have more than one. However, if you want more detailed statistics, you can click the title of a specific funnel to see a detailed funnel report page. Alternatively, you can click its name within the main navigation on the left hand side of the Matomo interface.

#### Funnel Report Page <a href="#funnel-report-page" id="funnel-report-page"></a>

![Funnel Report Page](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/funnel-report-page.png)

The funnel report page consists of four main sections which are described below.

**Funnel Row Evolution Graph**

This report shows you the [Funnel conversion rate over time](https://matomo.org/faq/funnels/faq\_22856/) by default. However, you can customise which metrics are displayed by clicking on the _**line chart icon**_ ![Line Chart Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/11/line-chart-icon.png) at the top left of the chart. This allows you to plot different funnel metrics and even compare them against each other.

![Funnel Evolution Metrics](https://m-img.org/spai/w\_496+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2016/12/funnel-evolution-metrics.png)

You can customise this chart and export the data with all of the standard [row evolution graph features available in Matomo](https://matomo.org/guide/reporting-tools/row-evolution/). However, most importantly for funnel analysis, you can confirm which metrics are plotted by referring to the colour key at the top of the chart.

![Funnel Chart Components](https://m-img.org/spai/w\_633+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/funnel-chart-components.png)

You can change the reporting range by using the [data selectors](https://matomo.org/faq/reports/data-selectors-in-matomo/) at the top of the page. To set reporting intervals, click the _**calendar icon**_ ![Calendar Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2013/09/calendar-icon.png) below the graph to make a selection. Additionally, you can change the number of periods displayed with the numeric drop down item at the bottom right.

**Funnel Sparkline Summaries**

These are the same as the simplified reports displayed on the Funnel overview page. They show you the funnel conversion rate and actual number of conversions. There is also a small orange marker which shows you the lowest point in time and a blue marker for the highest point in time. These simplified sparkline charts follow the same date range as is selected for the full row evolution chart.

![Funnel Sparkline Summary](https://m-img.org/spai/w\_574+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/funnel-sparkline-summary.png)

**Funnel Overview Card**

The Funnel Overview primarily provides a short text based overview of conversion rates for your goals and their funnel steps. The key metrics included are as follows:

* **Funnel Metrics**
  * Number of conversions vs number of visits
  * Funnel conversion rate
  * Funnel abandonment rate
* **Goal Metrics**
  * Number of goal conversions
  * Goal conversion rate
  * Goal revenue (if applicable)

There are also two more advanced features available by clicking on their text links.

**Show the visits log segmented by this funnel**

This is a useful feature for getting granular detail on exactly how each user completed (or did not complete a funnel). By reviewing individual sessions with this feature, you may be able to identify how people are getting diverted from your funnel. Additionally, if you use Matomo’s [crash analytics](https://matomo.org/guide/reports/crash-analytics/) feature, it may reveal errors at a certain funnel step on your website or application. You can [learn more about analysing Visits Logs in this guide](https://matomo.org/faq/reports/the-visits-log-report/).

**Show funnel summary**

This is useful for referencing all of the individual steps that make up a funnel. When clicked, it shows each step in table format listed by both name and targeting pattern as well as detailing whether the step is required or not. It also provides quick metrics for the number of; Entries, Visits, Exits and Proceeded Rate (i.e. what percentage make it to the next stage).

![Funnel Overview Summary](https://m-img.org/spai/w\_574+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/funnel-overview-summary.png)

**Goal Funnel Report**

The final section of this page is the Goal Funnel Report. This is a visual overview of your visitors’ progression through the funnel. Each stage shows a number of entries (people joining your funnel at this stage) and exits (people abandoning your funnel at this stage). There is also a proportionately shaded progression bar for each step with progression metrics for the step shown below.

Where there are entries or exits related to a specific page or referrer, a small table is also shown at that step. This can help you to identify the biggest funnel feeders and leaks for each step.

![Goal Funnel Report](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/goal-funnel-report.png)

You can also apply [segments](https://matomo.org/docs/segmentation/) to your Funnel reports.

