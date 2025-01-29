# Goals and Conversions

## Create a Goal

To set up a goal, visit the **Goals** section in Matomo and then click over to **Manage Goals**. This page will show any existing goals you have along with a large green button to **Add A New Goal**. Clicking this button will bring up the **Update Goal** view where you can add all of the required details.&#x20;

![Matomo Goal Creator](https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/matomo-goal-creator.png)

You should select a descriptive goal name that is easy to reference, for example _**New Job Application**_. It may also be useful to set up a specific naming convention if you will have many similar goals. For example, you might use a pattern such as, _**Click – Affiliate Link**_ and _**Click – Partner Site,**_ so you can easily find and reference similar goals. In the example above, you can tell at a glance that both Goals relate to link clicks on your site, as well as further detail on exactly what was clicked. You can also add a short description below the name field to help remember the more specific details or context of the goal.

### Goal Triggers <a href="#goal-triggers" id="goal-triggers"></a>

There are many actions that can trigger a Goal, however all triggers are set up in one of two ways. The first is by using automatic built-in triggers which you can select while the default **when visitors** option is active. Alternatively, you can set goals to trigger manually. The manual method requires you to add custom javascript, which is fired on your website when certain Goal conditions are met.

#### How to Trigger Goals Automatically “When Visitors” Do Something <a href="#how-to-trigger-goals-automatically-when-visitors-do-something" id="how-to-trigger-goals-automatically-when-visitors-do-something"></a>

Automatic triggers are the simplest method for setting up Goals. There are several of these methods you can use without having to add any additional code to your website. You simply select from the automatic triggers shown on screen:

* Visit a given URL (page or group of pages)
* Visit a given Page Title
* Send an event
* Download a file
* Click on a Link to an external website
* Stay for a certain amount of time

![Goal Trigger Editor](https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/goal-trigger-editor.png)

Each of the above methods will require you to define specific action that is required to fire the trigger for your goal. When using **Visit a given URL (page or group of pages)** as an example, you can select from the following options via the dropdown menu, then define your specific filters in the text box that follows:

* **Contains** – This method triggers the goal when a visitor visits a page where the URL contains what you type in the text box. This option is especially useful when you want to match a range of options with a similar theme, such as all contact pages on your website. For example, typing _**contact**_ in the text box would make the Goal fire if a user visited either of the following pages on your site:\
  &#xNAN;_**https://example.com/contact.html**_\
  &#xNAN;_**https://example.com/support/contact/**_
