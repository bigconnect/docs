# Personalizing Collections

By default, any user can see all the collection’s content and is part of the Anyone personalized variation. However, you can create a personalized variation of the collection’s items using segments. When you use a personalized variation, the items that are part of your collection don’t change, but the collection page, collection display fragment, or asset display widget show the items filtered for the segment.

IMPORTANT

To customize a collection using segments and set personalized variations for collections, you must define segments first. For more information, see Creating and Managing User Segments.

![Collections are available for anyone, by default](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/experience-personalization/personalizing-collections/images/01.png)

Consider the following example. You want to increase sales in your online Kitchenware Store by offering exclusive promotions to registered users. You create a collection including promotions for registered users and news about your products. You want all users to see the news about your products, but you want to restrict the promotions to registered users. In this example, you can create a new segment for registered users, and link the segment to a new personalized variation that filters the promotional content.

TIP

Create multiple personalized variations for a collection, and edit or delete them as needed. You can also edit the Anyone personalized variation, but you cannot delete it.

### Creating Personalized Variations <a href="#creating-personalized-variations" id="creating-personalized-variations"></a>

After you create segments for your users, you can add personalized variations for collections based on those segments.

1. Open the _Site Menu_ ( ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ) and navigate to _Site Builder_ → _Collections_.
2. Create a collection or select an existing one.
3.  Click _Add Personalized Variation_.

    If you already have a personalized variation in place and you have other available segments, add new personalized variations by clicking _Add_ (![add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add-app.png)) next to Personalized Variations.
4. Choose a Segment and customize the collection accordingly.

You can now control what users can see by prioritizing personalized variations or combining collection entries from all segments.

### Prioritizing Variations <a href="#prioritizing-variations" id="prioritizing-variations"></a>

for version 7.4+

For situations where a user may belong to multiple segments, you can [combine collections entries](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/personalizing-collections#combining-collection-entries-from-all-segments) from all the segments to which the users belong or prioritize a personalized variation to determine which personalized variation is displayed to a user.

When editing a collection, access a sortable list for the personalized variations and change the rank of a variation by clicking its _Actions_ button ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) and selecting _Prioritize_ or _Deprioritize_. You can also drag and drop variations to reorder their priority.

NOTE

Priority is assigned according to the position of each item in the list. The highest priority is the position at the top of the list.

![Prioritize collections variations to determine which variation is displayed for users](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/experience-personalization/personalizing-collections/images/02.png)

### Combining Collection Entries From All Segments <a href="#combining-collection-entries-from-all-segments" id="combining-collection-entries-from-all-segments"></a>

With users that belong to multiple segments, you can combine and show collection entries from all the segments to which they belong instead of showing only the default entries or the prioritized personalized variation.

1. Open the _Global Menu_ ( ![Applications Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ) and select the _Control Panel_ tab → _System Settings_ under Configuration.
2. Under Content and Data, select _Assets_.
3. Under System Scope, select _Collections_.
4. Enable the feature for Dynamic and Manual Collections.
5. Click _Save_.

![Enable the feature to combine assets from all segments in asset publisher for dynamic and manual collections.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/experience-personalization/personalizing-collections/images/03.png)

Users that belong to multiple segments can now see content from all segments.

For example, there are three segments: Registered Users, Brazilian Users, and Canadian users. If you are a registered user, you see only the content selected for that segment. The same works for Brazilian and Canadian users. However, if you are a Registered User who lives in Brazil, you can see the content selected for both segments. If you don’t belong to any segment, you see the content set to Anyone.

![Users that belong to multiple segments can see content from all segments](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/experience-personalization/personalizing-collections/images/04.png)

### version 7.2 <a href="#liferay-dxp-72" id="liferay-dxp-72"></a>

#### Content Set Personalization <a href="#content-set-personalization" id="content-set-personalization"></a>

Now, you’ll use segments to demonstrate content set personalization. For this example, create a content set to be the default displayed on the Home page. Then you’ll modify it to create a personalized variation containing technical articles for members of the American Engineers segment.

If you’re not familiar with content sets, see the Creating Content Sets article before you get started here.

**Creating and Setting the Default Content Set**

First create the default content set and configure it on the Home page using the asset publisher.

1. Open the _Site Menu_ ( ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ) and navigate to _Content and Data_ → _Content Sets_.
2. Click _Add_ ( ![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) and choose _Manual Selection_.
3. Name it _Home Page Content_.
4.  For the new content set, click _Select_ next to Asset Entries and select _Basic Web Content_.

    ![Click Select to add a new asset entry.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/experience-personalization/personalizing-collections/images/05.png)
5. On the Select Basic Web Content page, select the content you want to add and click _Add_.
6. Navigate to the Home page and add an Asset Publisher widget to the page.
7. Open _Configuration_ for the Asset Publisher.
8. Under Asset Selection select _Content Set_.
9. Under Select Content Set click _Select_, choose _Home Page Content_, and click _Save_.

Now the content set you configured appears in the Asset Publisher widget on the home page. Next configure the content set for personalization.

**Personalizing the Content Set**

Now create the content set for engineers and configure its display.

1. Go back to the content set from Site Administration.
2.  Click _New Personalized Variation_ and select the American Engineers segment

    ![Create a new personalized variation.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/experience-personalization/personalizing-collections/images/06.png)
3. Click _Select_ next to Asset Entries and select _Basic Web Content_.
4. Select articles appropriate to an engineering audience and click _Add_.

Now anytime a member of the American Engineers segment views this content set being displayed, they see the personalized version and not the default. Test this now, using the Simulator.

\
