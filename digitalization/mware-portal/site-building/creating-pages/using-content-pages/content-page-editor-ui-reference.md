# Content Page Editor UI Reference

Using the content page editor, you can add and configure elements in a content page. To open the content page editor, go to a content page and click _Edit_ (![Edit icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit.png)). This reveals the editing toolbar and sidebar, which contain tools and components for building content pages.

* Editing Toolbar
* Editing Sidebar

![Click the Edit button to access the editing toolbar and sidebar.](<../../../../../.gitbook/assets/01 (1) (2).png>)

### Editing Toolbar <a href="#editing-toolbar" id="editing-toolbar"></a>

The content page toolbar contains tools for creating unique user experiences, setting localized translations, switching between edit modes, and more.

![Click the Edit button to access the editing toolbar.](<../../../../../.gitbook/assets/02 (1) (2).png>)

#### Experiences <a href="#experiences" id="experiences"></a>

Using the Experience drop-down menu, you can create copies of the current page to customize for a specific audience. See Content Page Personalization for more information.

#### Translations <a href="#translations" id="translations"></a>

Click the language flag to access the translation drop-down menu. Here, you can view the status of active languages for a page, as well as switch between languages to view and edit translations directly. See Configuring Site Languages for more information.

If you use experiences for content page personalization, you can configure the experience using the languages available for your site. See Managing Experience Localization for more information.

#### Device Display <a href="#device-display" id="device-display"></a>

Click the device icons to access portal’s responsive layout editor. Here you can configure your page for different screen sizes. Available display options include desktop ( ![Desktop](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-display.png) ), tablet ( ![Tablet](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-tablet-portrait.png) ), landscape phone ( ![Landscape Phone](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-mobile-landscape.png) ), and portrait phone ( ![Portrait Phone](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-mobile-portrait.png) ). While editing the tablet and phone layouts, you can adjust the screen size manually using the resize handlebar. See Building Responsive Layouts for more information.

NOTE

The fragments and widgets and page design options panel are only available in the desktop layout.

#### Edit History <a href="#edit-history" id="edit-history"></a>

Click _History_ ( ![History Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-time.png) ) to view changes made during your current edit session. You can use this history to reserve or restore changes. Alternatively, you can click the Undo and Redo buttons.

![Revert your changes using the Undo, Redo, and History buttons.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/using-content-pages/content-page-editor-ui-reference/images/03.png)

#### Edit Mode Selector <a href="#edit-mode-selector" id="edit-mode-selector"></a>

The editing options available in the content page sidebar depend on the editing mode you choose.

| Edit Mode       | Description                                                                                | Available Options                                                                                                         |
| --------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| Page Design     | You can design pages when you have permission to update the page and the page content.     | <ul><li>Browser</li><li>Page Content</li><li>Comments</li></ul>                                                           |
| Content Editing | You can edit content when you have permission to update the page but not the page content. | <ul><li>Fragments and Widgets</li><li>Browser</li><li>Page Design Options</li><li>Page Content</li><li>Comments</li></ul> |

![Choose between Page Design or Content Editing editing modes.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/using-content-pages/content-page-editor-ui-reference/images/04.png)

#### Toggle Sidebars <a href="#toggle-sidebars" id="toggle-sidebars"></a>

Click _Toggle Sidebars_ ( ![Toggle Sidebars](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-view.png) ) to hide the editing sidebars and preview your page. Click _Actions_ ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) in the application bar and select _Preview in a New Tab_ to see the current page experience and language in a new tab.

#### Convert to Page Template <a href="#convert-to-page-template" id="convert-to-page-template"></a>

Click _Actions_ ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) in the application bar and select _Convert to Page Template_ (![Page Template](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-page-template.png)) to create a page template based on the current page design.

#### Publish <a href="#publish" id="publish"></a>

By default, publishing makes your changes live immediately. However, if workflow process is configured to direct the review and publishing process, clicking Publish initiates your defined procedure.