* **Is Exactly** – This method allows you to get really specific so the goal only fires on the exact URL that you enter, for example _**https://example.com/contact/submitted/**_ which might be a page shown once somebody has sent a message through a contact form.
* **Matches The Expression** – Matching by expression is the most complex method. However, it also allows for the most advanced targeting options. It utilises something called _regular expressions_, or regEx for short, which can be difficult to understand when you first encounter it. If you are unfamiliar with regular expressions, we highly recommend you [learn about regex](https://matomo.org/faq/how-to/faq_185/) before using it, and test your RegEx patterns extensively. The patterns you create would look a little something like this _**(.)\\/contact\\/(.)**_ which in this instance would match any page containing _**/contact/**_ in the URL, such as _**https://example.com/contact/**_

All matching types based on the text patterns described above are followed by a checkbox so you can decide if you would like your trigger to be case sensitive. With case sensitivity turned on, a trigger set to check when users **Visit a given Page Title** which exactly matches the trigger, _**Contact Us,**_ would not match a page title of _**CONTACT US**_ due to the use of all uppercase letters.

If none of the automatic matching settings are compatible with the your websites goals, you may need to manually trigger goal completions with JavaScript.

### Allow Multiple Goal Conversions (or Not) <a href="#allow-multiple-goal-conversions-or-not" id="allow-multiple-goal-conversions-or-not"></a>

The next step of the Goal configuration flow allows you to set whether you **Allow multiple conversions per visit** or not. By default, each **Goal can only be counted once per visit**. This is useful for things like newsletter subscription confirmation pages where the user can only register for your newsletter once, but they may load the confirmation page multiple times.

The reverse scenario of counting a specific conversion multiple times is useful for actions where you care about cumulative results. For example, if users can leave multiple product reviews or apply for multiple jobs, and you want to track each time this happens, then you would update this setting to _**Allow Goal to be converted more than once per visit**_.

### Tracking Goal Revenue (optional) <a href="#tracking-goal-revenue-optional" id="tracking-goal-revenue-optional"></a>

Not every goal will lead to revenue, but many will, whether directly or indirectly. The next field in the standard Goal creation flow is to set an optional **Goal Revenue** figure. This doesn’t have to be an exact value, or even require that the goal be attached to a sale, but it can help you evaluate the return on your marketing efforts.

For example, if you know that on average you sell one $1,000 service for every one hundred leads, then averaged out, each lead is equal to about $10. Therefore, you could set the Goal Revenue to $10 as the logic follows that by the time you’ve reached your hundredth lead you will likely have made at least one $1,000 sale.



## Analyse Goal reports and conversion rates

### Historical Goal Data <a href="#historical-goal-data" id="historical-goal-data"></a>

Within the first section, you will notice an **Evolution over the period** graph, which is useful for discovering trends in your Goal completions over time. If you use the Annotations feature, you will also see those entries plotted along the bottom of the graph, so you can see what might have caused any significant changes to your Goal conversion numbers.

![Historical Goal Data](https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/historical-goal-data.png)

By default, you will see the number of conversions plotted in the graph. For more options, you can hover over the **line chart icon** ![Line Chart Icon](https://m-img.org/spai/w_50+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/metrics-picker.png) at the top left of the graph. This will provide more metrics to help you drill down into the relationship between the different goals, revenue statistics, and conversion rates.

The most important metric for your goals is usually the conversion rate. It allows you to compare whether the website is converting better or not. When you only look at the absolute number of conversions instead, you don’t see the full picture.

For example, if one week, your website gets double the amount of visits, this would generally be a good thing. However, if you don’t get any more conversions, your conversion rate will have dropped, which actually represents worse performance towards your goals. Instead, you would want to see the number of conversions go up in line with the number of visits, which would maintain an almost consistent conversion rate, as shown below.

![Goal Conversion History](https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/goal-conversion-history.png)

Close tracking of the conversion rate can help you ensure you aren’t wasting opportunities. While more conversions are always good, tracking the conversion rate as well can help you discover if traffic to your website is reaching its full potential. You may also want to segment your users and view conversion rate for different segments.

### Comparing Goal Metrics Over Time <a href="#comparing-goal-metrics-over-time" id="comparing-goal-metrics-over-time"></a>

You can enable multiple metrics at the same time by clicking on the **metrics icon** ![Line Chart Icon](https://m-img.org/spai/w_50+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/metrics-picker.png) at the top left of the Evolution Graph. In the example graph below, you can see that the number of clicks reported for two partner websites started increasing at the same time, but one reached much higher levels.

![Historical Goal Comparison](https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/historical-goal-comparison.png)

Without knowing the full context in the example image, it is hard to say for sure why more people are clicking on Liveaboard links over Agoda links. However, it might suggest the content recommending Liveaboard is more engaging. Or, it could simply be that there are less opportunities to click Agoda links on the website. As always with analytics; the underlying data will help reveal underlying trends, but you will likely need to correlate it back to your knowledge of your website and business activities to draw meaning from the results.

You may also have noticed in the graph above there are four green icons in the bottom left of the graph, which provide further options for working with the data set. In order, these are:

* **Change period (**_**Calendar Icon**_ ![Calendar Icon](https://m-img.org/spai/w_50+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/calendar-icon.png)**)** – Select whether you would like data plotted by days, weeks, months, or years.
* **Export this data in other formats (**_**Export Icon**_ ![Export Icon](https://m-img.org/spai/w_50+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/export-icon.png)**)** – Available formats are: CSV, XML, PHP, RSS, TSV, JSON, and HTML
* **Export as Image (**_**Image Icon**_ ![Image Icon](https://m-img.org/spai/w_50+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/image-icon.png)**)** – This can be useful for sharing reports with clients or managers.
* **View notes for this date range (**_**Annotation Icon**_ ![Annotation Icon](https://m-img.org/spai/w_50+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/annotation-icon.png)**)** – This brings up relevant annotations in the selected reporting period.

### Goal Summaries <a href="#goal-summaries" id="goal-summaries"></a>

Below the Evolution graph you should see a series of sparkline summary cards containing a line chart, alongside these are conversion numbers and rates for your goals. The first card provides a combined summary of your goal conversion data within the currently selected date range. These statistics are useful to get a general feel of the marketing performance of your website. Generally speaking the higher these numbers are, the better your website is doing.

![Goal Sparkline Summaries](https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/goal-sparkline-summaries.png)

Underneath the top sparkline summary card, you will find a further summary card for each of the goals on your site so you can review each individually. You can see the number of conversions and conversion rates for each goal on this page.

If you would like to review any of the goals in more detail, simply click on the goal title and it will take you to a dedicated page for that goal with more detailed graphs and data.

![Job Application Goal](https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/09/job-application-goal.png)

### Goals Breakdown Reports <a href="#goals-breakdown-reports" id="goals-breakdown-reports"></a>

At the bottom of the Goals Overview page, you will see a list of goals reports alongside a table. Each of these reports summarises Goal data by various reporting criteria. These reports help you understand the conditions that lead to goal completions on your website.

### Goals by Referrers <a href="#goals-by-referrers" id="goals-by-referrers"></a>

The Goals by Referrers report can help you identify the traffic sources that lead to goal completions. When your website receives traffic from a variety of sources, some traffic will inevitably be more qualified than the rest. The reports within this section break down the sources of visitors which are most beneficial to your website goals.

#### Channel Type Report <a href="#channel-type-report" id="channel-type-report"></a>

The channel report is useful for analysing the types of traffic, e.g. search or website referrals, that are most likely to complete a goal on your website. As an example, you might expect that traffic from Social Networks would complete a goal based on clicking social media links the most. This report can help confirm or deny your suspicions so you can adjust your marketing plans appropriately.

#### Search Engines Report <a href="#search-engines-report" id="search-engines-report"></a>

This report shows you which search engines send traffic that complete goals on your website. As an example, you might notice visitors from DuckDuckGo, a privacy-focused search engine, rarely sign up to your newsletter. However, these same visitors often complete the goal of clicking on your blog’s RSS links. This could suggest your blog is a better medium for keeping in touch with these users instead of an email newsletter.

#### Keywords Report <a href="#keywords-report" id="keywords-report"></a>

The keywords report attempts to identify as many keywords as possible that visitors have used to reach your site and complete your goals. As an example, you might notice visitors searching for keywords including “report” are more likely to opt-in to your newsletter. This would suggest that offering reports as an incentive to opt-in is a beneficial strategy for your website.

#### Websites Report <a href="#websites-report" id="websites-report"></a>

This report can help you identify which third party websites bring in qualified traffic. For example, imagine you are advertising job listings on multiple sites. However, only visitors from one site complete your _**New Job Application**_ goal. In this case, you may want to investigate expanding your relationship with that site over the others. Continuing this example, you might also discover another job site leads to goal completions, but you aren’t directly advertising on it. In this case, you might want to see if they offer advanced job promotion features.

### Goal Engagement <a href="#goal-engagement" id="goal-engagement"></a>

This section is useful for tracking how long it takes for people to complete the goals on your website. As an example, if you were running a lead generation site, the **Visits to Conversion** report could show people tend to sign up on their third visit. Based on this, you might experiment with adding a popup subscription form to your site that shows on peoples’ first or second visits. You can then track if this results in more goal completions with less visits.

You might also notice in the **Days to Conversion** report, the majority of visitors complete your goals several days after their first visit. This could suggest people are coming in through multiple channels. Knowing this could help inform your multi-channel conversion attribution strategy.

#### Goals by User location <a href="#goals-by-user-location" id="goals-by-user-location"></a>

The next section provides data based on where and how visitors access your website.

**Country, Continent, Region and City reports**

These reports break down goal completions based on a visitors location. This can be useful, for example, if your site features marketing consent popups for visitors in certain areas, such as in Europe to address [GDPR](https://gdpr.eu/faq/). In this case, the **Continent** report can help you understand the impact that may be having on your goal conversions.

Alternatively, the **Country** report might show that lots of people complete your goals in English speaking countries, but not in others, even though you have a high number of visitors. This could suggest that it would be useful to offer translations for non-English speaking markets.

**Devices: Type, Model & Brand Reports**

Analysing goal completions by device can help you identify potential technical issues with your site. For example, if you were to notice that nobody was completing your newsletter subscription form from a mobile device, you might want to run further tests to confirm that your optin form is fully visible and functioning on mobile devices.

#### Goals by User attribute <a href="#goals-by-user-attribute" id="goals-by-user-attribute"></a>

Visits by server time can help you identify the most popular times that people complete your goals. For an internationally-focused organisation, this could help inform you on the best time to run time-bound campaigns or publish new conten
