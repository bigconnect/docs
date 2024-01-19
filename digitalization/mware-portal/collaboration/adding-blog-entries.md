# Adding Blog Entries

Each site contains a built-in blog instance, so you can add blog entries to it right away. The easiest way to do this is in the Site Administration menu. Follow these steps to add a blog entry in Site Administration:

1. Click the Menu button (![Menu](https://liferay-support.zendesk.com/hc/article\_attachments/360029925312/icon-menu.png)) to open the product menu. Then expand the menu for your site and select _Content & Data_ → _Blogs_. This takes you to the Blogs app for your site. The _Entries_ tab is selected by default, which lists the site’s blog entries.
2.  Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360029925232/icon-add.png)) to create a new blog entry. This presents the blog entry editor. Note that the same form appears when editing a blog entry.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360029909771/blogs-new-entry.png" alt="Figure 1: This screenshot shows some of the blog entry editors controls." data-size="original">

    Figure 1: This screenshot shows some of the blog entry editor's controls.
3.  The first input field, _Drag & Drop to Upload_, is for optionally adding a cover image for your entry. By default, an Asset Publisher shows this cover image as part of the blog entry’s abstract. You can insert any image you like in this field, either via drag and drop or the _Select File_ button. The latter lets you choose an existing image in the blog, an image from Documents and Media, or an image that you upload from your machine.

    If you select an image from Documents and Media, you can make changes to it with the Image Editor. Edits you make are applied automatically to a copy of the image, which you can then use as your cover photo.

    Upon upload, the image appears in the pane. To center the image, drag it into place. You can also add a caption. If you want to select a different image, click the _Change_ icon (![Change](https://liferay-support.zendesk.com/hc/article\_attachments/360029909791/icon-change.png)). Clicking the trash can icon removes the image from the blog entry.
4. Enter a title for your blog entry. You can also add a subtitle if needed.
5. Enter your blog entry’s content in the _Content_ field. This field is small at first, but it expands as you add content. The editor displays the editing controls when you need them and hides them from view when you don’t. When you select text in your blog post, for example, a bar with context-specific editing controls appears. This keeps your canvas uncluttered so you can focus on writing. You can also add images, videos, and tables in your blog entry’s content.&#x20;
6. Expand the _Categorization_ panel and associate your blog entry with tags and/or categories. Although this is optional, it improves search results for blog entries and gives your users more navigation options. For example, you can add the Tags Navigation app to another column on your blogs page, which lets users browse blog entries by tag.
7. Expand the _Related Assets_ panel and choose any other content in your site that you want to associate with this blog entry. Although this is optional, related assets let you tie together content on your site. For example, you might want to write a blog entry about a discussion that happened on the forums. To link those two assets together, select the forum thread under Related Assets.&#x20;
8.  Expand the _Configuration_ panel if you want to customize your blog entry’s URL, abstract, or display date. You can also set whether to allow pingbacks for your blog entry. For the URL, the default selection of _Automatic_ generates the URL for you based on the blog entry’s title. This URL appears in the _Blog Entry URL_ text box. Selecting _Custom_ lets you enter your own URL. Note that if you change the blog entry’s URL after publishing the entry, the original URL redirects to the new URL.

    You can also specify the blog entry’s abstract. Enter a 400 character text-only abstract, or a custom abstract that contains a thumbnail image and a manually written description. The _Small Image_ section lets you add a small image that appears when blog entries are displayed in list view. Below the abstract section, you can set the entry’s display date and time.

    Note that if you’re editing an existing blog entry, the _Send Email Entry Updated_ toggle appears. Setting this to _YES_ sends an email to any subscribers when the blog entry is updated. You can customize this email when configuring the Blogs app.

    Finally, you can allow _pingbacks_ for the blog entry. Pingbacks are XML-RPC requests that are sent automatically when you link to another site. If you link to another site in your blog entry, the portal sends a pingback to the other site to notify that site that you linked to it. Similarly, if someone links to your blog entry, the portal can receive a pingback from that site and record the link.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360029925292/blog-entry-configuration.png" alt="Figure 2: When creating a blog entry, the Configuration panel lets you control when and where the blog entry appears, and what to use for the entrys abstract." data-size="original">

    Figure 2: When creating a blog entry, the Configuration panel lets you control when and where the blog entry appears, and what to use for the entry's abstract.
9.  Expand the _Display Page Template_ panel if you want to select a display page template for displaying your blog entry. The following options are available:

    **Default Display Page Template:** Use the default display page template.

    **Specific Display Page Template:** Click the _Select_ button to select the display page template you want to use.

    **No Display Page Template:** Do not use a display page template.
10. Expand the _Permissions_ panel to customize your blog entry’s permissions. Use the _Viewable by_ selector to set who can view the blog entry:

    **Anyone (Guest Role):** Anyone, including guests, can view the entry.

    **Site members:** Only site members can view the entry.

    **Owner:** Only the entry’s owner can view the entry.

    Click the _More Options_ link to bring up a permissions table that lets you grant or revoke the following permissions for guests and site members:

    **Update Discussion**: Edit another user’s comment on the blog entry.

    **Delete**: Move the blog entry to the Recycle Bin.

    **Permissions**: View and modify the blog entry’s permissions.

    **Delete Discussion**: Delete any comments on the blog entry.

    **Update**: Edit and modify the blog entry.

    **View**: View the blog entry.

    **Add Discussion**: Comment on the blog entry.
11. Click _Publish_ to publish your blog entry. It now appears in the _Entries_ tab.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360029925272/blog-entries-site-admin.png" alt="Figure 3: The Blogs app in Site Administration lists the sites blog entries." data-size="original">

    Figure 3: The Blogs app in Site Administration lists the site's blog entries.
