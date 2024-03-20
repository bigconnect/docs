# Configuring SEO and Open Graph

Display page templates are custom layouts that display individual content items at a dedicated URL. These layouts use both fragments and widgets and can be created for web content articles, documents, blog entries, and more. Each template has its own SEO and Open Graph settings that can be filled manually or dynamically.

Follow these steps to configure SEO and Open Graph settings for a display page template:

1.  Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), navigate to _Design_ → _Page Templates_, and click on the _Display Page Templates_ tab.

    ![Go to the display page templates tab.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph/images/01.png)
2. Click _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired template and select _Edit_. When editing the template, click _Options_ (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) on the application bar and click _Configure_ (![Configure icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-cog.png)).
3.  Go to the SEO or Open Graph tab and enter the desired configuration.

    ![Enter the desired configuration in the SEO and Open Graph tabs.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph/images/02.png)
4.  Click _Save_.

    IMPORTANT

    Unsaved changes are lost when navigating between the SEO and Open Graph tabs.

When configuring SEO and Open Graph settings, you can enter values directly and use field mapping to add values dynamically. Available mapping options depend on the template’s content type and subtype. To map a field, click on the _field selector_ button (![Mapping button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-map.png)) and select from available fields. As of version 7.4, fields added this way are represented as `${}` placeholders with two parts: a field reference ID and field label (`${fieldReferenceID:Field Label}`).

For earlier versions, mapping placeholders only include the field reference ID (e.g., `${title}`, `${authorName}`, `${Text84981642}`).

The portal provides the field label value for clarity on the administrative side, since field reference IDs in custom web content and document structures are not human-readable (e.g., `${Text84981642:Contributors}`). The label is not displayed to end users and does not affect mapping, so users can edit it as desired. Mapping only depends on the placeholder’s field reference ID.

NOTE

The portal provides translations for mapped fields when possible. However, text provided outside of mapped fields is not translated.

### SEO Settings Reference <a href="#seo-settings-reference" id="seo-settings-reference"></a>

Search engine optimization (SEO) refers to the methods used to improve your page’s ranking in a search engine results page (SERP). With display page templates, you can configure your content’s display pages for SEO.

#### HTML Title <a href="#html-title" id="html-title"></a>

The HTML Title field defines a display page’s `<title>` tag. This title is used by search engines to rank your page and serves as the page’s heading in search engine results. By default, display page templates map this field to `${title}`. You can edit this value directly and use the field selector to include additional mappings (e.g., `${title} - ${authorName:Author Name}`).

The recommended length for an _HTML title_ is under 60 characters.

TIP

It is best practice to update the SEO and Open Graph titles together.

![Combine multiple mapped fields to build your own HTML title.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph/images/03.png)

#### Description <a href="#description" id="description"></a>

The Description field defines a display page’s description `<meta>` tag. This description is used by search engines to rank your page and appears in search engine results as a preview of your page. By default, display page templates map this field to `${description}`. You can edit this value directly and use the field selector to include additional mappings (e.g., `${description} - ${authorName:Author Name}`).

The recommended length for a page’s description is under 155 characters.

TIP

It is best practice to update the SEO and Open Graph descriptions together.

![Combine multiple mapped fields to create a custom HTML description.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph/images/04.png)

#### Robots <a href="#robots" id="robots"></a>

The Robots field configures `robots.txt` rules for a display page. These rules provide instructions to search engines and other tools crawling and indexing your site, defining what paths should or should not be crawled. Note that web crawlers may or may not obey instructions in `robots.txt`.

You can also localize this field using the Language Flag.

![The Robots field configures robots.txt rules for a display page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph/images/05.png)

#### Sitemap <a href="#sitemap" id="sitemap"></a>

Determine whether to include a display page in your `sitemap.xml` file, as well as set its Priority and Change Frequency fields. They inform search engines whether to crawl and index the display page, how it should be prioritized relative to other site pages, and how frequently it is updated.

![Determine whether to include a display page in your sitemap.xml file, as well as set its priority and change frequency.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph/images/06.png)

### Open Graph Settings Reference <a href="#open-graph-settings-reference" id="open-graph-settings-reference"></a>

Available: versions 7.3+

Open Graph is an internet protocol that standardizes previews of site content when shared in application contexts that support it, such as Facebook, Slack, and Twitter. It does this by embedding structured data in page headers as `<meta>` tags, similar to RDFa.

With MWARE Portal, you can create display page templates that dynamically configure a page’s Open Graph `<meta>` tags. Values defined here override default values defined at the site level.

NOTE

Open Graph `<meta>` tags are only included in page headers to unauthenticated users. They are not included when the user is logged in.

#### Title <a href="#title" id="title"></a>

The Title field defines a display page’s `og:title` property, which defines the title displayed for your content in rich previews. By default, this field is mapped to `${title}`. You can edit this value directly and use the field selector to include additional mappings (e.g., `${title} - ${authorName:Author Name}`).

TIP

It is best practice to update the SEO and Open Graph titles together.

![Combine multiple mapped fields to create a custom og:title value.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph/images/07.png)

#### Description <a href="#description-1" id="description-1"></a>

The Description field defines a display page’s `og:description` property which determines the description displayed for your content in rich previews. By default, this field is mapped to `${description}`. You can edit this value and use the field selector to include additional mappings (e.g., `${description} - ${authorName:Author Name}`).

TIP

It is best practice to update the SEO and Open Graph descriptions together.

![Combine multiple mapped fields to create a custom og:description value.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph/images/08.png)

#### Image <a href="#image" id="image"></a>

The Image field defines a display page’s `og:image` properties, which configures the image displayed for your content in rich previews. In addition to the basic image tag, DXP automatically adds a number of structured properties that determine how your selected image is displayed. The Image field defines the following `<meta>` tags.

```html
<meta property="og:image" content="http://example.com/ogp.jpg" />
<meta property="og:image:secure_url" content="https://secure.example.com/ogp.jpg" />
<meta property="og:image:type" content="image/jpeg" />
<meta property="og:image:width" content="400" />
<meta property="og:image:height" content="300" />
```

By default, this field is unmapped in display page templates. This means the template defaults to the image set at site level, unless you select a different image field.

![The image field defines a display page's og:image property.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph/images/09.png)

#### Image Alt Description <a href="#image-alt-description" id="image-alt-description"></a>

The Image Alt Description field defines a display page’s `og:image:alt` property, which determines the alt text read by screen readers for your displayed content’s `og:image` property.

```html
<meta property="og:image:alt" content="This is an example." />
```

By default, this field is unmapped in display page templates. This means the template defaults to the alt text set at the site level, unless you select a different text field.

![The image alt description field defines a display page's og:image:alt property](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph/images/10.png)

