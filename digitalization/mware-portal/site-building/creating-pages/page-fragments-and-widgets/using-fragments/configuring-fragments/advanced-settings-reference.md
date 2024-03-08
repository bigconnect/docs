# Advanced Settings Reference

All Liferay’s default fragments include advanced settings except for the Collection Display fragment. Advanced settings can add custom CSS to a fragment, configure whether its contents are searchable, and more.

NOTE

Configuring a framgent’s advanced settings requires the Update - Advanced Options permission for the page.

To access a fragment’s advanced settings,

1. Begin editing a page or template that supports fragments.
2.  Open the _Browser_ side panel (![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png)), go to the _Page Elements_ tab, and select the desired fragment.

    Alternatively, click the desired fragment in the page or template.
3. Click the _Advanced_ tab in the Page Elements side menu.

![Access a fragment's advanced settings in the Page Elements side menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference/images/01.png)

### [Standard Advanced Settings](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference#standard-advanced-settings) <a href="#standard-advanced-settings" id="standard-advanced-settings"></a>

All default fragments with advanced settings have the following options.

#### [Hide from Site Search Results](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference#hide-from-site-search-results) <a href="#hide-from-site-search-results" id="hide-from-site-search-results"></a>

Specify whether to hide the fragment’s content from search results in your site. Hiding fragment content can reduce irrelevant and redundant search results. If the selected fragment contains child fragments, they inherit its configuration.

![Determine whether to hide the fragment's content from search results in your site.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference/images/02.png)

#### [Custom CSS](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference#custom-css) <a href="#custom-css" id="custom-css"></a>

Available 7.4 U34+/GA34+

Add CSS to fragments using the _Custom CSS_ text box. By default, this text box contains this placeholder:

```css
.[$FRAGMENT_CLASS$] {

}
```

`[$FRAGMENT_CLASS$]` is a CSS class token that represents the unique name of the fragment you are configuring. This CSS code is applied only to the current fragment. For example, you can set the fragment’s background to red normally and blue when the user hovers their cursor over it:

```css
.[$FRAGMENT_CLASS$] {
    background: red;
}

.[$FRAGMENT_CLASS$]:hover {
    background: blue
}
```

When the page renders, `[$FRAGMENT_CLASS$]` is replaced with the fragment’s class name (e.g., `lfr-layout-structure-item-01d752e4-32e3-ed4a-9cd6-fcaf483d0c34`).

TIP

Click the _Expand_ icon ( ![Expand](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-expand.png) ) to open a modal window and give yourself more room when editing the fragment’s CSS.

You can also use variables in your custom CSS that come from the [style book](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance) you are currently using. This example uses the Info color from a style book (defined as dark blue) to set a fragment’s background color:

```css
.[$FRAGMENT_CLASS$] {
    background: var(--info);
}
```

![You can use variables from your style book to fine-tune the CSS for specific fragments.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference/images/03.png)

#### [CSS Classes](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference#css-classes) <a href="#css-classes" id="css-classes"></a>

Use the _CSS Classes_ field to add classes for use in [Custom CSS](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference#custom-css). When adding multiple classes, separate each with a space, comma, or line break. You can use these classes for other fragments on the same page. Suggestions for existing classes appear while you type.

![Entering custom CSS classes in this field makes them appear as suggestions in the advanced settings for other fragments on the same page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference/images/04.png)

NOTE

Only CSS classes that you have added yourself appear as suggestions in the CSS Classes text box. Pre-existing classes or variables (such as `[$FRAGMENT_CLASS$]`) do not appear, and they do not need to be added to this field.

### [Advanced Container Settings](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference#advanced-container-settings) <a href="#advanced-container-settings" id="advanced-container-settings"></a>

Container fragments include [standard ones](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference#standard-advanced-settings) and advanced settings.

#### [HTML Tag](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference#html-tag) <a href="#html-tag" id="html-tag"></a>

Choose the HTML tag generated for the container fragment: `div`, `header`, `nav`, `section`, `article`, `main`, `aside`, or `footer`. These tags help web scrapers, browsers, and site renderers find and retrieve page content more easily. The selected tag only applies to the selected container itself and does not apply to nested fragments or content.

![You can choose which HTML tag is used for container fragments in the Advanced Settings tab.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference/images/05.png)

#### [Set Content Visibility to Auto](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference#set-content-visibility-to-auto) <a href="#set-content-visibility-to-auto" id="set-content-visibility-to-auto"></a>

By default, Liferay automatically renders all containers in a page. However, setting the container’s [`content-visibility`](https://drafts.csswg.org/css-contain/#propdef-content-visibility) property to `auto` causes elements to render only when needed. This can improve the page’s load performance.

![Improve page load performance by setting the container's content-visibility to auto.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/advanced-settings-reference/images/06.png)
