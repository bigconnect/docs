# Behaviour

Matomo will measure many kind of actions by your visitors such as pageviews, clicks on files (Downloads), clicks on links (Outlinks), viewing and clicking on specific content blocks with content tracking. You can even implement custom events and track page performance

Additional insightful and visual tools are provided including: page overlay, transitions and users flow report. And you can understand your audience behavior such as click, move, and scroll behaviour with heatmaps as well as individual session recordings.

## Introduction to Page Analytics Reports

Page analytics help you to understand how people navigate through your site and what content is most popular at a given time so you can optimise under-performing areas and replicate winning content formulas. The main area for reviewing page analytics within Matomo is the **Behaviour** section of the main navigation.

![Matomo Page Analytics Report](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/matomo-pages-report.png)

### The Page Analytics Reports <a href="#the-page-analytics-reports" id="the-page-analytics-reports"></a>

Within the **Behaviour** section you will find links to the following page reports:

* **Pages** – General overview of the popularity of pages on your site.
* **Entry Pages** – Provides an overview of how many people enter your site from a specific page and how many of those instantly leave.
* **Exit Pages** – Provides an overview of which pages people most commonly exit your site from.
* **Page Titles** – This provides an overview of the popular sections of your site based on the page titles. It can be a little less specific than the Pages report.

Each of these page reports are presented as hierarchical tables by default. They all share a similar configuration with some common features and metrics which are discussed in the following section.

### Identify Your Most Popular Pages with the Pages Report <a href="#identify-your-most-popular-pages-with-the-pages-report" id="identify-your-most-popular-pages-with-the-pages-report"></a>

If you want to know which pages are most popular on your site, you should review the **Pages** report. To access this report, make sure you are logged in to your dashboard and click **Behaviour** in the main navigation on the left hand side. Next, click on the **Pages** link below it to load a paginated table which lists all of the pages on your site. The **Pages** report shows you the following metrics by default:

* **Pageviews** – How many unique times the page has been loaded.
* **Unique Pageviews** – How many unique visitors have loaded the page.
* **Bounce Rate** – The percentage of people that immediately leave after viewing a page.
* **Avg. Time on Page** – The amount of time users spend on the page on average.\
  Note: Some pages will naturally have shorter average times like gateway pages used to navigate your site. While others should have a longer time on page, such as long articles. It helps to understand the context of a page to know if this is important.
* **Exit Rate** – The percentage of visitors who leave the site after viewing a specific page.
* **Avg. Page Load Time** – The amount of time spent waiting for a page to load; from the moment the user requests a page until it has fully completed processing.

By default, the tables are listed in order of **Unique Pageviews** numbers. However, you can sort the order of pages in this table by any of the above metrics. Sorting by differing metrics can help you discover the top (and worst) performing pages for each. To sort by an alternative metric, click on the title of the column you want to sort by and the data will reload sorted by this metric.

![Pages Report](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/pages-report.png)

You can also click the currently selected metric (represented by the **sort icon** ![Down Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/down-icon.png)) to toggle and reverse the sort, so that you can review either the lowest or highest performing pages first. The icon flips vertically to indicate whether the metric is being sorted from **highest to lowest** ![Down Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/down-icon.png) or **lowest to highest** ![Up Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/up-icon.png)

### Switching between hierarchical and flat reports <a href="#switching-between-hierarchical-and-flat-reports" id="switching-between-hierarchical-and-flat-reports"></a>

Page reports are shown hierarchically by default. Hierarchical reports make it easier to navigate your analytics for large sites by rolling up the metrics for sub-pages into their top-level page.

![Hierarchical Report](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/hierarchical-report.png)

However, if you want the metrics for individual pages within a section you can click on the **plus icon** ![Plus Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2013/09/plus-icon.png) to expand the pages under that level. When you are finished reviewing an expanded row, you can click on the **minus icon** ![Minus Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2013/09/minus-icon.png) to contract the table.

![Hierarchical Page Report](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/hierarchical-page-report.png)

The screenshot above shows the expanded view of the my-account page. All of the pages indented below `my-account` are sub-pages. For example, the `orders` page would actually be found at `/my-account/orders/`. Where you see a page called `/index`, that means it is the default page at the top level of a directory. So in this case, accessing /my-account/ or /my-account/index would take you to the same page. It is also worth noting that the top level metrics are equal to the sum of the sub-pages totals.

