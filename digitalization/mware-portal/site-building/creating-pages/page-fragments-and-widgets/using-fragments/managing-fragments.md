# Managing Fragments

Liferay DXP provides tools for managing fragments in the Liferay UI. With out-of-the-box tools, you can quickly edit fragment code, export/import fragment sets between sites, and more.

TIP

To share fragments and sets between sites, add them to the Global site or use the export/import actions.

You can use the Export/Import actions within the fragment sets and individual page fragments if you have a ZIP file. Alternatively, if you have a LAR file, you can use the Export/Import options in the Options menu located at the Applications Bar.

### [Managing Fragment Sets](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/managing-fragments#managing-fragment-sets) <a href="#managing-fragment-sets" id="managing-fragment-sets"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Design_ → _Fragments_.
2. Select the desired fragment set.
3.  Click the set’s _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)).

    ![Click the fragment set's Actions button to access management options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/managing-fragments/images/01.png)
4.  Select an action:

    **Edit**: Change the name or description for the fragment set.

    **Export**: Download the fragment set as a `.zip` file. This file includes the set’s `.json`, fragment code, and resources.

    **Import**: Upload fragments and resources to a set as a `.zip` file. If an imported fragment has invalid rules, it is saved as a draft. This import process works similarly to the [page template import process](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/exporting-and-importing-page-templates#importing-a-page-template).

    **Delete**: Remove the current fragment set and all its contents.

TIP

To export multiple sets, click the Fragment Sets _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Export_. Then, check the sets you want to export and click _Export_. Each set is downloaded as a separate `.zip` file.

### [Managing Individual Page Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/managing-fragments#managing-individual-page-fragments) <a href="#managing-individual-page-fragments" id="managing-individual-page-fragments"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Design_ → _Fragments_.
2. Select the desired fragment set.
3.  Click the fragment’s _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)).

    ![Click the fragment's Actions button to access fragment management options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/managing-fragments/images/02.png)
4. Select an action below.

**Edit**: Modify the fragment’s code or configuration.

**Change Thumbnail**: Change the fragment’s thumbnail image.

**Remove Thumbnail**: Remove the fragment’s thumbnail image and use the default fragment type icon.

**Rename**: Change the fragment’s name.

**Mark or Unmark as Cacheable**: Mark or unmark the fragment as cacheable. When a fragment is cached, it improves the performance of its pages.

**View Usages**: View the pages where the fragment is used. You can see the page’s name, type, version, and the time since the page’s last propagation. You can also select a page and propagate it.

**Export**: Download the fragment as a `.zip` file.

**Make a Copy**: Duplicate the fragment. Duplicated fragments share the same name with _(Copy)_ appended to the end.

**Move**: Move the fragment to a different fragment set.

**Delete**: Remove the fragment.

IMPORTANT

Users cannot edit or copy [saved fragment compositions](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions).

### [Copying Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/managing-fragments#copying-fragments) <a href="#copying-fragments" id="copying-fragments"></a>

For Liferay DXP 7.2 SP1+ and Liferay Portal 7.2 GA2+

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Design_ → _Fragments_.
2. Select the desired fragment set.
3.  Click the fragment’s _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Copy To_.

    ![Click the fragment's Actions button and select Copy To.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/managing-fragments/images/03.png)
4. Select the set where you want to copy the fragment.

TIP

Users cannot edit [default fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/default-fragments-reference) directly. However, you can copy default fragments into a custom set and edit them there.

\
