# Page Hierarchy

Using the Page Hierarchy, you create public and private pages and organize those pages in whatever order or structure that you see fit.

### Creating a Page <a href="#creating-a-page" id="creating-a-page"></a>

New pages are created on the _Site Builder_ → _Pages_ page in Site Administration. Pages can be created as _Public Pages_ which anyone can view, or _Private Pages_ which can only be viewed by Site Members. To create a new page,

1.  Go to _Site Administration_ for the Site you want to work on, then _Site Builder_ → _Pages_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030189232/default-nav-pages.png" alt="Figure 1: In the default site, initially only the Home and the hidden Search pages exist in the Public Pages Hierarchy." data-size="original">

    Figure 1: In the default site, initially only the \*Home\* and the hidden \*Search\* pages exist in the Public Pages Hierarchy.
2. Click _Add_ (![Add Page](https://liferay-support.zendesk.com/hc/article\_attachments/360030189172/icon-add.png)) and select _Public Page_.
3. Select _Widget Page_.
4. Set the _Name_ as _About Us_.
5. Click _Save_.

![Figure 2: When you create a page, by default it is added to the site hierarchy.](https://liferay-support.zendesk.com/hc/article\_attachments/360030157951/page-hierarchy-menu.png)

Now that the page is created, it appears in the hierarchy, and you can move or organize its position there.

### Organizing Pages <a href="#organizing-pages" id="organizing-pages"></a>

Drag and drop pages to reorder their position in the page hierarchy (and subsequently the default navigation that users see), and to nest them as subpages. The page at the top of the list is the _Home_ page that users see automatically when visiting your Site.

1. Click _Go to Site_ to view the organization of the navigation menu. You can see the order of the pages matches the order of the pages from Site Administration.
2. Drag the _About Us_ page above the _Welcome_ page in the list. It automatically becomes the _Home_ page.
3. Click _Go to Site_ to see how this affects your menu.
4. Drag _About Us_ on top of _Welcome_ to nest it.
5. Click _Go to Site_ one more time to see how nested pages appear.

![Figure 5: About Us is now nested under Welcome and appear when you mouse-over Welcome.](https://liferay-support.zendesk.com/hc/article\_attachments/360030189192/navigation-practical3.png)

As you’ve just demonstrated, organizing pages in the default menu is simple, but very powerful.

#### PUBLIC AND PRIVATE PAGES <a href="#public-and-private-pages" id="public-and-private-pages"></a>

As noted above, Private Pages work just like Public Pages, except they can be viewed only by registered members of a Site. In the default configuration, Public Pages are at the URL `[web-address]/`**web**`/[site-name]` while Private Pages are at `[web-address]/`**group**`/[site-name]`. Other than the membership distinctions, Public and Private Pages share the same behavior.

### Page Options <a href="#page-options" id="page-options"></a>

While managing the default menu, you can also access page options. Clicking on the _Options_ icon Option accesses several configuration tools:

**View** goes to the selected page on the Site.

**Configure** goes to page configuration.

**Copy Page** creates a new page in the current Site that duplicates the selected page.

**Permissions** opens the Permissions dialog.

**Orphan Widgets** clears data related to widgets that have been removed from the page.

**Delete** deletes the page and all its data.

Creating and managing pages using the page hierarchy is simple but very powerful. If you need more navigation options, however, Navigation Menus provide more flexibility.
