# Using Display Page Templates

_Display page templates_ define structures for displaying content at a dedicated URL. When you create or upload content items, Liferay generates a unique friendly URL for accessing it. However, you can only use this URL to view the content if you first define a template for displaying it.

![Create display page templates for displaying content at a dedicated URL.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/images/01.png)

When creating a display page template, you begin by selecting a content type (e.g., web content article, document, blog entry). This determines which content items can use the template for rendering their display pages. You can then design the template using the same UI and design elements as content pages. This means you can use page [fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments) and [widgets](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages/using-widgets-on-a-content-page) to build dynamic displays that map to content fields and integrate additional functionality. Additionally, you can define [SEO settings](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph) to improve your pages’ search ranking.

After designing the template, you can make it the default template for displaying its content type or manually configure only some of your content items to use it. Once set, users can view the content item’s display page by visiting its friendly URL. The content is rendered dynamically according to your template design.

Display page templates currently support the following content types:

* [Blog Entries](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/blogs/getting-started-with-blogs)
* [Categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content) 7.4+
* [Custom Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/displaying-object-entries) 7.4+
* [Documents](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/publishing-documents)
* [Web Content Articles](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/adding-a-basic-web-content-article)

TIP

Use display page templates for designing layouts that you want to reuse for multiple content items.

These templates are ideal for displaying a list of elements in a consistent way, like business case studies, product or service options, job listings, web articles, and more. For example, on an insurance business website, you may want to list the available options. Individual web content articles describe each option. You want a landing page where site visitors can choose an option to see the corresponding insurance details from the articles.

![You can apply the same layout to different content of the same type using a single display page template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/images/02.png)

To achieve this goal, you can create a single display page template that maps your web content article fields to fragments in your template. Each article can use the same template. Then, you can link each insurance option in your landing page to the corresponding article’s friendly URL. When site visitors click an option, the display page is populated dynamically with information from your web content articles and rendered according to your template design.

If a content type has multiple display page templates created for it, you can [map links to any of the available templates](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/using-multiple-display-page-templates-to-create-multi-step-applications) for that asset type. When mapping a link to its display page, you can choose the default option or any of the other available display page templates, allowing for greater customization and flexibility in how your content is displayed.
