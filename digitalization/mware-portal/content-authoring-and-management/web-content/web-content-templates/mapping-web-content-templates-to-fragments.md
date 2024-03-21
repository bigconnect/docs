# Mapping Web Content Templates to Fragments

By default, you can map web content structure fields holding a single value (such as date, numeric, image, number, or text) to fragments. For multi-value fields (such as select from list, radio button, or HTML) you must first render the field using a web content template; then you can map the template to a fragment.

For example, the _Web Content with Audience_ structure on this image contains a Content field of _Text_ type and an audience field of _Select from List_ type:

![You can map single-value structure fields to fragments by default.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-templates/mapping-web-content-templates-to-fragments/images/01.png)

You can map the single-value content field directly to a fragment, but you need a web content template to map the multi-value audience field. The web content template should use the same structure as the web content article containing the field you want to map. The following image shows this relationship:

![Create a web content template that uses the same structure as the web content article.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-templates/mapping-web-content-templates-to-fragments/images/02.png)

NOTE

If you want to map several structure field values to individual fragments, you must create a separate web content template that renders just one field for each field that uses that structure. If you render all your fields in one template, all their values map to the fragment when you select the template.

### Mapping a Web Content Template to a Fragment <a href="#mapping-a-web-content-template-to-a-fragment" id="mapping-a-web-content-template-to-a-fragment"></a>

1. Create a web content structure that contains the multi-value field you want to map.
2. Create a web content template that uses the same structure with the multi-value field (A).
3.  In the web content template, under fields (B), select the _variable_ (C) with the multi-value field in the structure.

    ![Create a web content template based on the structure with the multi-value field.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-templates/mapping-web-content-templates-to-fragments/images/03.png)
4.  Create the web content based on the previous web content template.

    ![Create the web content based on the template with the multi-value field.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-templates/mapping-web-content-templates-to-fragments/images/04.png)
5. On the fragment-supported page (content page, display page template, etc.) add the fragment.
6. On the sidebar panel, click the _Selection_ (![Selection](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-pages-tree.png)) button and choose the element in the fragment you want to map.
7. Under the Mapping section, _select and map_ the web content article that contains the value you want to map.
8.  Click the _Field_ drop-down menu and select the web content template you created before.

    ![Map the field in the web content template to your fragment.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-templates/mapping-web-content-templates-to-fragments/images/05.png)
9. Click _Publish_ to save the changes.

WARNING

A web content article is reindexed when it is edited and saved, but content pages are reindexed only when published (to conserve resources). Therefore, when a web content article mapped to a cached fragment is updated, the update doesn’t appear on the content page until you publish the page again or unmark the fragment as cacheable.

\