### Editing Sidebar <a href="#editing-sidebar" id="editing-sidebar"></a>

Depending on your selected editing mode, you can access these panels in the editing sidebar:

* ![Fragments and Widgets](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png) - Fragments and Widgets
* ![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png) - Browser (previously _Selection_)
* ![Page Design Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-format.png) - Page Design Options
* ![Page Content](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-paperclip.png) - Page Content (previously _Contents_)
* ![Comments](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-message.png) - Comments

![The editing sidebar includes these panels.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/using-content-pages/content-page-editor-ui-reference/images/05.png)

#### Fragments and Widgets <a href="#fragments-and-widgets" id="fragments-and-widgets"></a>

In the Fragments and Widgets panel ( ![Fragments and Widgets icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png) ), you can access both out-of-the-box and custom page elements.

**Fragments**: Fragments are extensible, reusable, drag-and-drop elements that serve as the primary building blocks for content pages. Examples include layout elements, videos, and banner images with text overlay. See Using Fragments to learn more about the out-of-the-box fragments or Developing Fragments to learn how to create your own.

**Widgets**: Widgets are applications that add dynamic functionality to a page. Examples include blogs, message boards, and calendars. See Using Widgets on Content Pages for more information.

TIP

Click the Order button (![Order Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-order.png)) to reorder sets of fragments and widgets. Click the Star button (![Star Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-star.png)) to add a fragment or widget to your list of favorite elements.

#### Browser <a href="#browser" id="browser"></a>

Use the Browser panel ( ![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png) ) to view, organize, and manage all elements in the page. Elements are displayed in a hierarchy. You can drag and drop elements in the Browser panel to reorganize them or double click elements to rename them. Selecting an element opens its settings in the configuration side panel. Select a fragment’s sub-elements to access additional settings. See Configuring Fragments for more information.

NOTE

When editing a content page, header and footer elements are not included in the Browser panel. You can only edit these elements in master page templates.

#### Page Design Options <a href="#page-design-options" id="page-design-options"></a>

Open the _Page Design Options panel_ (![Page Design Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-format.png)) to change the page’s master page template or style book. Click _Options_ (![Options button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-cog3.png)) to access additional page design options, including front-end client extensions.

#### Page Content <a href="#page-content" id="page-content"></a>

In the Page Content panel ( ![Page Content](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-paperclip.png) ), you can view and edit content directly added to the page. This can include inline text, images, web content articles, and more. While viewing content, you can search for content items and filter by type. If the page includes a collection, you can view its items and add new items. See Managing Content in Content Pages for more information.

NOTE

Besides collections, mapped content is not listed in the Page Content panel. The panel only includes content added directly to the page.

#### Comments <a href="#comments" id="comments"></a>

In the Comments panel ( ![Comments](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-message.png) ), you can comment on fragments and widgets to collaborate with page contributors. You can also view, edit, delete, or reply to open comments. If [mentions](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/notifications-and-requests/user-guide/mentioning-users) are enabled, you can tag users with page update permissions as part of your comment. To add a comment, open the Comments panel and select the desired page element.

#### Browser <a href="#browser-1" id="browser-1"></a>

In the Selection panel (![Selection](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-pages-tree.png)), you can view a hierarchical outline of all page elements, as well as select, configure, or remove those elements. Similarly, clicking any element on the page while in the design mode opens the Selection panel and highlights the selected page element in the hierarchy. When an element is selected, you can access its general, styles, and link options, if it has any. See Configuring Fragments for more information.

![Selection shows you a hierarchy of your page and contents.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/using-content-pages/content-page-editor-ui-reference/images/06.png)

NOTE

A page’s header and footer are not listed in the Selection panel hierarchy, because you can only modify them in the master page template.

#### Contents <a href="#contents" id="contents"></a>

In the Contents panel (![Contents](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-list-ul.png)), you can view, edit, and manage all web content on the page. This includes content displayed in widgets and mapped to content fields. See Managing Content in Content Pages for more information.

\