And last but not least, you can hover over the table and click the **cog icon** ![Cog Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2013/09/settings-icon.png) and select the **Make it flat** menu item to completely flatten the table results. When you flatten the report, the full URL of every page gets its own row, so you can analyse individual pages instead of grouped sections. .

![Flat Page Report](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/flat-report.png)

### Analyse site sections by searching within page reports <a href="#analyse-site-sections-by-searching-within-page-reports" id="analyse-site-sections-by-searching-within-page-reports"></a>

You can also search for specific pages or sections of pages within your pages reports. To do this, load the table as usual. Hover your mouse over the table, then click on the **green search icon** ![Search Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2013/09/search-icon.png) that appears at the bottom left of the table. From here you can type in a search term such as _**my-account**_ to find any pages that contain the search string within the URL. Continuing the aforementioned example, the following pages could all appear:

![Page Search](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/page-search.png)

One other tip that can be useful is to use Segments when reviewing certain pages or sections of your site. For example, if you are reviewing engagement with account related pageviews then it may make sense to use a segment that only includes customers so you get data based on the people that can access the full versions of a page.

## Explore analytics for specific pages in more detail

While browsing the tables within Matomo’s Page reports, there are several features available to dig deeper into specific pages. To access these, you can hover your mouse over the row that you are interested in and several icons will appear. Each of these are discussed in more detail below.

![Page Row Options](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/page-row-options.png)

Additionally, if you’d like to edit the number of table rows shown per page at any point, you can hover your mouse over the table to reveal a number in the bottom right.

