# Navigation Menu Element Types

Navigation Menus have a variety of element types that you can add and arrange in your desired hierarchy. These types include [specific pages](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#pages), [Vocabularies](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#vocabularies), Display Pages for specific content types, or custom [placeholders](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#submenus) or [URLs](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#urls). See [here](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/using-the-navigation-menus-application) for more information on adding Navigation Menus with these elements.

### [Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#pages) <a href="#pages" id="pages"></a>

Specific pages are some of the most common elements used for Navigation Menus. You can add any published page from your Site’s Page Sets to the menu.

### [Submenus](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#submenus) <a href="#submenus" id="submenus"></a>

Submenus are placeholder elements you can add to Navigation Menus to group additional menu elements under them without, needing a specific page or content for it. When used on a page, submenu elements do not have clickable links, but expand to show their child elements when users hover over them.

![Submenu elements in the Navigation Menu are not clickable, but their contained child elements are.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/navigation-menu-element-types/images/01.png)

Using the default Application Display Template for Navigation Menus, if a submenu has no other elements organized under it, then it does not appear in the Navigation Menu for user.

### [Blogs Entries](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#blogs-entries) <a href="#blogs-entries" id="blogs-entries"></a>

You can add a [blog entry](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/adding-blog-entries) as an independent menu element that links to the entry’s [Display Page](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages). Using the default Application Display Template for Navigation Menus, if a blog entry without a Display Page is added, then it does not have a link, so it is hidden from users on the menu. This behavior can change if you use a different Application Display Template.

### [Web Content Articles](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#web-content-articles) <a href="#web-content-articles" id="web-content-articles"></a>

You can add a [web content article](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/adding-a-basic-web-content-article) as an independent menu element that links to the article’s [Display Page](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages). Using the default Application Display Template for Navigation Menus, if a web content article without a Display Page is added, then it does not have a link, so it is hidden from users on the menu. This behavior can change if you use a different Application Display Template.

### [Documents](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#documents) <a href="#documents" id="documents"></a>

You can add a [document](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media) as an independent menu element that links to the document’s [Display Page](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages). Using the default Application Display Template for Navigation Menus, if a document without a Display Page is added, then it does not have a link, so it is hidden from users on the menu. This behavior can change if you use a different Application Display Template.

### [Categories](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#categories) <a href="#categories" id="categories"></a>

You can add a [Category](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#defining-categories) as a menu element that links to the Category’s [Display Page](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/publishing-content-with-display-pages). Using the default Application Display Template for Navigation Menus, if a category without a Display Page is added, then it does not have a link, so it is hidden from users on the menu. This behavior can change if you use a different Application Display Template.

From Liferay 7.4 versions U9+ and GA13+, you can select multiple Categories at once. Adding a Category together with Subcategories under it places them in the Navigation Menu in the correct hierarchy.

NOTE

If a Category added as a Navigation Menu element contains Subcategories, changes to the Subcategories do not automatically reflect in the Navigation Menu’s hierarchy. If you want changes to the Subcategories to automatically update the Navigation Menu, then you can use a dynamic [Vocabulary](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#vocabularies) element instead.

### [Vocabularies](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#vocabularies) <a href="#vocabularies" id="vocabularies"></a>

Liferay Portal 7.4 GA26+, DXP 7.4 U26+

You can add entire [Vocabularies](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#defining-vocabularies) as menu elements. Vocabularies dynamically update so that all changes to the Categories within them and their hierarchies reflect in the Navigation Menu itself. However, this does not allow you to reorder or change the hierarchy of the contained Categories; if you want to create a different hierarchy of Categories, then [add the Categories individually outside of the Vocabulary](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#categories) instead.

By default, when you add a Vocabulary as a menu element, the menu replaces that element with all of its top-level Categories when it is displayed on a page. However, when you select the Vocabulary element while editing the Navigation Menu, you can also check the _Display Name as First Level_ checkbox to make the Vocabulary itself appear in the menu (as an unclickable element), with the top-level Categories under it.

![Check Display Name as First Level to make the Vocabulary appear as an element in the Navigation Menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/navigation-menu-element-types/images/02.png)

Using the default Application Display Template for Navigation Menus (which can change if you use a different Application Display Template), each top-level Category in the Vocabulary must have a configured Display Page for it to appear in the Navigation Menu. Categories shown in the menu still display all Subcategories, with or without their own Display Pages. However, Subcategories without Display Pages do not have clickable links and they do not display any of their own Subcategories under them.

### [URLs](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/navigation-menu-element-types#urls) <a href="#urls" id="urls"></a>

You can also add a Navigation Menu element with a custom URL, that does not need to be tied to any specific content. When you add a URL element to a Navigation Menu, you must input the display name of the menu element and the URL that the element points to on click. Check the _Open in a new tab_ checkbox if you want the URL to open in a separate tab in the user’s browser.

![To add a URL menu element, enter the name and the URL to point to.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/navigation-menu-element-types/images/03.png)

\
