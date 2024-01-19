# Creating and Managing Forms

The Forms widget can do a lot of things really well, but if you just need a simple form, how do you wade through all the features you don’t need? Is your simple survey going to make you late for that lunch outing you’ve been planning with colleagues at that new shawarma place? No!

Let’s create a simple yet elegant form, give access to the intended users, and get you on your way to lunch.

At The Lunar Resort, it’s important to capture guests’ feedback about their stay at the resort. After a (hopefully) safe journey home, guests should receive an email with a link to brief survey that prompts them to rate their stay from a list of selections, and add any additional comments they’d like in an optional field.

![Figure 1: Get feedback from guests of The Lunar Resort.](https://liferay-support.zendesk.com/hc/article\_attachments/360030193352/forms-guest-survey.png)

### Viewing Forms <a href="#viewing-forms" id="viewing-forms"></a>

Whether creating a form or managing existing forms, it all starts in the same place: the Forms Application in your site’s Content section. Access this in the Menu, first choosing the site to work in (for example, The Lunar Resort) and clicking _Content & Data_ → _Forms_. The first thing you’ll see is a list of existing forms (if there are any). This list is styled by the Display Style selector next to the Add button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030193412/icon-add.png)). By default, forms are displayed in List format.

![Figure 2: Forms are displayed in List format by default.](https://liferay-support.zendesk.com/hc/article\_attachments/360030162151/forms-list-view.png)

There’s also a Table format. Change the style for a single site right here in the Forms site menu application, or change the default display style for the system scope in Control Panel → Configuration → System Settings → Forms (in the Content section). Click the _Forms_ entry and find the Default Display View property. Click _Update_ and your changes are propagated to all sites.

### Building a Form <a href="#building-a-form" id="building-a-form"></a>

To add a new form,

1. Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030193412/icon-add.png)). The form builder view appears.
2. Name the form. Replace _Untitled Form_ with _Guest Survey_.
3. For the description text enter _Tell us how your stay was!_
4.  Add the fields. Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030193412/icon-add.png)) to open the sidebar if it’s not already opened.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030193432/forms-sidebar.png" alt="Figure 3: You can choose from nine field types when creating forms." data-size="original">

    Figure 3: You can choose from nine field types when creating forms.
5.  Drag a _Select from List_ field onto the form builder and configure it like this:

    **Label:** _Rate your visit to The Lunar Resort._

    **Help Text:** Leave this blank for now. If you want a subheading for your field to provide additional guidance, this would be useful.

    Turn on the _Required Field_ selector. At a minimum, this form must capture whether guests like their stay or not.

    Leave the manual option checked for creating the list of selections.&#x20;

    Add these options:

    * _It was out of this world!_
    * _I had a good time._
    * _I’d rather go to the beach._
    * _I’ll never come back._

    Typing in one of the fields automatically adds another blank selection line. Just leave the last one blank when you’re done.
6. To see additional options, click _Properties_. Close the sidebar when finished.
7.  Add a text field, using the same procedure you used for the select field.

    **Label:** _Want to tell us more?_

    **Help Text:** Leave this blank again to give the form a consistent look.

    **My text field has:** Choose _Multiple Lines_. Let guests prattle on about their stay if they want to.

    **Required Field:** Leave this unselected. Only require guests to fill out the select field and leave this one as optional.
8. Close the sidebar.
9.  In the form builder, you can see the way the fields are laid out on the form page.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030193372/forms-form-builder.png" alt="Figure 4: The form builder page lets you preview your form layout, add a page to the form, or add some more fields." data-size="original">

    Figure 4: The form builder page lets you preview your form layout, add a page to the form, or add some more fields.
10. When the form is finished, click _Save Form_. It’s also auto-saved every minute by default.
11. Click _Publish Form_. A dedicated URL to the form is generated, but nobody has the URL yet.

Now your form can be added to a page, and Lunar Resort guests can be emailed and provided with a link to the page where the form is displayed.

### Accessing Forms <a href="#accessing-forms" id="accessing-forms"></a>

Once the form is developed and published, there are two options for getting the published form to targeted users:

1. Place the Form widget on a site page. This approach lets users navigate to the page in the site.
2. Copy the dedicated form URL and provide it to users (for example, via email). This limits access to the form to only those users who have the direct link.

To display the form on a site page in The Lunar Resort site:

1.  Add a page to the site (choose Full Page Application for the page type if you only want the form application on the page). Call it _Guest Survey_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030193332/forms-guest-survey-page.png" alt="Figure 5: Add a page for guests to view and fill out your new form." data-size="original">

    Figure 5: Add a page for guests to view and fill out your new form.
2. Add the Form widget to the page if you’ve chosen a Widget Page. If you used a full page application, use the page configuration to choose _Form_ from the Full Page Application dropdown.
3. Once the Form widget is on the page, click _Select Form_, choose the _Guest Survey_ form, and click _Save_.
4. Close the _Form—Configuration_ dialog window and your form is ready for Lunar Resort site users.

To display the form on a dedicated page accessed only by its link:

1.  In the form builder, click _Publish Form_ if you haven’t already.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030162171/forms-link-grayed.png" alt="Figure 6: You must first publish a form before you can get a shareable link." data-size="original">

    Figure 6: You must first publish a form before you can get a shareable link.
2.  Once published, click the link icon at the top right of the builder.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030193392/forms-link.png" alt="Figure 7: Copy the link to your form." data-size="original">

    Figure 7: Copy the link to your form.
3.  Once you get the link out to users, it’s showtime.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030193352/forms-guest-survey.png" alt="Figure 8: Lunar Resort guests can use a simple form to record their feelings about the resort." data-size="original">

    Figure 8: Lunar Resort guests can use a simple form to record their feelings about the resort.

Next you’ll learn how to view the form entries. Since there aren’t any yet, fill out and submit the form a few times.

Now you know the basics of creating and managing forms, but this presentation didn’t do the Forms application justice. It’s much more powerful than hinted at here. The remaining articles in this section immerse you in more advanced form building features.
