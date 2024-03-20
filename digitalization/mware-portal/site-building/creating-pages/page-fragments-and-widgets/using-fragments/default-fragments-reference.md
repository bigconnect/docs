# Default Fragments Reference

MWARE PORTAL provides a variety of fragments out-of-the-box, so you can start building your solution right away. To view available fragments, begin editing a content page or page template. While editing, open the _Fragments and Widgets_ tab ( ![Fragments and Widgets](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png) ) in the editing sidebar menu.

Fragments are organized into the following sets:

* Default Fragments Reference
  * Layout Elements
  * Basic Components
  * Form Components
  * Content Display
  * Account
  * Cart
  * Featured Content
    * Banners
    * Features
    * Highlights
  * Footers
  * Navigation Bars
  * Menu Display

NOTE

Available configuration options vary by fragment and fragment sub-element. See Configuring Fragments for more information.

### Layout Elements <a href="#layout-elements" id="layout-elements"></a>

Layout elements define drop zones for designing page and template layouts. By default, the portal includes two layout elements: _Container_ and _Grid_.

| Fragment                       | Description                                                                                               |
| ------------------------------ | --------------------------------------------------------------------------------------------------------- |
| Container (previously _Panel_) | Adds a configurable drop zone.                                                                            |
| Grid (previously _Row_)        | Adds multiple drop zone modules. These modules can be configured and arranged vertically or horizontally. |

### Basic Components <a href="#basic-components" id="basic-components"></a>

These are simple design elements that add basic functionality to a page. Examples include buttons, headings, and tabs. With these components, you can build your page piece by piece. The portal includes the following basic components.

| Fragment                                | Description                                                                                                                                                                                                                            |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Button                                  | Adds a link element. It can be a Link or an Action type, and you can apply five different button styles: Primary, Secondary, Link, Outline Primary, and Outline Secondary.                                                             |
| Card                                    | Adds image, rich text, and link elements arranged as a display card.                                                                                                                                                                   |
| Date                                    | Adds a field that you can map to custom date fields.                                                                                                                                                                                   |
| Drop-down 7.4+                          | Adds a simple text element and drop zone. You can drag-and-drop other fragments and widgets into the drop zone to create an advanced drop-down menu.                                                                                   |
| External Video 7.4+                     | Embeds an external video in a page or template. Supported video sources include YouTube, Facebook, Twitch, Vimeo, and videos stored in Documents and Media. See Embedding Videos Into Portal Assets and Pages for more information.    |
| HTML                                    | Adds an editable HTML element. Double-clicking the element opens a modal window where you can edit its content using HTML syntax.                                                                                                      |
| Heading                                 | Adds a simple text element that can be configured to use H1-H6 heading level.                                                                                                                                                          |
| Image                                   | Adds a configurable image element. When configuring, determine whether to use the image’s original size, or adjust it to fit its context.                                                                                              |
| Paragraph                               | Adds a rich text element. When editing, highlight the fragment’s text to access additional formatting options.                                                                                                                         |
| Separator                               | Adds a horizontal line between page elements. You can configure its spacing and color.                                                                                                                                                 |
| Slider                                  | Adds multiple images with rich text overlay that are grouped into slides. Users can manually cycle through each slide.                                                                                                                 |
| Social                                  | Adds image elements with Facebook, Twitter, and LinkedIn icons, which you can link to pages on each platform.                                                                                                                          |
| Spacer                                  | Adds a horizontal blank space between fragments on a page. You can configure its height.                                                                                                                                               |
| Tabs                                    | Adds multiple tabs with simple text elements and individual drop zones for adding widgets and other fragments.                                                                                                                         |
| Video URL (previously _Video Fragment_) | Embeds a video from YouTube or Documents and Media in a page or template. You can set the video to autoplay, loop, mute, or hide controls. You can also hide video controls from end users. See Embedding Videos for more information. |

### Form Components <a href="#form-components" id="form-components"></a>

Use form components fragments to build forms in content pages. These fragments only support custom objects. To enable and access them, you must have at least one published object. See Using Fragments to Build Forms for more information.

