# Content Set Personalization

In Creating User Segments you created a Segment called _American Engineers_. Now, you’ll use it to demonstrate Content Set Personalization. For this example, create a Content Set to be the default displayed on the _Home_ page. Then you’ll modify it to create a personalized variation containing technical articles for members of the _American Engineers_ segment.

If you’re not familiar with Content Set, see the Creating Content Sets article before you get started here.

### Creating and Setting the Default Content Set <a href="#creating-and-setting-the-default-content-set" id="creating-and-setting-the-default-content-set"></a>

First create the default Content Set and configure it on the Home page using the Asset Publisher.

1. Go to _Site Administration_ → _Content & Data_ → _Content Sets_.
2. Click the Add button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360029911971/icon-add.png)) and choose _Manual Selection_.
3. Name it _Home Page Content_.
4.  For the new Content Set, click _Select_ next to _Asset Entries_ and select _Basic Web Content_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360029911931/create-default-content-set.png" alt="Figure 1: Click Select to add a new Asset Entries." data-size="original">

    Figure 1: Click \*Select\* to add a new Asset Entries.
5. On the _Select Basic Web Content_ page, check the boxes next to the content you want to add and click _Add_.
6. Navigate to the _Home_ page and add an Asset Publisher to the page.
7. Open _Configuration_ for the Asset Publisher.
8. Under _Asset Selection_ select _Content Set_.
9. Under _Select Content Set_ click _Select_, choose _Home Page Content_, and click _Save_.

Now the Content Set that you configured appears in the Asset Publisher on the _Home Page_. Next configure the Content Set for Personalization.

### Personalizing the Content Set <a href="#personalizing-the-content-set" id="personalizing-the-content-set"></a>

Now create the content set for engineers and configure its display.

1. Go back to the Content Set from _Site Administration_.
2.  Click _New Personalized Variation_ and select the _American Engineers_ segment

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360029911951/create-personalized-variation.png" alt="Figure 2: Create a new Personalized Variation." data-size="original">

    Figure 2: Create a new Personalized Variation.
3. Click _Select_ next to _Asset Entries_ and select _Basic Web Content_.
4. Select articles appropriate to an engineering audience and click _Add_.

Now anytime a member of the _American Engineers_ segment views this Content Set being displayed, they see the personalized version and not the default. Test this now, using the _Simulator_.
