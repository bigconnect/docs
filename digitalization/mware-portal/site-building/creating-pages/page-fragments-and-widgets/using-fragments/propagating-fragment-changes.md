# Propagating Fragment Changes

By default, [changes](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor) made to a custom fragment’s code are not propagated to deployed instances of the fragment. Using Fragment Propagation, you can propagate changes to fragment instances in one of two ways:

* [Manually for individual instances of a fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/propagating-fragment-changes#manually-propagating-changes)
* [Automatically for all instances of a fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/propagating-fragment-changes#automatically-propagating-changes)

IMPORTANT

To avoid overwriting data, changes made to editable fields cannot be propagated to the fragment directly. However, you can force propagation by changing the fragment’s field ID. When propagated, the field’s content is no longer displayed, though you can retrieve its content from the database using the old field ID. To learn more, see [Fragment Specific Tags and Attributes Reference](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference) in the developer documentation.

### [Manually Propagating Changes](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/propagating-fragment-changes#manually-propagating-changes) <a href="#manually-propagating-changes" id="manually-propagating-changes"></a>

Follow these steps to manually propagate changes to individual instances of a fragment:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Design_ → _Fragments_.
2. Navigate to the _Fragment Set_ for the desired fragment.
3.  Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired fragment and select _View Usages_.

    ![Click the fragment's Actions button and select View Usages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/propagating-fragment-changes/images/01.png)

    TIP

    If the _View Usages_ option is unavailable, the fragment is not in use.
4.  In the Usages and Propagation page, check the fragment instances you want to update to the latest version.

    In the table, you can review whether a page or template is using a _Previous Version_ or the _Latest Version_.

    ![Select the fragment instances to update.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/propagating-fragment-changes/images/02.png)
5. Click the _Propagate_ button (![Propagate](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-propagate.png)) to update the selected instances of the fragment.

Changes made to non-editable fields are automatically propagated to the selected fragment instances.

### [Automatically Propagating Changes](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/propagating-fragment-changes#automatically-propagating-changes) <a href="#automatically-propagating-changes" id="automatically-propagating-changes"></a>

Follow these steps to automatically propagate changes to all instances of a fragment:

WARNING

Automatic propagation can lead to unintended consequences in the production environment, such as typos or inaccurate content being published. Use Automatic propagation only for testing or development purposes.

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_ tab, and go to _Configuration_ → _System Settings_.
2. Under the _Content and Data_ section, click _Page Fragments_.
3.  Check the _Propagate Fragment Changes Automatically_ box.

    ![Check the Propagate Fragment Changes Automatically box.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/propagating-fragment-changes/images/03.png)
4. Click _Save_.

When enabled, changes made to a fragment’s non-editable fields are automatically propagated to all instances of the fragment.

\
