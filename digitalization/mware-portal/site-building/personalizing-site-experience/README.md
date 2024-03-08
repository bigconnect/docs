# Personalizing Site Experience

Liferay DXP provides powerful tools out-of-the-box for understanding and delivering personalized experiences to users.

For example, you can create [user segments](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments) and map them to different [experiences](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/creating-and-managing-experiences). With experiences in place, [personalize collections](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/personalizing-collections) and customize the way the content or the [pages](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/content-page-personalization) that users see.

TIP

When a user belongs to multiple segments, you can [prioritize a certain personalized variation](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/personalizing-collections#creating-personalized-variations) or [combine collection entries from all segments](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/personalizing-collections#combining-collection-entries-from-all-segments).

### [Segmentation](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience#segmentation) <a href="#segmentation" id="segmentation"></a>

Using the Segment Builder, you can group similar users by user profile attributes, behavior, site role, and more.

![Build user segments by checking for different user property values.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/images/01.png)

See [Creating and Managing a Segment](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments) to get started.

NOTE

You can assign roles to user segments. See [Assigning Roles to User Segments](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/assigning-roles-to-user-segments) for more information.

#### [Tracking Behavior of User Segments with Liferay Analytics Cloud](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience#tracking-behavior-of-user-segments-with-liferay-analytics-cloud) <a href="#tracking-behavior-of-user-segments-with-liferay-analytics-cloud" id="tracking-behavior-of-user-segments-with-liferay-analytics-cloud"></a>

Supercharge your segments by integrating your Liferay DXP instance with Liferay Analytics Cloud. Analytics Cloud integrates DXP with Salesforce(tm) and other data sources to provide even more ways to understand your user base.

Integration with Analytics Cloud provides in-depth, machine-learning powered, data analytics capabilities to help you understand user behavior and engagement for every page of your DXP site. Analytics Cloud enables you to see user behavior and interaction with both standard and targeted content. See [Getting Analytics for User Segments](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/getting-analytics-for-user-segments) for more information.

![Site metrics using Analytics Cloud.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/images/02.png)

### [Personalization](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience#personalization) <a href="#personalization" id="personalization"></a>

Once you [create user segments](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments), you can begin tailoring site [experiences](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/creating-and-managing-experiences) specific to a user segment. Content page personalization and content set personalization are two methods you can use to deliver personalized user experiences.

#### [Content Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience#content-pages) <a href="#content-pages" id="content-pages"></a>

Content page personalization dynamically changes the page layout and content based on the current user. You can create experiences that provide different text, images, widgets, and even different layouts for any [content page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages) based on the current user’s user segment criteria. See [Content Page Personalization](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/content-page-personalization) for more information.

![You can create unique experiences for different segments of users.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/images/03.png)

#### [Collections](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience#collections) <a href="#collections" id="collections"></a>

[Collections](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/about-collections-and-collection-pages) organize and display content. [Collections personalization](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/personalizing-collections) provides a selection of collections based on user segments. This means the content set that appears in a given context is determined by the [user segment](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments) criteria. For example, you could use a collection to show “featured” articles at the top of a page. Then you could create user segments containing users who should receive more specialized content, rather than the default. Those segments would then see content personalized to their interest rather than the default. See [Personalizing Collections](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/personalizing-collections) for more information.

![You can personalize content sets to display assets for specific user segments.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/images/04.png)

NOTE

Collections are named Content Sets in Liferay DXP 7.2.

#### [Previewing User Experiences](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience#previewing-user-experiences) <a href="#previewing-user-experiences" id="previewing-user-experiences"></a>

You can preview the different experiences that users can have on a page by clicking the _Simulation_ button (![Simulation](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-simulation.png)) at the top of the page and selecting a user segment from the Segments selection to preview the page as a member of that user segment.

Viewing the perspective of a user segment previews any personalizations for content pages or content sets for that User Segment.

![You can preview different experiences from the Preview panel.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/images/05.png)
