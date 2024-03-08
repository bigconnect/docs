# Fragment Specific Tags and Attributes Reference

Along with standard HTML, CSS, and JavaScript, you can use Liferay-specific tags and attributes to define editable sections, embed widgets, and more.

Editable elements can be modified before publication, which means that you can create simple, reusable fragments that have identical formatting, but contain elements that are adaptable to the specific context.

These Liferay-specific tags and attributes are available:

* [Editable Text](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#making-text-editable)
* [Editable Images](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#making-images-editable)
* [Editable Links](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#creating-editable-links)
* [Editable HTML (Liferay Portal 7.3 GA3+ and DXP 7.3+)](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#creating-editable-html)
* [Embedded Widgets](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#including-widgets-within-a-fragment)
* [Localizable Fragment Fields](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#localizing-fragment-configurations)
* [Date Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#using-date-fragments)
* [Map Object Actions to Buttons](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#making-buttons-action-ready)

Since Liferay Portal 7.3 GA3 and Liferay DXP 7.3, you can use `data-lfr-editable*` attributes to define elements as editable, rather than wrapping editable elements with `lfr-editable` tags.

WARNING

Editable elements have a `data-lfr-editable-id` attribute, and it must be unique. Do not change the ID after the page fragment has been added to a page; you will lose changes made to the fragment.

The old tags still work for backwards compatibility, but you should use the newer data attributes if you’re running Portal 7.3 GA3+ or Liferay DXP 7.3+, as they are easier to write.

TIP

When you start typing the name of a tag, the [HTML editor](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor) provides auto-completion for `lfr` tags like editable elements and embeddable widgets.

This reference lists the available editable tags and attributes along with examples of how to use them in your fragments.

### [Editable Elements](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#editable-elements) <a href="#editable-elements" id="editable-elements"></a>

Create editable sections, dynamic and reusable fragments for building a site, or bring a fragment to life by including a widget embedded to a portlet using editable elements.

Provide text, images, links, and HTML code to these editable elements which become the default values for their fields. You may want to display the editable content in the final version of the page, or you may want filler content that should be replaced before publishing the page.

WARNING

Freemarker code in editable fields is executed only once when the fragment is added to the page (to set its default value).

Avoid using it if you need the value re-evaluated. For example, if you localize a value using `${languageUtil.get(locale,'word')}`, the word is only localized when you add the fragment to the page. The code does not run again even if you change your portal’s language.

Editable elements have many uses. Imagine you need a title, a small text box, an image, and a link to provide a product description. Create a fragment containing editable filler text, space for an editable image, the appropriate formatting, and an editable link.

You can then add the fragment to multiple pages and define the image, text, and link for each product you need to describe.

![You can use editable elements to create a fragment with an editable text, image, and link. This fragment can be edited and reused in different contexts.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference/images/01.png)

WARNING

Nesting editable elements inside other editable elements in a fragment is not supported.

### [Making Text Editable](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#making-text-editable) <a href="#making-text-editable" id="making-text-editable"></a>

You can make fragment text editable by including the `data-lfr-editable-type="text"` attribute in the image element. The `data-lfr-editable-id` must be a unique ID, like this example:

```html
<p data-lfr-editable-id="text1" data-lfr-editable-type="text">
  Placeholder
</p>
```

NOTE

All block elements and inline elements are supported for editable text.

For Liferay Portal 7.3 GA2 and below, use the syntax below. A unique ID is required to render the element properly:

```html
<lfr-editable id="unique-id" type="text">
   This is editable text!
</lfr-editable>
```

If you need formatting options like text or color styles, use `rich-text`:

```html
<p data-lfr-editable-id="text1" data-lfr-editable-type="rich-text">
  Placeholder
</p>
```

NOTE

All block element tags are supported for editable Rich text.

For Liferay Portal 7.3 GA2 and below, use the syntax below:

```html
<lfr-editable id="unique-id" type="rich-text">
   This is editable text that I can make bold or italic!
</lfr-editable>
```

NOTE

If you want to make text inside an HTML element editable, you must use the `rich-text` type. The `text` type strips HTML formatting out of the text before rendering.

### [Making Images Editable](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#making-images-editable) <a href="#making-images-editable" id="making-images-editable"></a>

Images use the same `data-lfr-editable-type` attribute as text, but with the `image` type. The `data-lfr-editable-id` must be a unique ID:

```html
<img
  src="placeholder.jpg"
  alt="Placeholder"
  data-lfr-editable-id="img1"
  data-lfr-editable-type="image"
>
```

For Liferay Portal 7.3 GA2 and below, use this syntax:

```html
<lfr-editable id="unique-id" type="image">
   <img src="...">
</lfr-editable>
```

After you add the `lfr-editable` tag with the type `image` to a fragment, when you add that fragment to a page, you can then click on the editable image and configure the image source and other properties from the content page editor sidebar.

![You have several options for defining an image on a content page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference/images/02.png)

Most images can be handled like this, but to add an editable background image you must add an additional property to set the background image ID, `data-lfr-background-image-id`. The background image ID is set in the main `div` for the fragment and is the same as your editable image ID.

```html
<div data-lfr-background-image-id="background-img-unique-id">
   <h1 data-lfr-editable-id="h1-unique-id" data-lfr-editable-type="text">
      Placeholder
   </h1>
   <p data-lfr-editable-id="p-unique-id" data-lfr-editable-type="text">
      Placeholder
   </p>
</div>
```

Content mapping connects editable fields in your fragment with fields from an asset type like web content or blogs. For example, you can map an image field to display a preview image for a web content article. For more information on mapping fields, see [Fragment Mapping Settings](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference#mapping-settings).

### [Creating Editable Links](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#creating-editable-links) <a href="#creating-editable-links" id="creating-editable-links"></a>

You can make general link elements editable by setting the `data-lfr-editable-type` to `link`. The `data-lfr-editable-id` must be a unique ID.

You can create editable link elements in different ways. Here are some examples:

```html
<a
  href="#placeholder"
  target="_blank"
  data-lfr-editable-id="link1"
  data-lfr-editable-type="link"
>
  Go to placeholder
</a>
```

Use `mailto:` URI scheme to associate the link with an email:

```html
<a
  href="mailto:email@liferay.com"
  target="_blank"
  data-lfr-editable-id="link-to-email"
  data-lfr-editable-type="link"
>
  Send a Message
</a>
```

Use `tel:` URI scheme to associate the link with a telephone number:

```html
<a
  href="tel:555-2368"
  target="_blank"
  data-lfr-editable-id="link-to-phone"
  data-lfr-editable-type="link"
>
  Who You Gonna Call?
</a>
```

#### [Creating Editable Links with Liferay Portal 7.3 GA2 and Below](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#creating-editable-links-with-liferay-portal-73-ga2-and-below) <a href="#creating-editable-links-with-liferay-portal-73-ga2-and-below" id="creating-editable-links-with-liferay-portal-73-ga2-and-below"></a>

For Liferay Portal 7.3 GA2 and below, use this syntax:

```html
<lfr-editable id="unique-id" type="link">
    <a href="default-target-url-goes-here">Link text goes here</a>
</lfr-editable>
```

You can edit the type of link, target URL, and link mapping from the content page editor sidebar.

![You have several options for defining a link's appearance and behavior.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference/images/03.png)

For more information on editable links, see [Editable Links](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference#link-settings).

### [Creating Editable HTML](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#creating-editable-html) <a href="#creating-editable-html" id="creating-editable-html"></a>

You can make general HTML elements editable as well by setting the `data-lfr-editable-type` attribute to `html`:

```html
<article data-lfr-editable-id="text1" data-lfr-editable-type="html">
  <h1>Placeholder</h1>
</article>
```

For Liferay Portal 7.3 GA2 and below, use this syntax:

```html
<lfr-editable type="html" id="text1">
  <h1>Placeholder</h1>
</lfr-editable>
```

### [Including Widgets Within A Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#including-widgets-within-a-fragment) <a href="#including-widgets-within-a-fragment" id="including-widgets-within-a-fragment"></a>

Each widget has a registered name and corresponding `lfr-widget-[name]` tag, which you must use to embed it in a fragment. For example, the Menu Display widget is registered as `nav`, so its tag is `<lfr-widget-nav />`. You can embed it in a block like this:

```html
<div class="nav-widget">
    <lfr-widget-nav>
    </lfr-widget-nav>
</div>
```

These are the widgets that can be embedded and their accompanying tags:

| Widget Name         | Tag                              |
| ------------------- | -------------------------------- |
| DDL Display         | `<lfr-widget-dynamic-data-list>` |
| Form                | `<lfr-widget-form>`              |
| Asset Publisher     | `<lfr-widget-asset-list>`        |
| Breadcrumb          | `<lfr-widget-breadcrumb>`        |
| Category Filter     | `<lfr-widget-categories-nav>`    |
| Flash               | `<lfr-widget-flash>`             |
| Media Gallery       | `<lfr-widget-media-gallery>`     |
| Menu Display        | `<lfr-widget-nav>`               |
| Polls Display       | `<lfr-widget-polls>`             |
| Related Assets      | `<lfr-widget-related-assets>`    |
| Site Map            | `<lfr-widget-site-map>`          |
| Tag Cloud           | `<lfr-widget-tag-cloud>`         |
| Tag Filter          | `<lfr-widget-tags-nav>`          |
| Web Content Display | `<lfr-widget-web-content>`       |
| RSS Publisher       | `<lfr-widget-rss>`               |
| Iframe              | `<lfr-widget-iframe>`            |

#### [Enabling Embedding for Your Widget](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#enabling-embedding-for-your-widget) <a href="#enabling-embedding-for-your-widget" id="enabling-embedding-for-your-widget"></a>

If you have a custom widget that you want to embed in a fragment, you can configure that widget to be embeddable. To embed your widget, it must be an OSGi Component. Inside the `@Component` annotation for the portlet class you want to embed, add this property:

```properties
com.liferay.fragment.processor.PortletRegistry
```

Also, set the `@Activate` and the `@Deactivate` life cycle methods to register and unregister an alias for the portlet using the `PortletRegistry`:

```java
public class MySamplePortlet extends MVCPortlet {

	@Activate
	protected void activate() {
		_portletRegistry.registerAlias(
			_ALIAS,
			MySamplePortletKeys.SAMPLE);
	}

	@Deactivate
	protected void deactivate() {
		_portletRegistry.unregisterAlias(_ALIAS);
	}

	private static final String _ALIAS = "sample";

	@Reference
	private PortletRegistry _portletRegistry;
}
```

When you deploy your widget, it’s available to add. The name you specify in the property must be appended to the `lfr-widget` tag like this:

```markup
<lfr-widget-app-name>
</lfr-widget-app-name>
```

> Available: Liferay DXP/Portal 7.4+ before U60

For previous versions, if you have a custom widget that you want to embed in a fragment, you can configure that widget to be embeddable. To embed your widget, it must be an OSGi component. Inside the `@Component` annotation for the portlet class you want to embed, add this property:

```properties
com.liferay.fragment.entry.processor.portlet.alias=app-name
```

When you deploy your widget, it’s available to add. The name you specify in the property must be appended to the `lfr-widget` tag like this:

```markup
<lfr-widget-app-name>
</lfr-widget-app-name>
```

NOTE

According to the W3C HTML standards, custom elements can’t be self-closing. Therefore, even though you can’t add anything between the opening and closing `<lfr-widget...>` tags, you can’t use the self-closing notation for the tag.

### [Localizing Fragment Configurations](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#localizing-fragment-configurations) <a href="#localizing-fragment-configurations" id="localizing-fragment-configurations"></a>

> Available: Liferay DXP/Portal 7.4+

You can localize fragment configuration for a page’s target language. For example, on a button fragment you can define one button type when the page language is en-US, and a different button type when the page language is es-ES. To localize a fragment configuration field, use the `localizable` attribute.

NOTE

The `localizable` attribute is not available for fragment configuration fields where the `configurationRole` property is set to `style`.

In the following code excerpt, the button fragment configuration sets the `localizable` attribute to `true` for the `fields` section under `fieldSets`. The `localizable` attribute is set at the field level. In the example, there is only one `buttonType` field. If you have a fragment with multiple fields, you can set the `localizable` attribute for each one:

```markup
"fieldSets": [
  {
    "fields": [
      {
        "dataType": "string",
        "defaultValue": "primary",
        "label": "type",
        "name": "buttonType",
        "type": "select",
        "localizable": true,
        "typeOptions": {
          "validValues": [
            {
              "value": "primary"
            },
            {
              "value": "secondary"
            },
            {
              "value": "link"
            },
            {
              "value": "outline-primary"
            },
            {
              "value": "outline-secondary"
            }
          ]
        }
      }
    ]
  }
]
```

You can use this sample code to change the button type depending on the page’s target language. In the following example, the _Contact Us_/_Contacto_ button fragment sets the `localizable` attribute to `true` for the `buttonType` field. The example uses this attribute to configure the _Primary_ button type when the page uses the en-US language (A) and the _Outline Primary_ type when the page uses es-ES (B).

![Localizable elements in the fragment show the flag icon under the General tab and support different configurations for different languages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference/images/04.png)

TIP

The flag icon under the fragment’s General settings indicates the configuration field as localizable.

Fragments with the `localizable` attribute that do not specify a custom configuration for a language use the default page language’s configuration.

### [Using Date Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#using-date-fragments) <a href="#using-date-fragments" id="using-date-fragments"></a>

Conforming date formats a locality can be a challenge in many contexts. You can customize and localize date formats using date fragments by including the `data-lfr-editable-id="date-time"` and `data-lfr-editable-type="date-time"` attributes in the fragment. `data-lfr-editable-id` must be a unique ID, like this example:

```html
<div data-lfr-editable-type="date-time" data-lfr-editable-id="date-time">
    02/03/11 00:00 AM
</div>
```

#### [Date Formats](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#date-formats) <a href="#date-formats" id="date-formats"></a>

You can choose the most common date formats out-of-the-box or customize your date format following [`SimpleDateFormat`](https://devdocs.io/openjdk\~8/java/text/simpledateformat).

There are four out-of-the-box options available:

| Date Format | How it looks |
| ----------- | ------------ |
| MM/DD/YY    | 08/07/23     |
| DD/MM/YY    | 07/08/23     |
| YY/MM/DD    | 23/08/07     |
| DD/MM/YYYY  | 07/08/2023   |

If you customize your date format, you can include different date and time patterns (e.g. era designator, time zone, and day name in the week).

Here are some examples:

| Date Format                | How it looks                        |
| -------------------------- | ----------------------------------- |
| MMMM dd, YYYY. hh:mm a     | August 07, 2023. 11:57 AM           |
| MM.dd.YY                   | 08.07.23                            |
| hh ‘o''clock’ a, zzzz      | 11 o’clock AM, Greenwhich Mean Time |
| KK:mm a, z                 | 11:57 AM, GMT                       |
| EEE, d MMM yyyy HH:mm:ss Z | Mon, 7 Aug 2023 11:57:00 +0000      |

TIP

You can localize the date format the same way you [localize fragment configuration fields](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#localizing-fragment-configurations).

### [Making Buttons Action-ready](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference#making-buttons-action-ready) <a href="#making-buttons-action-ready" id="making-buttons-action-ready"></a>

You can trigger [object actions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions) using button components by including the `data-lfr-editable-id="action"` and `data-lfr-editable-type="action"` attributes to the button tag. The `data-lfr-editable-id` must be a unique ID, like this example:

```html
<button class="btn btn-${configuration.buttonSize} btn-${configuration.buttonType}" data-lfr-editable-id="action" data-lfr-editable-type="action">
    Go Somewhere
</button>
```

\
