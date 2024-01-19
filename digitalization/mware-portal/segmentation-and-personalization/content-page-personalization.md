# Content Page Personalization

In Creating Segments with Customer Fields and Session Data you created a Segment called _Premium Card Prospects_. Now, you’ll use it to demonstrate Content Page Personalization.

If you’re not familiar with Content Pages, see the Creating Content Pages article before you get started here.

### Creating the Default Page <a href="#creating-the-default-page" id="creating-the-default-page"></a>

First, you need to create the _Credit Cards_ page. To do this, create a new content page, add some fragments to it, and edit the content to match. First you need to build the bones of the page:

1. Go to _Site Administration_ → _Site Builder_ → _Pages_.
2. Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360033265631/icon-add.png)).
3. Select _Content Page_ and name the page _Credit Cards_.
4. Open the _Sections_ → _Basic Sections_ from the build menu on the right side of the screen and add a _Banner_ to the page.
5. Next, click on _Section Builder_, open _Layouts_, and add a three column layout to the page, above the banner.

![Figure 1: Open Layouts from the Section Builder.](https://liferay-support.zendesk.com/hc/article\_attachments/360029928032/section-builder-layouts.png)

1. Open the _Basic Components_ tab and add a _Card_ inside each of the columns.

Next, edit the content (this step is optional):

1. Click on the text for the cards and the banner to edit it and provide content relevant to someone looking for information about credit cards.
2. Click on each image and provide an appropriate image.
3. Click _Publish_.

![Figure 2: Your final result might look something like this.](https://liferay-support.zendesk.com/hc/article\_attachments/360029911871/personalization-default-content.png)

This is the default page that anyone visiting the site sees.

### Defining Custom Experiences <a href="#defining-custom-experiences" id="defining-custom-experiences"></a>

Next, define an experience specifically tailored to customers whom you have identified as _Premium Card Prospects_ using User Segments.

1.  At the top of the page, for the _Experience_ click on _Default_ to open the experience selection dialog.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360033265611/select-experience.png" alt="Figure 3: Click on the current experience to create a new one or select a different existing experience." data-size="original">

    Figure 3: Click on the current experience to create a new one or select a different existing experience.
2. Click on _New Experience_.
3. Name it _Card Prospects_ and select _Premium Card Prospects_ for the _Audience_.
4. Add a _Banner_ fragment above the three columns you added earlier.
5. Edit the Banner to provide information specifically related to upgrading a card for an existing customer.
6. Click _Publish_.

The _Default_ version of the page appears for everyone except for those defined as _Premium Card Prospects_, but customers in that segment have an experience curated just for them.

![Figure 4: Your final result for the card prospects might look something like this.](https://liferay-support.zendesk.com/hc/article\_attachments/360029928012/personalization-prospects.png)

**Note:** When you create a new experience, it copies the _Default_ experience at the time that it is created. Any further changes to the _Default_ experience do not effect any of experiences for that page.
