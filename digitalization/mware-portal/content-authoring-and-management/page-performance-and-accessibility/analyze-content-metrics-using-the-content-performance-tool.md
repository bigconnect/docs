# Analyze Content Metrics Using the Content Performance Tool

You can use the Content Performance tool to analyze content metrics, like the number of views, traffic acquisition channels, or keywords driving people to your Page. In the Content Performance tool, you can view metrics for the URL of the Content Page, Widget Page, or Web Content associated with a [Display Page Template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages).

IMPORTANT

Content Performance metrics are only available for the [Default Experience](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/creating-and-managing-experiences).

Access the Content Performance tool using the _View Metrics_ (![View Metrics](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-analytics.png)) option for your content. You can find this option in the [Applications toolbar](https://learn.liferay.com/web/guest/w/dxp/getting-started/navigating-dxp#applications-bar) and the [Content Dashboard](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/about-the-content-dashboard). For more information, see [About the Content Performance Tool](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/about-the-content-performance-tool).

The Content Performance tool contains three different areas:

* [Content Information](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool#reviewing-content-information)
* [Engagement](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool#analyzing-content-engagement)
* [Traffic Channels](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool#analyzing-traffic-channels)

### [Reviewing Content Information](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool#reviewing-content-information) <a href="#reviewing-content-information" id="reviewing-content-information"></a>

The top area in the Content Performance tool provides general information about your content.

| Label              | Description                                                                                                                                                                                                  |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Content Name       | Content or Widget Page’s name or name of the asset based on a Display Page Template.                                                                                                                         |
| URL                | Represent the URL of the Content Page, Widget Page, or Web Content associated with a Display Page Template.                                                                                                  |
| Author             | Author’s name. This information is only available for Web Content using a Display Page Template.                                                                                                             |
| Time Span Selector | Select a time span (Last 7 Days / Last 30 Days) and the charts shows the data for the time span selected.                                                                                                    |
| Publication date   | Shows the publication date. Note that when you link the asset to a Display Page Template newer than the asset publication date, the publication date corresponds to the Display Page Template creation date. |

The [Engagement](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool#analyzing-content-engagement) and [Traffic Channels](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool#analyzing-traffic-channels) areas in the Content Performance tool show statistics for the language selected in the language icon selector. When your content is localized in different languages, you can view metrics for each language.

NOTE

Even when your content is not translated into a specific language, you may find statistics for that language if users access the content using that language’s URL.

![The top area in the Content Performance tool provides general information about your content.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool/images/01.png)

#### [Using the Time Span Selector](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool#using-the-time-span-selector) <a href="#using-the-time-span-selector" id="using-the-time-span-selector"></a>

You can scope your metrics analysis by using the time span dropdown (Last 7 Days / Last 30 Days).

![Using the Time Span Selector](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool/images/02.png)

The time span selector is also included in the detail view for the _Referral_ and _Social traffic_ panels.

![Time Span on the Detail View of the Panel](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool/images/03.png)

### [Analyzing Content Engagement](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool#analyzing-content-engagement) <a href="#analyzing-content-engagement" id="analyzing-content-engagement"></a>

This area shows the number of views for the content or Page URL and how this number changes over time. The number of views is key to understanding content performance and discovering content insights. For Web Content or Documents and Media based on a Display Page Template, the Engagement area also shows the number of reads. A visitor accessing the Web Content may or may not read it. To distinguish the audience that only visits the content (number of Views) from the audience engaging with the content (number of Reads), Liferay uses a specialized algorithm that considers parameters like article length, language, and scroll behavior, among others.

By default, the Engagement area shows the total number of views for the content, along with the number of views in the last seven days. You can change this period in the drop-down time selector and analyze trends over time using the backward and forward buttons (![Arrow left](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-angle-left.png) ![Arrow right](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-angle-right.png)). Hover over any part of the line chart to show views for a specific time.

NOTE

If the Engagement chart shows a zero value for a certain period, it means that the information could not be collected or analyzed for that period.

![The Engagement area in the Content Performance tool provides Views information.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool/images/04.png)

### [Analyzing Traffic Channels](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool#analyzing-traffic-channels) <a href="#analyzing-traffic-channels" id="analyzing-traffic-channels"></a>

This part of the Content Performance sidebar provides information about the sources driving web traffic to your content. The sources can be:

| Traffic Channel | Source Description                                           |
| --------------- | ------------------------------------------------------------ |
| Social          | Social networks                                              |
| Organic         | Search engines                                               |
| Paid            | Paid keywords from Google Ads                                |
| Referral        | Sources other than the search engines and same Page’s domain |
| Direct          | Other or unknown sources                                     |

![The Search Engines Traffic area in the Content Performance tool provides information about your traffic channels.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool/images/05.png)

When you click one of the traffic channels (A), you can see extended information for the channel (B).

![Click on the traffic channels to access additional channel details.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/analyze-content-metrics-using-content-performance-tool/images/06.png)

The following table describes the metrics you can find in the Traffic Channels area:

| Label                      | Description                                                          |
| -------------------------- | -------------------------------------------------------------------- |
| Traffic Volume             | The estimated number of visitors to your Page.                       |
| Traffic Share              | Percentage of traffic your content receives from the traffic source. |
| Top Referring Social Media | Top ten social media networks driving traffic to your Page.          |
| Top Referring Pages        | Top ten referrer Pages driving traffic to your Page.                 |
| Top Referring Domains      | Top ten referrer domains driving traffic to your Page.               |

NOTE

Keywords are one or more words that people use to find content.

\
