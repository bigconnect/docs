# Page Transitions

The Transitions Plugin gives you a report that shows the things your visitors did directly before and after viewing a certain page. This page will explain how to access, understand, and use the powerful Transitions report.

### How to Open the Transitions Report for a Page? <a href="#how-to-open-the-transitions-report-for-a-page" id="how-to-open-the-transitions-report-for-a-page"></a>

You can launch it from the pages and page titles reports (_Behaviour > Pages_ and _Behaviour > Page Titles_). When you move your mouse over a row of the table, two icons are shown. Click the ![](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2012/10/index1.png) icon to open Transitions for this row:

![](https://m-img.org/spai/w\_890+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2012/10/Transitions-Open-.png)

The report that is shown contains data for the period you selected in the normal date picker.

### Understanding the Page Transitions Report <a href="#understanding-the-page-transitions-report" id="understanding-the-page-transitions-report"></a>

<figure><img src="https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2012/10/Page-Transitions-Analytics-Report.png" alt=""><figcaption></figcaption></figure>

**In the center**, you see general statistics about the page you launched Transitions on.

* The first number is the number of pageviews for this specific page. Hover the number to see the share of all pageviews.
* You can also hover the other numbers to see the share of the groups.

On the sides of the box in the center, you see what visitors did directly before and after visiting the page in the center.

_The height of the connections to the box in the center is proportional to the amount of traffic that took this route._

**On the left side**, you can see where users came from.

* _From Internal Pages_ means that visitors came to the page in the center from other pages on your website. This might be via links in the content or the navigation.
* The group _From Search Engines_ shows how many people came from external search engines like Google directly to the page in the center. You can click “From Seach Engines” to expand the group and see the keywords that were used.
* _From Websites_ shows which external websites linked directly to the page in the center. You can also expand this group by clicking on it in order to see the links that referred visitors to the page.
* There’s also a group _From Campaigns_ which is not visible in the screenshot because there are no such referrals in the example. This group shows how many visitors triggered a campaign when entering on the page in the center.
* _From Internal Searches_ lists the keywords that your visitors searched for on your Internal Search engine, and then clicked to the page.
* The group _Direct Entries_ shows visits that started on this page, i.e. the visitor used the page as a landing page.

**The right side** shows what users did after visiting the page in the center.

* _To Internal Pages_ means that visitors went to a different page on your website after their visit to the page in the center.
* _Internal Searches_ lists all the keywords that visitors started searching for, when they were on the page in the center.
* When people click a download or outlink on the page, this is shown in the groups _Downloads_ and _Outlinks._
* The group _Exits_ shows visits that ended on this page, i.e. no more actions were tracked for the visitor.

The last element is the _Page Reload_ group on the bottom. A page reload means loading the page multiple times in a row.

### Using & Interacting with the Transitions Report <a href="#using-interacting-with-the-transitions-report" id="using-interacting-with-the-transitions-report"></a>

There are several ways you can interact with Transitions.

* You can _open and close groups_ by clicking either their title or the corresponding line in the center box.
* Transitions shows you _additional information_ about various parts in a tooltip when you move your mouse around.
* You can click the pages under From Internal Pages and To Internal Pages to _reload Transitions for a different page_.
* The current state of the popover is part of the URL. This means that you can use your browser’s back and forward buttons to navigate. Also, you can send the link to someone else and the Transitions popover will open for the page you were viewing when you copied the URL.

### Conclusion <a href="#conclusion" id="conclusion"></a>

When designing Transitions, we made some deliberate choices that distinguish it from similar Clickpath features. It is not focused on the path, instead it shows the transitions through a page – hence the name. With this feature, you can see _everything_ that happened before and after a page, not only the internal pageviews.

**This information is quite hard to find in other tools so we hope you enjoy easy analytics with Matomo and have fun using Transitions!**

### Resources <a href="#resources" id="resources"></a>

Users Flow is a visual representation of the most popular paths your users take through your website or app, which complements Transition perfectly. Here is what the Users Flow report looks like:

<figure><img src="https://m-img.org/spai/w_633+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2016/12/1_Visualization.png" alt=""><figcaption></figcaption></figure>
