# Content Tracking

Content Tracking is a feature within Matomo that is designed to show how effective specific pieces of content are at generating interactions on any page of your website or app. You might be wondering how often a specific ad or a banner was displayed and viewed by your visitors on any of your pages and how often a visitor actually interacted with them. We call these two parts content impression and content interaction. Comparing these two metrics allows us to generate an interaction rate.

This feature is not only limited to ads or banners and it can help you discover the most effective placements and variations for any of your content. Once a page has loaded we log a content impression for each content block that is visible to the user. A content block consists of a content name (My Product 1), a content piece ("product1.jpg" or "Buy Product 1 now") and a content target (http://landingpage.example.com). As soon as a visitor clicks on a content block a content interaction is logged allowing you to analyze the ratio of content impressions to content interactions. As soon as a visitor scrolls down the page and sees more ads/banners/blocks, we automatically log the impression for each block.

## Analyse Content Tracking reports

Not only does the **Content Tracking** feature allow you to track different types of content, it also offers multiple tracking methods so the specific metrics will vary depending on how you have configured it. For this reason, it is important that anyone working with your analytics understands how it has been set up. There can be a significant difference between the number of times a banner is loaded on a page, and how many times it has actually been seen, so you should note which method of tracking you use. With that in mind, you can start looking at your reports.

### Where to find Content Tracking Reports <a href="#where-to-find-content-tracking-reports" id="where-to-find-content-tracking-reports"></a>

Content Tracking reports can be found on the **Contents** page within the **Behaviour** section. This is accessed via the main navigation shown down the left-hand side of Matomo analytics.

![Behaviour Menu Item](https://m-img.org/spai/w\_197+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/behaviour-menu.png)

There are two **Content Tracking** reports available within Matomo:

*   **Content Name Report** – This shows you the top level content blocks so you can review aggregate reports of how many impressions and how many interactions each type of content (banners, listings, thumbnails, etc.) is getting. Each Content Name can be clicked to view the list of Content pieces with this name.\


    <figure><img src="https://m-img.org/spai/w_769+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2014/09/Content-Name.png" alt=""><figcaption></figcaption></figure>
*   **Content Piece Report** – This report provides granular details on the specific pieces of content that get the most impressions and also the most interaction. Each content piece can be clicked to view the list of Content Names for this piece.\


    <figure><img src="https://m-img.org/spai/w_807+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2014/09/Content-Piece.png" alt=""><figcaption></figcaption></figure>

**Note:** You won’t actually be able to see the content target within these reports, which is why it is important to set descriptive **Content Name** and **Content Piece** labels.

### Flat vs Hierarchical Reports <a href="#flat-vs-hierarchical-reports" id="flat-vs-hierarchical-reports"></a>

By default, your reports will be rolled up and hierarchical so you can review at the **Content Name** level and then expand to see the pieces that have contributed to the score. Alternatively, you can flatten the entire table to rank each individual **Content Piece**.&#x20;

**Hierarchical Report**\


<figure><img src="https://m-img.org/spai/w_450+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2022/03/hierarchical-report-450x256.png" alt=""><figcaption></figcaption></figure>

**Flattened Report**

![Content Tracking - Flattened Report](https://m-img.org/spai/w\_450+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/flattened-report-450x283.png)

### Easily Analyse Related Content Pieces <a href="#easily-analyse-related-content-pieces" id="easily-analyse-related-content-pieces"></a>

If you’d like to review similarly themed content within your reports it can be helpful to give them similar names with consistent wording that can be searched for. This means you can filter the table to only see a specific name via search. To do this, hover your mouse over the table and then click on the _**search icon**_ ![Search Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2013/09/search-icon.png) in the bottom left. This reveals a search bar where you can enter what you are searching for, e.g. _**banner**_ if you want to see all banners.

### Identifying your best and worst performing content with content tracking <a href="#identifying-your-best-and-worst-performing-content-with-content-tracking" id="identifying-your-best-and-worst-performing-content-with-content-tracking"></a>

If you are tracking a lot of content pieces on your website and you want to know which have the most or least impressions and interactions, then one way to do this is by utilising the sort feature on the content tracking tables. To do this, visit either of the content reports and then click on the title of the metric that you’d like to sort by. You click on the title again to toggle between sorting from the **highest to lowest** ![Descending](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/descending.png) or the **lowest to highest** ![Ascending Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/03/ascending.png).

## Content Tracking metrics

While tracking a piece of content, the following metrics are generated for your Content Tracking Reports.

* **Impressions** – The number of times that a content block, such as a banner or advert, was on a page that somebody visited. If you have configured Matomo to only track impressions for visible content blocks, it will tell you how many times it has been shown on the screen. Otherwise, it will tell you many times a page was loaded containing the block of content.
* **Content Interaction** – An interaction typically occurs when a user clicks on a banner or ad, but it can be any interaction that a visitor makes within a specified content block. Other examples of interactions could be a video play or form submission.
*   **Interaction Rate** – This is the ratio of content **Impressions** to **Interactions**. For example, if an ad was displayed 100 times and there were 2 interactions, this would result in an interaction rate of 2%. The formula to calculate interaction rates is:

    ```
    (Interactions / Impressions) * 100 = % Interaction Rate 
    ```

The screenshot below is from a **Content Name report** showing how often each piece of content was viewed and interacted with.

![Content Tracking Metrics](https://m-img.org/spai/w\_450+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2022/12/content-tracking-metrics-450x348.png)
