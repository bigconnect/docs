# Displaying Blogs

MWARE Portal provides two ways to display blog entries: display page templates and the Blogs widget.

### Using Display Page Templates <a href="#using-display-page-templates" id="using-display-page-templates"></a>

You can use display page templates to create reusable, dynamic displays for blog entries. Once set up, users can access each blog entry at a dedicated URL composed of the display page’s URL and the entry’s friendly URL (i.e., `display-page-url/blog-friendly-url`). See Publishing Content with Display Pages for instructions on how to set up display page templates for blog entries.

TIP

In content pages, you can use collections to link to groups of blog entry display pages. See About Collections and Collection Pages for more information.

#### Managing the Friendly URL History <a href="#managing-the-friendly-url-history" id="managing-the-friendly-url-history"></a>

Version 7.4+

Each blog entry has a friendly URL that is set automatically or manually during entry creation. If you change the friendly URL after creation, the portal stores the previous value in the entry’s friendly URL history. As long as the old URL remains in the history, the portal redirects users to the latest version of the blog entry, and you cannot use it for other blog entries in the same site. To reuse an old value for a new entry, remove it from the entry’s friendly URL history.

NOTE

By default, friendly URL redirects are temporary (302), but you can set them to permanent (301) if needed. See Configuring Friendly URL Redirects for more information.

To manage an entry’s friendly URL history,

1. Open the _Site Menu_ ( ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ), expand _Content & Data_, and go to _Blogs_.
2. Click the _Actions_ button ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the desired entry and select _Edit_.
3.  Expand the Configuration section and click the _History_ button ( ![History Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-history.png) ) in the Friendly URL field.

    ![Click the History button for the Friendly URL field.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/blogs/displaying-blogs/images/01.png)
4. Click the _Restore_ button ( ![Restore Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-restore2.png) ) to replace the current friendly URL with the selected URL.
5.  Click the _Forget URL_ button ( ![Forget URL Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-delete.png) ) to remove the selected URL from the entry’s history. Once removed, you can no longer use the URL to access the blog entry.

    ![Click the Restore or Forget URL buttons.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/blogs/displaying-blogs/images/02.png)

Changes to the friendly URL history are automatically saved.

### Using the Blogs Widget <a href="#using-the-blogs-widget" id="using-the-blogs-widget"></a>

Adding the Blogs widget to a page exposes the blog interface to site members. Similarly, adding the widget to a user’s personal site (Profile) creates a blog just for that user. The widget works the same way in both cases.

To add a Blogs widget to a page,

1. Create a new page or start editing a content page.
2. Click _Add_ ( ![Add widget icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add-widget.png) ) to open the Fragments and Widgets panel.
3. Under the Collaboration category in the Widgets tab, drag the _Blogs_ widget to a place on the page.

Once you’ve added a Blogs widget to a page, there are several display options for configuring the listing to look the way you want.

To configure the widget, click _Options_ (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) → _Configuration_.

![There are different setup options for the Blogs widget](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/blogs/displaying-blogs/images/03.png)

#### Setup Tab <a href="#setup-tab" id="setup-tab"></a>

**Enable Ratings:** Enable the ratings system for blog entries.

**Enable Comments:** Enable comments on blog entries.

**Show View Count:** Enable showing the number of views for each entry.

**Social Bookmarks:** Enable sharing blog entries on the social media platforms you add to the _Current_ column. To move social networking sites between the _Current_ and _Available_ columns, select the sites and use the arrows between those columns. Similarly, use the up/down arrows beneath the Current column to reorder the sites as they appear on each blog entry.

**Display Style:** Define how social bookmarks appear. _Inline_ is the default and displays the social bookmark icons in a row. _Menu_ hides them inside a share menu.

**Maximum Items to Display:** The total number of blog entries to display on the initial page. You can select up to 60 to display at once.

**Display Template:** Choose the display layout for blogs:

* **Abstract:** Displays an abstract of each blog entry.
* **Full Content:** Displays the full content of each blog entry.
* **Title:** Displays only the title of each blog entry.
* **Basic:** A stripped-down version of the Abstract, with less text and no cover image.
* **Card:** Displays each blog entry in a card-like rectangle that shows the cover image, title, author, post date, and a few lines of text.

![The card display template makes your blog posts look like fun little trading cards.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/blogs/displaying-blogs/images/04.png)

To select a different application display template (ADT) or create your own, click _Manage Templates_.

**Enable Report Inappropriate Content:** Enable flagging content as inappropriate, which sends an email to administrators.

**Enable Ratings for Comments:** Enable the ratings system for blog entry comments.

**Show Related Assets:** Enable showing related content from other widgets.

#### Communication, Sharing, and Scope Tabs <a href="#communication-sharing-and-scope-tabs" id="communication-sharing-and-scope-tabs"></a>

**Communication:** Lists public render parameters the widget publishes to other widgets on the page. Other widgets can read and take actions on these. For each shared parameter, you can specify whether to allow communication using the parameter and select which incoming parameter can populate it.

**Sharing:** Embed the widget instance as a widget on any website, Facebook, Netvibes, or as an OpenSocial gadget.

**Scope:** Specify the blog instance the widget displays: the current Site’s blog (default), the global blog, or the page’s blog.

\
