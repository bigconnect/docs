# Understanding Content Recommendations

When users visit your website, they interact with your content and show different behaviors depending on their interests. Liferay DXP, in combination with [Liferay Analytics Cloud](https://learn.liferay.com/analytics-cloud/latest/en/index.html), can analyze this browsing behavior and show relevant content for your users. This can be useful to promote certain products or content that your users are more likely to buy or consume. For example, when a user visits your online store and shows interest in a particular type of product, you can use content recommendations to show the user similar products or related product promotions.

IMPORTANT

You must [connect Analytics Cloud to your Liferay DXP instance](https://learn.liferay.com/analytics-cloud/latest/en/connecting-data-sources/connecting-liferay-dxp-to-analytics-cloud.html) to provide content recommendations to your users.

### [How Content Recommendations Work](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/understanding-content-recommendations#how-content-recommendations-work) <a href="#how-content-recommendations-work" id="how-content-recommendations-work"></a>

Configuring content recommendations is a two-step process. First, you track users’ behavior and define topics of interest. This step requires [Analytics Cloud connected to your DXP instance](https://learn.liferay.com/analytics-cloud/latest/en/connecting-data-sources/connecting-liferay-dxp-to-analytics-cloud.html). Based on previous user’s behavior in the site, Analytics Cloud defines and ranks topics of interest, and associates these interests to the user identifier in the site. Second, you show users recommended content, based on their interests. This step requires creating a [Dynamic Collection](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/about-collections-and-collection-pages) and showing this Collection on your website. Analytics Cloud sends to DXP a series of keywords based on users’ interests, and these keywords work as a filter in the Collection. You need a Collection to define the type of content you want to show to your users.

NOTE

For more information on how to work with Interests in Analytics Cloud, see the [Analytics Cloud documentation](https://learn.liferay.com/analytics-cloud/latest/en/people/segments/segments.html).

![Interests view in Liferay Analytics Cloud](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/experience-personalization/understanding-content-recommendations/images/01.png)

Analytics Cloud creates interests based on different content elements like tile, description, [Categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content), or [Tags](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/tagging-content-and-managing-tags). Adding Categories and Tags to your content is not strictly required to generate content recommendations. However, when you categorize and tag your content, Analytics Cloud has more information to define user’s interests and can provide better recommendations. Besides, [using Categories and Tags](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/organizing-content-with-categories-and-tags) provides you more control about the content you want to recommend.

NOTE

Analytics Cloud currently only considers titles and descriptions in English for creating users’ interests.

### [Configuring Content Recommendations](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/understanding-content-recommendations#configuring-content-recommendations) <a href="#configuring-content-recommendations" id="configuring-content-recommendations"></a>

Content recommendations work automatically as long as the following elements are in place:

1. [Analytics Cloud is connected to your Liferay DXP instance](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/configuring-content-recommendations#connecting-analytics-cloud-to-your-liferay-dxp-instance) and the Site content is synchronized.
2. You [create a Dynamic Collection](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/configuring-content-recommendations#creating-a-dynamic-collection) in Liferay DXP including different content, and enable the Content Recommendation option.
3. You [display the Dynamic Collection](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/configuring-content-recommendations#displaying-the-dynamic-collection) using an Asset Publisher, a Collection Display Fragment, or a Collection Page.

NOTE

Collections are named Content Sets in Liferay DXP 7.2.

The way you show Collections or Content Sets depends on your Liferay DXP version. In Liferay DXP 7.2, you show the Content Sets in a Display Page using an Asset Publisher. In DXP 7.3+, in addition to the Asset Publisher, you can show the Collection’s content using the Collection Display Fragment or a Collection Page. For more information on Collections, Collection Pages, and Content Sets, read [About Collections and Collection Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/about-collections-and-collection-pages).

For details about how to display content recommendations for your site, see [Configuring Content Recommendations](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/configuring-content-recommendations)

### [An Example of How to Implement Content Recommendations](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/understanding-content-recommendations#an-example-of-how-to-implement-content-recommendations) <a href="#an-example-of-how-to-implement-content-recommendations" id="an-example-of-how-to-implement-content-recommendations"></a>

To understand content recommendations in context, consider the following example. In your online Kitchenware store you offer a variety of kitchen-related products. A user visits your site and interacts with coffee accessories like coffee makers, coffee mugs, or coffee blenders. Based on the user interaction, Analytics Cloud generates a topic of interest for the user (“coffee”, in our example.)

In DXP, you create a Dynamic Collection where you include information and promotions for different types of products, like cutlery, cookware, food storage, and coffee accessories. When the same user accesses a page displaying this Collection, you Kitchenware site prioritizes coffee-related products and promotions over other types of products, showing this content more prominently to this user.

The more the user visits and interacts with coffee-related content, the higher “coffee” is associated in this user’s individual profile, increasing the number of content recommendations about coffee and decreasing the recommendations for other products.

### [Content Recommendations and Segments](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/understanding-content-recommendations#content-recommendations-and-segments) <a href="#content-recommendations-and-segments" id="content-recommendations-and-segments"></a>

Showing recommended content based on users’ behavior is not the same as personalizing users’ experience using [Segments](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments). When you use Segments, you categorize users into groups and target content for these particular groups. In Segment-based personalization, you target specific content to a specific group of users. In content recommendations, you don’t need to define a group because the content is automatically recommended to users based on their previous behavior on your site.

However, you can leverage the ability to [customize Collections with Segments](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/personalizing-collections) to provide content recommendations to specific groups of users. For example, you can combine the Dynamic Collection for content recommendations with a Segment that includes only visitors from Germany. In this case, only visitors to your site from Germany receive content recommendations from this Collection.

\
