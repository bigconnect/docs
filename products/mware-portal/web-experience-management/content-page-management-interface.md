# Content Page Management Interface

Unlike Widget Pages, Content Pages can only be edited through the _Site Builder_ and cannot be edited live on the page. Any edits that you make to a page are saved as a draft until you publish the page. Subsequent changes after the initial publication are again saved as a draft, without affecting the live page, until the page is published again. To create a Content Page,

1. Go to _Site Management_ → _Site Builder_ → _Pages_.
2. Click ![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030157411/icon-add.png).
3.  On the next page, select _Content Page_ and provide a name for the page.

    You will be brought to the Content Page management interface.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030157451/content-page-edit-blank-page.png" alt="Figure 1: Each Content Page starts as a blank page." data-size="original">

    Figure 1: Each Content Page starts as a blank page.

To edit an existing Content Page,

1. Go to _Site Administration_ → _Site Builder_ → _Pages_.
2. Click _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360034582812/icon-staging-bar-options.png)) → _Edit_ next to the Content Page you want to edit.

You can also get to this page by selecting the _Edit_ button (![Edit](https://liferay-support.zendesk.com/hc/article\_attachments/360030188472/icon-edit-pencil.png)) from the Control Menu if you’re viewing the published Content Page.

On this page you can view a preview of your page, add Fragments and Widgets, and manage the configuration for the page or any Fragments and Widgets currently residing on the page.

Your tools for building the page are all found on the right side of the page. From top to bottom, the options are

* Sections
* Section Builder
* Widgets
* Page Structure
* Look and Feel

#### SECTIONS <a href="#sections" id="sections"></a>

When you open _Sections_ you see a list of Collections available. Initially, you only have the **Basic Sections** Collection which is included with the product. You can open the Collection and drag Sections directly onto the page.

![Figure 2: Sections contains Fragments that fully define spaces on your page.](https://liferay-support.zendesk.com/hc/article\_attachments/360030157551/content-page-sections-editor.png)

Once a section is added to a page, you can edit its background color, background image, and spacing. Since these options are available to marketers and administrators editing a page, the options are limited, and the color palette can be set by the Fragment developer.

![Figure 3: The Section managment tool provide powerful tools, but with the training wheels still on.](https://liferay-support.zendesk.com/hc/article\_attachments/360030188512/content-page-sections-config.png)

#### SECTION BUILDER <a href="#section-builder" id="section-builder"></a>

In Section Builder, you start with _Layouts_ and _Basic Components_. Add Layouts to the page to provide a spaces where you can add Components.

![Figure 4: Sections Builder contains Component Fragments which are intended to be combined to create Sections.](https://liferay-support.zendesk.com/hc/article\_attachments/360030157511/content-page-section-builder-editor.png)

#### WIDGETS <a href="#widgets" id="widgets"></a>

The Widgets section functions just like the _Add_ menu on a Widget Page. The full list of available widgets is displayed, and you can add them to the page.

![Figure 5: The Widgets section provides a list of Widgets that can be added inside of a Layout.](https://liferay-support.zendesk.com/hc/article\_attachments/360030157431/content-page-widget-editor.png)

The main difference is that only the main configuration options are available for widgets on Content Pages. Various other configurations like _Look and Feel_ are only available for widgets on Widget Pages.

#### PAGE STRUCTURE <a href="#page-structure" id="page-structure"></a>

Page Structure provides a high level view of every Fragment and every field within each Fragment on the page.

![Figure 6: Page Structure shows you a hierarchy of your page.](https://liferay-support.zendesk.com/hc/article\_attachments/360030188452/content-page-page-structure-editor.png)

Figure 6: \*Page Structure\* shows you a hierarchy of your page.

Clicking on a field in Page Structure will highlight it on the page. On large complicated page, this helps you keep on top of where everything is and also access items that might be hard to click on directly.

#### LOOK AND FEEL <a href="#look-and-feel" id="look-and-feel"></a>

Click the _Look and Feel_ icon (![Look and Feel](https://liferay-support.zendesk.com/hc/article\_attachments/360037575131/icon-look-and-feel.png)) to change the theme or manage other options for the page. These options are fully documented in Creating Pages.

#### COMMENTS <a href="#comments" id="comments"></a>

You can also comment on any page fragments. This allows discussion and collaboration for teams creating content pages.

Comments are disabled by default, but administrators can enable them from _Control Panel_ → _Configuration_ → _System Settings_ → _Pages_ → _Content Page Editor_. Select the _Comments Enabled_ checkbox and click _Update_. This enables content page comments for all instances. To control this on an instance-by-instance basis, navigate to the same setting in _Instance Settings_ (instead of System Settings).

![Figure 7: Administrators can enable comments for content pages.](https://liferay-support.zendesk.com/hc/article\_attachments/360037575111/enable-content-page-comments.png)

If comments are enabled, you can access them via the _Comments_ icon (![Comments](https://liferay-support.zendesk.com/hc/article\_attachments/360037575091/icon-comments.png)). The comments appear for the selected fragment. You can take the following actions in the comments UI for a fragment:

* Add new comments and reply to any existing ones.
* Resolve comments by clicking the checkbox for each. Resolving a comment hides it from view, unless _Show Resolved Comments_ is selected.
* Edit and delete your own comments via the Actions button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360037581072/icon-actions.png)) for each.

If you de-select a fragment or enter the comments UI without a fragment selected, a list of the fragments on the page appears with the number of comments for each. Selecting a fragment then shows its comments.

![Figure 8: When creating content pages, you and your team can comment on any fragments.](https://liferay-support.zendesk.com/hc/article\_attachments/360037581092/content-page-comments.png)

#### THE TITLE BAR <a href="#the-title-bar" id="the-title-bar"></a>

The title bar provides navigation back to the Main Menu, a link to page configuration, and the ability to search for other pages. The title bar is covered in more detail in Creating Pages.

![Figure 9: The title bar has several tools built into it.](https://liferay-support.zendesk.com/hc/article\_attachments/360030157371/content-page-edit-title-bar.png)