| Fragment         | Description                                                                                                                                                                                                                                                                                                                                                                        |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Form Container   | Adds a container for building forms in content pages. Once added, you must map the container to a custom object. The container automatically generates form fragments for each field in the object. You can add or remove fragments from the container and set a custom success message for form submissions. You can only place other form components in a mapped form container. |
| CAPTCHA          | Adds CAPTCHA confirmation to a form container. Once added, the form container requires human verification for submissions.                                                                                                                                                                                                                                                         |
| Checkbox         | Adds a checkbox to the form container that you can map to boolean fields.                                                                                                                                                                                                                                                                                                          |
| Date             | Adds a field that you can map to custom date fields.                                                                                                                                                                                                                                                                                                                               |
| File Upload      | Adds a file upload button that you can map to attachment fields. The mapped attachment field determines how files are uploaded. By default, this fragment displays supported file extensions.                                                                                                                                                                                      |
| Multiselect List | Adds a field that you can map to multiselect picklists.                                                                                                                                                                                                                                                                                                                            |
| Numeric Input    | Adds a field that you can map to custom numeric fields (i.e., integer, long integer, decimal, and precision decimal). You can set a placeholder value and help text.                                                                                                                                                                                                               |
| Rich Text Input  | Adds a field that you can map to rich text fields.                                                                                                                                                                                                                                                                                                                                 |
| Select from List | Adds a drop-down menu that you can map to custom picklist and relationship fields. If the mapped field includes more than 10 options, the user can search in the drop-down menu for querying options.                                                                                                                                                                              |
| Submit Button    | Adds a button for submitting the form entry. Every form container must include a submit button.                                                                                                                                                                                                                                                                                    |
| Text Input       | Adds a field that you can map to text fields (i.e., text, long text).                                                                                                                                                                                                                                                                                                              |
| Textarea         | Adds a field that you can map to long text fields.                                                                                                                                                                                                                                                                                                                                 |
| Tags             | Adds a field to add tags from the current site or global site to object entries.                                                                                                                                                                                                                                                                                                   |
| Categories       | Adds a field to add categories from the current site or global site to object entries.                                                                                                                                                                                                                                                                                             |

### Content Display <a href="#content-display" id="content-display"></a>

Use content display fragments to display site content individually or as part of a collection. You can also add page elements for rating or flagging page content. The portal includes the following content display fragments:

| Fragment                           | Description                                                                                                                                                                                                                                                                                                           |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Collection Display                 | Displays manually and dynamically defined collections, as well as collections defined by a collection provider. You can configure whether the collection is displayed in a grid, or with a list template (i.e., bordered, bulleted, inline, numbered, and unstyled). See Displaying Collections for more information. |
| Content Display                    | Displays a single content item from documents and media, web content, blogs, or categories. Available configuration options depend on the selected content type.                                                                                                                                                      |
| Applied Filters (Available 7.4+)   | Adds an element for displaying and managing filters applied to a collection display.                                                                                                                                                                                                                                  |
| Content Flags                      | Adds an element for flagging a content item.                                                                                                                                                                                                                                                                          |
| Collection Filter (Available 7.4+) | Adds an element for filtering a collection display using a vocabulary or category.                                                                                                                                                                                                                                    |
| Content Ratings                    | Adds an element for rating a content item.                                                                                                                                                                                                                                                                            |

```log
!!! note
    Collection Filter fragments used in Collection Display fragments only work if mapped to object collection providers or blueprint collection providers.
```

### Account <a href="#account" id="account"></a>

Use the account selector fragment to switch easily between your accounts when browsing products and creating orders. See Using the Account Selector Fragment for more information.

### Cart <a href="#cart" id="cart"></a>

Use the mini cart fragment to obtain a detailed overview of your cart. You can use it to adjust the product quantity, remove items, start the checkout process, or easily search for and add products to your cart. See Using the Mini Cart Fragment for more information.

### Featured Content <a href="#featured-content" id="featured-content"></a>

Featured content fragments are complex page elements composed of multiple components. To preview a fragment’s layout, switch to _Card View_ ( ![Card View](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-cards2.png) ) in the Fragments and Widgets panel. The portal includes the following featured content fragments.

NOTE

You can preview layouts by hovering over each fragment’s _Information_ icon (![Information](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png)).

#### Banners <a href="#banners" id="banners"></a>

Use banner fragments to add text and link elements to a content page for creating CTA (Call to Action) banners. The portal includes the following layouts.

![Add text and link elements to a content page for creating call to action banners.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/default-fragments-reference/images/01.png)

#### Features <a href="#features" id="features"></a>

Use the features fragment to add text, images, and link elements to a content page for promoting key features. The portal provides the following layout.

![Add text, image, and link elements to a content page for promoting key features](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/default-fragments-reference/images/02.png)

#### Highlights <a href="#highlights" id="highlights"></a>

Use the highlights fragment to add image, text, and link elements to a content page for highlighting. The portal provides the following layouts.

![Add image, text, and link elements to a content page for highlighting.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/default-fragments-reference/images/03.png)

### Footers <a href="#footers" id="footers"></a>

Use footer fragments to add a custom footer to master page templates. Each footer includes a navigation menu, sign-in link, image element, and copyright text. The portal provides the following layouts for footer fragments:

![Add a custom footer to master page templates for content page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/default-fragments-reference/images/04.png)

### Navigation Bars <a href="#navigation-bars" id="navigation-bars"></a>

Use navigation bar fragments to add a custom header to master page templates. Each navigation bar includes a navigation menu, sign-in link, and image element for your logo. The portal provides the following layouts for navigation bar fragments:

![Add a custom header to master page templates for content page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/default-fragments-reference/images/05.png)

### Menu Display <a href="#menu-display" id="menu-display"></a>

Using the menu display fragment, you can add menus to your content pages. Once deployed, you can select from any predefined navigation menu. To learn more about how to configure the menu display fragment for your site navigation, see Configuring the Menu Display Widget in Configuring Menu Displays.

You can also create a custom menu display fragment with specific markup and desired behavior by using the navigation menu selector configuration.

\
