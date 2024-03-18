# Scoping Your Message Boards

Multiple message boards can be created and managed on the same DXP instance at the same time. This can be accomplished by scoping each message board to a particular scope. The [Global, Site, or Page Scopes](https://help.liferay.com/hc/articles/360028819992-Widget-Scope) are available. The following sections show administrators how to setup _Message Boards_ in each of these scopes.

### [Global](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards#global) <a href="#global" id="global"></a>

Content created at the global site scope level is available on all other sites but is not _automatically_ visible on every site page.

#### [Creating a Globally Scoped Message Board](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards#creating-a-globally-scoped-message-board) <a href="#creating-a-globally-scoped-message-board" id="creating-a-globally-scoped-message-board"></a>

To create a message board in the global scope, follow these steps:

1. Navigate to the site page where a _Message Boards_ widget has been deployed (for example, _Liferay DXP_’s _Forum_).
2. Click _Options_ (![widget options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-widget-options.png)) → _Configuration_.
3. Click _Scope_.
4.  Select **Global** from the _Scope_ dropdown menu.

    ![Figure 3. Selecting the Global scope](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards/images/07.png)
5. Click _Save_ and close the window.

The _Message Boards_ widget is now configured to display and create threads at the _Global_ scope.

![Figure 4. Viewing globally scoped content on another site](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards/images/08.png)

### [Managing a Globally Scoped Message Board](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards#managing-a-globally-scoped-message-board) <a href="#managing-a-globally-scoped-message-board" id="managing-a-globally-scoped-message-board"></a>

To centrally manage a globally scoped message board:

1. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), then click the compass icon (![Compass](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-compass.png)) on the Site Administration menu. This opens the Select Site dialog.
2.  Select the _My Sites_ tab, then select _Global_.

    ![Figure 1: Select the Global site from the site selector.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards/images/01.png)
3.  Select _Message Boards_ from the _Content & Data_ menu.

    ![Figure 2: After changing to the global scope, select Message Boards from the Content & Data menu in Site Administration.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards/images/02.png)

Any changes made here apply to the global scope.

### [Site](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards#site) <a href="#site" id="site"></a>

By default, the _Message Boards_ app is scoped to the current selected Site. Message Boards scoped to a particular site are not visible when viewing Messages Boards that may be deployed on another site in the same instance.

### [Page](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards#page) <a href="#page" id="page"></a>

Creating page scoped message boards allows forum members to view different message boards on different pages housed on the same site. The example below has the same content on both site pages (_Forum 1_ and _Forum 2_.)

#### [Creating a Page Scoped Message Board](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards#creating-a-page-scoped-message-board) <a href="#creating-a-page-scoped-message-board" id="creating-a-page-scoped-message-board"></a>

To create a page scoped message board, follow these steps:

1. Navigate to the page that which will be scoped (_Forum 2_).
2. Click the _Options_ icon (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-widget-options.png)) in the widget’s title bar and select _Configuration_.
3. Click the _Scope_ tab.
4.  Select the page’s name or _\[YOUR-PAGE] (Create New)_ if the page scope doesn’t exist yet.

    ![Figure 6. page scoped content](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards/images/04.png)
5.  Click _Save_ then close the dialog window.

    ![Figure 7. Viewing page scoped content](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards/images/05.png)

The _Message Boards Categories_ and _Threads_ added on the _Forum 2_ page will be displayed only on that page.

#### [Managing Page Scoped Message Board](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards#managing-page-scoped-message-board) <a href="#managing-page-scoped-message-board" id="managing-page-scoped-message-board"></a>

Note that administrators must still use the _Site Administration_ menu to administer a page scoped Message Boards widget by setting the _Site Administration_ menu’s active scope. Follow these steps to do this:

1. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand the menu for your Site, then expand _Content & Data_.
2.  The current scope appears just below the _Content & Data_ heading. _Default Scope_ is the current Site. To change this, click the gear icon (![Gear](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-control-menu-gear.png)) and then select your desired scope. This changes the Site Administration menu to reflect scope you selected. To work in a page’s scope, for example, select that page from the gear icon. That page’s name then becomes the _Site Administration_ menu’s title.

    ![Figure 8: Select the page's scope under the Content & Data menu in Site Administration.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/scoping-your-message-boards/images/06.png)
3. Select _Message Boards_ from the _Content & Data_ menu. Any changes you make here apply to the scope that you selected in the previous step.

\