![Table Row Settings](https://m-img.org/spai/w\_408+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/row-settings.png)

### View Page Shown in a Report <a href="#view-page-shown-in-a-report" id="view-page-shown-in-a-report"></a>

At times you may want to view the page that is being referenced in a report, for example, to clarify what it contains. To do this you can click on the **link icon** ![New Window Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/new-window-icon.png) which will open the page in a new browser tab. This is especially useful on larger websites where you may not be familiar with every page. It can help you to identify what the page is and what your visitors are seeing.

### Transitions <a href="#transitions" id="transitions"></a>

Shows more detailed statistics about a page such as where visitors come from and where they go. To learn more about this feature view our detailed [Transitions guide](https://matomo.org/docs/transitions/).

### Page Overlay <a href="#page-overlay" id="page-overlay"></a>

The page overlay view provides in-page analytics so that you can understand what links people are clicking on within a specific page. It displays little bubbles next to each of your links containing a percentage of how many visitors have clicked that link. You can edit the date range that the statistics are drawn from by using the Matomo sidebar shown alongside your page. To learn more about this feature view our detailed [page overlay guide](https://matomo.org/docs/page-overlay/).

![Page Overlays](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/page-overlays.png)

### Segmented Visits Log for Pages <a href="#segmented-visits-log-for-pages" id="segmented-visits-log-for-pages"></a>

Within any of the page reports, you can hover over a specific row to show the **segmented visits log icon** ![Visit Log Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/visit-log-icon.png). Clicking on this will load a screen that shows you all visits where somebody visited the page or section that you are querying. This can help to provide some context on what kind of people are visiting the page and what they are doing before and after.&#x20;

### Page Performance Report <a href="#page-performance-report" id="page-performance-report"></a>

While viewing one of the page reports, if you hover your mouse over one of the rows, you will see a series of green icons on that row. Clicking the **Page Performance icon** <img src="https://m-img.org/spai/w_50+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2023/01/performance-icon.png" alt="Performance Icon" data-size="line"> will take you to the page performance report for the selected row so you can track it over time.&#x20;

### How to See Page Popularity Over Time With the Row Evolution Feature <a href="#how-to-see-page-popularity-over-time-with-the-row-evolution-feature" id="how-to-see-page-popularity-over-time-with-the-row-evolution-feature"></a>

While it is useful to know how many visits a page received within a certain period, it can also be useful to know how that number has evolved over time. This is especially true for pages that you have taken time to optimise their performance. To this, hover your mouse over the page within the table and click on the\*\* line chart icon\*\* ![Line Chart Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2014/04/line-chart-icon.png). Clicking this reveals the row evolution view which will show you the pageviews over time.

![Page Row Evolution](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/page-row-evolution.png)

Below this chart, there is a **Compare records** section. This section enables you to select a second or third row etc, to compare the metrics of these pages over time.

![Page Row Comparison](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2023/01/page-row-comparison.png)

## Page Performance

As long as you have installed the [standard Matomo tracker](https://developer.matomo.org/api-reference/tracking-javascript), Matomo will automatically try to track performance data for each page view when using the Matomo JavaScript tracker. The primary way to analyse your website speed is in the **Performance** report (available in Matomo version 4 and above) which can be found under the **Behaviour** section of the main navigation.

This section shows you how long it takes for all of the pages on your site to load and provides detailed analysis of which stages of the loading process contribute most to your site’s speed. As you can see in the example below, it also tracks how these metrics change over time, so you can avoid ever increasing load times and identify potential issues as they arise.

![Page Performance Overview](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2020/10/pageperformance\_overview.png)

The top section of the **Performance** page displays a bar chart providing an overview of how your site is performing on the whole over the selected time period. You can change which dates are focused on through the standard [Date Selector](https://matomo.org/docs/matomo-tour/#date-selector) at the top of the page.

![Performance Date Selector](https://m-img.org/spai/w\_450+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2020/10/performance-date-selector-450x126.png)

Below the performance chart you will find sparkline summary cards of your key performance metrics. Again, these metrics reflect the each metric averaged out across all of your pages within the currently selected time frame.

![Performance Date Selector](https://m-img.org/spai/w\_450+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2020/10/page-performance-overview-450x168.png)

### Page Performance Tables <a href="#page-performance-tables" id="page-performance-tables"></a>

Below the performance charts and sparkline summaries, you will find detailed performance metrics broken down for each individual page.

![](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2020/11/performance\_pagesreport.jpg)

There are two different tables so you can review your page performance metrics broken down either by **Page URLs** or **Page Titles** depending on your site configuration and preference. One critical thing to remember is that while the specific performance metrics available within each of these tables are the same, the results may vary depending on your website configuration. For example the **Page Titles** report can include multiple pages with duplicate titles whereas the **Page URL** report should always account for a single page.

You’ll also find related report links at the bottom of each table if you would like to focus on either Entry or Exit pages. These related reports can be useful as slow load times are a common cause of high Bounce Rates and Exit Rates.

![Page Performance Navigation](https://m-img.org/spai/w\_450+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2020/10/page-performance-navigation-450x113.png)

### ACCESS THE PAGE PERFORMANCE REPORT FROM ANY PAGE TABLE <a href="#access-the-page-performance-report-from-any-page-table" id="access-the-page-performance-report-from-any-page-table"></a>

While viewing one of the page reports, if you hover your mouse over one of the rows, you will see a series of green icons on that row. Clicking the **Page Performance icon** <img src="https://m-img.org/spai/w_50+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2020/10/dash-icon.png" alt="Performance Icon" data-size="line"> will take you to the page performance report for the selected page so you can track it over time.

![Access Page Performance](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2020/10/access-page-performance.png)

Along the bottom of the chart you will notice a **calendar icon** ![Calendar Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2013/09/calendar-icon.png) which allows you to choose the interval for the bar chart. You can view the page performance data grouped by days, weeks, months or years.

In the example screenshot above, you can see where an issue affecting the DOM processing time appeared on a specific day. The report was handed to a developer who recognised an error that was adding to the page load time and it was fixed by the next day.

Choosing an appropriate interval can help you to powerpoint where changes have occurred over time. In some cases, it may not be as obvious as the example above but if you have been making use of the [Matomo annotations](https://matomo.org/docs/annotations/) feature represented (**annotation icon** ![Annotation Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2013/09/annotation-icon.png)), you might find that you’ve noted down specific changes on your site that coincide with a page speeding up or slowing down.

One last note is that if a specific period stands out, you can hover your mouse over the bar to reveal a tooltip containing the specific date or dates covered by this period and exactly how long each part of the loading process contributed to the total.

![Specific Page Performance Tooltip](https://m-img.org/spai/w\_633+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2020/10/specific-page-performance-tooltip.png)

### Analyse the performance of specific pageviews <a href="#analyse-the-performance-of-specific-pageviews" id="analyse-the-performance-of-specific-pageviews"></a>

Whilst viewing the [Visits Log](https://matomo.org/docs/real-time/#visits-log), you can actually look at how long it took for any user to load a specific page. To see the **page load time** of each individual pageview, hover your mouse over the page URL for the visit you are interested in and the **page load time** is displayed within the tooltip that pops up.

![Page Performance Visitor Log](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2020/10/page-performance-visitor-log.png)
