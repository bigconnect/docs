# Using Full Page Applications

Full Page Applications are the ideal way to display a Message Board, Wiki, or other application that demands a full page.

### Configuring the Page <a href="#configuring-the-page" id="configuring-the-page"></a>

Creating a Full Page Application starts just like creating any other type of page.

1. Go to _Site Administration_ → _Site Builder_ → _Pages_.
2. Click the (![Add Page](https://liferay-support.zendesk.com/hc/article\_attachments/360037581232/icon-add.png)) icon.
3. Select the _Full Page Application_ page type.
4. Give your page a _Name_ and click _Save_.
5.  Set the _Full Page Application_ to _Wiki_ and click _Save_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360037575191/full-page-app-configure.png" alt="Figure 1: The Full Page Application configuration page." data-size="original">

    Figure 1: The Full Page Application configuration page.
6.  Finish the page configuration and click _Save_.

    Out of the box, you can set the Blogs, Wiki, Media Gallery, Message Boards, RSS, Hello Soy Portlet, Documents and Media, Form, or Application Authorization Request to be the sole application for the page. Developers can make their applications Full Page Applications.
7. Click _Go to Site_ in the Site Administration menu, and then click on your page.

Now the page is configured to display the Wiki and only the Wiki. No other widgets can be added to the page, and the Wiki app cannot be removed.

Note that all of the applications that can be added to the page are non-instanceable and the content of whichever application you select is based on the instance for that site. So if you already had data in your Wiki it appears on this page.

If you want to configure the application to be scoped to this specific page, you can configure that through the application’s settings.

1. From the page, click the (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360037581252/icon-options.png)) button for the Wiki and select _Configuration_.
2. From the _Wiki - Configuration_ page, select the _Scope_ tab.
3. Open the _Scope_ menu and select _Space Wiki_.

![Figure 3: Configuring the scope.](https://liferay-support.zendesk.com/hc/article\_attachments/360037581292/configuring-scope.png)

Now the Wiki is scoped for this page, and doesn’t share data with the Site or globally scoped Wiki.
