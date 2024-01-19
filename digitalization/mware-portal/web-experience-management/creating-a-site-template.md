# Creating a Site Template

Suppose you need to create the following three private Sites for the Lunar Resort’s internal use: Engineering, Marketing, and Legal. These should be accessible only to members of these respective departments. You could design each Site separately, but you can save yourself time if you create a Site template instead.

1. Go to the Control Panel and click _Sites_ → _Site Templates_.
2. Click the _Add_ icon (![Add Site Template](https://liferay-support.zendesk.com/hc/article\_attachments/360030189312/icon-add.png)) and enter the name _Department_ for your template.
3. Leave the _Active_ and _Allow Site administrators to modify pages associated with this Site template…_ boxes checked.
4. Click _Save_ to create your Site template.

The _Active_ box must be checked for your template to be usable. If your template is still a work in progress, uncheck it to ensure that no one uses it until it’s ready. Checking _Allow Site administrators to modify pages associated with this Site template…_ allows Site administrators to modify or remove the pages and apps that the template introduces to their Sites—if you want the templates to be completely static, you should uncheck this.

Now it’s time to edit your Site template. This example, includes four pages.

Click the _Options_ icon (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030189332/icon-options.png)) and select _Manage_.

1. This brings you to the _Pages_ page for the Site Template. You already have a home page. Create three more pages.
2. Create a _Full Page Application_ page named _Documents_.
3. Click _Options_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030189352/icon-actions.png)) → _Configure_ and set the _Full Page Application_ to _Documents and Media_.
4. Create a page using the Global Page Template _Wiki_ and name it _Wiki_.
5. Create a widget page named _Message Boards_.
6. Click _Go to Site_ in the menu to the left to go to the pages you just created.
7. On the _Home_ page add the Activities, Announcements, and Calendar apps.
8. On the _Message Boards_ page add the Message Boards and Tag Cloud apps.

The changes you made to your Site template above are completed in real time, so there’s no _Save_ button.

![Figure 1: You can see the name of the Site template youre currently editing.](https://liferay-support.zendesk.com/hc/article\_attachments/360030158031/editing-site-template.png)

Next, you’ll use your Site template to create the Engineering, Marketing and Legal Sites.

1. Go to the Control Panel and click on _Sites_ → _Sites_.
2. Click the _Add_ icon (![Add Site](https://liferay-support.zendesk.com/hc/article\_attachments/360030189312/icon-add.png)) → _Department_.
3. Enter _Engineering_ for the Site name.
4. Check the _Create default pages as private (available only to members). If unchecked, they will be public (available to anyone)_ option since the Engineering Site is intended for internal use only.
5. Click _Save_.
6.  In the next section, set the Membership Type to _Private_.

    Recall that private Sites don’t appear in the My Sites application so that regular users won’t even know that the Engineering Site exists. Also, the only way users can be added to a private Site is via an invitation from a Site administrator.
7. Leave the _Active_ selector enabled so that your Site can be used immediately.
8. Check the _Create default pages as private (available only to members). If unchecked, they will be public (available to anyone)._ option since the Engineering Site is intended for internal use only.
9. Leave the _Enable propagation of changes from the Site template_ box enabled so that the Engineering Site receives updates if the Department Site template is modified.
10. Click _Save_ to create your Engineering Site.
11. Repeat these steps to create the Marketing and Legal Sites.

The new Sites have all the pages and apps you created in the Site template. To view the pages of the new Sites, click _Sites_ → _Sites_ in the Control Panel and then click _Actions_ → _Go to Private Pages_ next to one of your new Sites.

Using Site templates streamlines the Site creation process for administrators, making it easy to create Sites quickly. Now each department of the Lunar Resort has its own Calendar, Documents and Media Library, Wiki, and Message Boards on their Sites. Although the pages and apps of each department’s Site are the same, each Site will quickly be filled with department-specific information as users add and share content within the Sites. Also, Site administrators can add new pages, apps, and content to their Sites, further differentiating each department’s Site from the others.
