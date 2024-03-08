# Configuring Open Graph

[Open Graph](https://ogp.me/) is an Internet protocol that standardizes previews of Site content when shared in application contexts that support it, such as Facebook, Slack, and Twitter. It does this by embedding structured data in page headers as `<meta>` tags, similar to [RDFa](https://en.wikipedia.org/wiki/RDFa).

By default, Open Graph `<meta>` tags are embedded in all public Liferay Pages, but you can disable this behavior at the Site level if desired. In Site settings, you can also set default `og:image` properties. These values are used for all Pages, unless they’re overridden at the [Page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#open-graph) or [Display Page](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph) level.

NOTE

Open Graph `<meta>` tags are only included in page headers to unauthenticated users. They are not included when the user is logged in.

Follow these steps to configure Open Graph for your Site:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Configuration_ → _Settings_ (previously _Site Settings_).
2. Scroll down to the _Open Graph_ sub-heading and expand it.
3.  Check/uncheck the _Enable Open Graph_ setting to enable/disable Open Graph for your Site.

    ![Enable or disable embedded Open Graph tags in your Site's Pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/configuring-open-graph/images/01.png)
4.  (Optional) Use the _Image_ and _Image Alt Description_ fields to set default `og:image` properties for the Site’s Pages.

    These values are overridden by [Page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#open-graph) and [Display Page](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph) configurations.

    ![Define the default og:image and og:image:alt properties used for your Site.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/configuring-open-graph/images/02.png)
5.  Preview your default `og:image` configuration.

    Keep in mind the image’s ratio may change depending on where its shared.

    ![Preview your default Open Graph configuration.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/configuring-open-graph/images/03.png)
6. Click _Save_.

### [Analyzing the `og:image` Properties](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/configuring-open-graph#analyzing-the-ogimage-properties) <a href="#analyzing-the-ogimage-properties" id="analyzing-the-ogimage-properties"></a>

When you select an image, DXP automatically adds a number of structured properties that determine how your selected image is displayed. The Image field defines the following `<meta>` tags.

```html
<meta property="og:image" content="http://example.com/ogp.jpg" />
<meta property="og:image:secure_url" content="https://secure.example.com/ogp.jpg" />
<meta property="og:image:type" content="image/jpeg" />
<meta property="og:image:width" content="400" />
<meta property="og:image:height" content="300" />
```

When you enter text into the Image Alt Description field, you determine the default text read by screen readers for your selected image.

To create localized alt descriptions for your Open Graph image, click on the _Language Flag_ button and select the language you want to set. Values entered here define the default `og:image:alt` tag.

```html
<meta property="og:image:alt" content="This is an example." />
```
