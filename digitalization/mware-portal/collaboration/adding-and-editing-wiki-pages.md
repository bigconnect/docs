# Adding and Editing Wiki Pages

Wiki nodes initially have no pages. When you navigate into a node for the first time, a default page called _FrontPage_ is created automatically. To view the page, click the wiki node’s name and then click _FrontPage_. The FrontPage appears and shows a message that explains the page is empty and needs you to add content. That message is a link; click it to start editing the page. The wiki page editing form then appears.

![Figure 1: Each empty wiki page presents a default message link you can click to edit the page.](https://liferay-support.zendesk.com/hc/article\_attachments/360029926232/wiki-empty-frontpage.png)

![Figure 2: The wiki page editing form lets you create and edit your pages content.](https://liferay-support.zendesk.com/hc/article\_attachments/360029910551/wiki-page-editor.png)

Follow these steps to use the wiki page editing form:

1. Enter your content in the field that contains the text _Write your content here…_. This is a rich-text, WYSIWYG editor that is almost identical to the one used in the Blogs app. The only difference is that the wiki editor uses Creole instead of HTML as its source. Click the link _Show Syntax Help_ if you need help with Creole syntax (e.g., syntax for text styling, header formatting, link creation, etc.).&#x20;
2. If you want to attach files to the page, open the _Attachments_ section of the form and add them via drag and drop or the _Select Files_ button.
3. If you want to associate a tag with the page, open the _Categorization_ section and enter a new or existing tag in the _Tags_ field.&#x20;
4. If you want to select an existing asset in the portal (e.g., a media file, blog post, etc.) to relate to the page, open the _Related Assets_ section and use the _Select_ button to select that asset.
5. In the form’s _Configuration_ section, you can set the page to use Creole (default), plain text, or HTML. We recommend that you stick with the Creole format, as it allows for a much cleaner separation of content and code. You can also use the Configuration section to summarize your edit, and specify whether it’s a minor edit.
6. Click _Publish_ to publish the page when you’re done editing it.

As is common with wikis in general, if you link to a page that doesn’t exist, clicking that link opens the new page form with a note stating that the page doesn’t exist and that you are creating it.

**Note**: When you create a page by clicking a link to a page that doesn’t exist, the new page is **not** a child of the current page. The page is created at the wiki node’s root. From Wiki in Site Administration, you can use the page’s Move action to assign it a new parent page. Clicking the Move action brings up a window that lets you select a new parent for the wiki page.

Return to the wiki node view to see a list of the node’s top-level pages. If you navigate to a page that has child pages, its child pages are listed. In these page listings, each page’s Actions menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029910511/icon-actions.png)) lists the following actions you can take on the page:

**Edit**: Opens the page in the page editor.

**Permissions**: Lets you determine which roles can view, update, delete, subscribe to, or set permissions on the page, and add, update, or delete page discussions (comments).

**Copy**: Opens a page editor window with all the content from the source wiki page. You’re prompted to specify a new title for it.

**Move**: Opens a dialog that lets you rename the page or assign the page to a new parent page within the wiki node.

**Add Child Page**: Create a new child page of the wiki page.

**Subscribe (or Unsubscribe)**: Subscribes you to (or unsubscribes you from) notifications for the wiki page’s modifications.

**Print**: Print the wiki page.

**Move to Recycle Bin**: Moves the wiki page to the Recycle Bin.

Each wiki page has a check box next to it. When you select a page’s check box, the Management Bar changes to show an Info icon (![Info](https://liferay-support.zendesk.com/hc/article\_attachments/360029926272/icon-information.png)) and Recycle Bin icon (![Recycle Bin](https://liferay-support.zendesk.com/hc/article\_attachments/360029926292/icon-trash.png)). To move the selected page to the Recycle Bin, click the Recycle Bin icon. To get additional information about the page via an info panel, click the Info icon. The info panel has a star icon that you can select to subscribe to the page’s modifications. The info panel’s Details section displays the page’s summary, format, version, creation and modification dates, number of attachments, and RSS link.

There are several more features in the wiki node view’s Management Bar. The _Filter and Order_ menu orders the pages by title or modification date and filters them by page type. The arrows button sorts the pages in ascending or descending order. The search bar searches for pages.

The _View Types_ button is next to the Info icon. It lets you choose how to display the pages. The View Types button’s icon depends on the selected view type:

**List** (![List](https://liferay-support.zendesk.com/hc/article\_attachments/360029910491/icon-view-type-list.png)): Shows the pages in a list with an icon representing each page. Each page’s entry contains the name of its author, when it was last modified, and its workflow status (e.g., Approved, Draft, etc.).

**Table** (![Table](https://liferay-support.zendesk.com/hc/article\_attachments/360029910531/icon-view-type-table.png)): Shows the same information as the List view type, in a smaller list with no page icon. Also, the page’s information is in columns and includes the revision number.

![Figure 3: The wiki nodes view in site administration has features that help you access and learn information about a wiki nodes pages.](https://liferay-support.zendesk.com/hc/article\_attachments/360029926252/wiki-node-view-in-admin.png)
