# Adding Blog Entries

Blog entries are scoped to sites. By default, only authenticated users have the ability to create blog entries. See the [Blogs Permissions Reference](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/blog-permissions-reference) for more information.

You can [create a blog entry](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/adding-blog-entries#creating-an-entry) and [display it on a page](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/displaying-blogs).

TIP

Using a Blogs [widget](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-widget-pages/adding-widgets-to-a-page) is one of the easiest ways to display your blog entries on a page.

### [Creating an Entry](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/adding-blog-entries#creating-an-entry) <a href="#creating-an-entry" id="creating-an-entry"></a>

1. Open the _Site Menu_ ( ![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and go to _Content & Data_ → _Blogs_.
2.  Under the Entries tab, click _Add_ ( ![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) to add a new blog entry.

    ![Blog entry form editor](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/blogs/adding-blog-entries/images/01.png)
3. Drag and drop or select a file to upload as an optional cover image for the entry. By default, this cover image is published as part of the blog entry’s abstract.
4. Enter a title for the blog entry.
5. Enter an optional subtitle for the entry.
6. Enter the entry’s content in the _Content_ field. To learn more about the different text editing features, see [Blog Entry Editor Reference](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/blog-entry-editor-reference).

#### [Categorizing Blog Entries](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/adding-blog-entries#categorizing-blog-entries) <a href="#categorizing-blog-entries" id="categorizing-blog-entries"></a>

You can help users find content on your site by categorizing blog entries.

Use categories (public and internal), tags, or relate the entry to an asset to categorize your blog entry. See [Defining Categories and Vocabularies for Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content) to learn more about categories and vocabulary and [Tagging Content and Mananing Tags](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/tagging-content-and-managing-tags) to learn more about using tags.

To categorize your entry using categories and tags,

1. Expand the Categorization section.
2.  Click _Select_ to choose an existing category or tag.

    Alternately, you can create a new tag by entering the tag name in the Tags field and pressing Return.

To relate an asset to the entry,

1. Expand the Related Assets section and click _Select_ to select an asset and link it to the entry.

#### [Configuring How the Entry is Displayed](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/adding-blog-entries#configuring-how-the-entry-is-displayed) <a href="#configuring-how-the-entry-is-displayed" id="configuring-how-the-entry-is-displayed"></a>

To configure the blog entry and how it is displayed, expand the _Configuration_ and the _Display Page_ sections and adjust the following settings,

| Configuration            | Description                                                                                                                                                                                                                  |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| URL                      | Configure the entry’s URL: _Automatic_ sets the URL based on the entry’s title. _Custom_ prompts for a custom Friendly URL                                                                                                   |
| Abstract                 | Specify the entry’s abstract. By default, the Blogs application uses the first 400 words. _Custom Abstract_ prompts for a manual description.                                                                                |
| Small Image              | Add a small image to be shown as the entry’s thumbnail.                                                                                                                                                                      |
| Display Date             | Configure the display date to be used for the entry.                                                                                                                                                                         |
| Send Email Entry Updated | Send comments about the entry update.                                                                                                                                                                                        |
| Allow Pingbacks          | When you include a link to another website’s content in your post, and if that website supports pingbacks, it receives a notification or “ping” from your site. Pingbacks use XML-RPC requests to the URL you linked.        |
| Display Page Template    | Select the default [display page template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates), a specific one, or no display page template to display your blog entry. |

NOTE

When you update an entry’s friendly URL, Liferay stores the previous value in the entry’s friendly URL history. As long as the old URL remains in the history, Liferay redirects users to the latest version of the blog entry, and you cannot use it for other blog entries in the same site. See [Managing the Friendly URL History](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/displaying-blogs#managing-the-friendly-url-history) to learn more about managing the friendly URL history for blog entries.

![When creating a blog entry, use the Configuration panel to control when and where the blog entry appears, and what to use for the entry's abstract.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/blogs/adding-blog-entries/images/02.png)

#### [Publishing the Blog Entry](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/adding-blog-entries#publishing-the-blog-entry) <a href="#publishing-the-blog-entry" id="publishing-the-blog-entry"></a>

After you finish editing, categorizing, and configuring your blog entry, click _Publish_ to publish it.

Fellow site members can add comments, rate the entry, or share it on other social media sites.

![The Blog Entry has been published.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/blogs/adding-blog-entries/images/03.png)

\
