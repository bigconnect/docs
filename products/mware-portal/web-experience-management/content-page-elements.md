# Content Page Elements

Content Pages, like Widget Pages, are built by dragging and dropping elements onto the page and then configuring the way those elements appear. There are three kinds of elements:

**Sections** are fragments that define a space to place other elements. A section fills the entire width of the page. Sections can be thought of as _complete_ Fragments that serve a purpose by themselves. A large banner image with a text overlay is an example of something you might build as a section.

![Figure 1: A Section named Banner being displayed while editing a Content Page.](https://liferay-support.zendesk.com/hc/article\_attachments/360037575071/content-page-section-example.png)

**Layouts** are special Sections that define spaces where you can add fragments or widgets. Each layout you add fills the width of the page. You can add any number of layouts to the page.

![Figure 2: A 3 Column and 1 Column layout stacked on top of each other.](https://liferay-support.zendesk.com/hc/article\_attachments/360037575051/content-page-layout-example.png)

**Components** are small design elements or pieces that add functionality to the page. A component might be an image with formatting or a block of text with styling pre-applied. Components must be added to the page inside a Layout. If you add a component outside an existing Layout, a one column layout is automatically added to contain the Component. While Sections should be complete by themselves, Components work together to build pages piece by piece.

![Figure 3: Here are several of out of the box components arranged in the layout you saw previously.](https://liferay-support.zendesk.com/hc/article\_attachments/360037581012/content-page-component-example.png)

The portal ships with a plethora of Layouts, Sections, and Components to use to build pages, and a web developer can create their own Fragments to add to these.

### Editable Elements <a href="#editable-elements" id="editable-elements"></a>

Fragments can have editable elements. After a Fragment has been added to a page, you can click on an editable area to provide your own text, image, or links in place of the default defined in the Fragment.

You can also map these elements to content. You can set the _Content_ for the element (web content article, document, or blog) and choose its applicable _Field_ to display (e.g., title, author name, tags, etc.). You can configure this by selecting the element’s _Map_ button (![Map](https://liferay-support.zendesk.com/hc/article\_attachments/360040158632/icon-map.png)).

**Note:** You can also map custom fields.&#x20;

For more information on developing these elements, see Fragment Specific Tags.

Now you’ll learn about each editable type.

#### EDITABLE TEXT <a href="#editable-text" id="editable-text"></a>

Editable text can be plain or rich text. Plain text has no special styling. Rich text enables text styles, typographical emphasis, alignment, and list formatting.

![Figure 4: The rich text editor provides a simple WYSIWYG interface with a number of formatting options.](https://liferay-support.zendesk.com/hc/article\_attachments/360037581052/content-page-rich-text-editor.png)

#### EDITABLE IMAGES <a href="#editable-images" id="editable-images"></a>

Editable image elements allow replacing the image URL or an image from your Documents and Media library. You can provide a link target for the image.

To edit an image from the Content Page editor,

1. Click on the image you want to replace.
2. Click ![Image Properties](https://liferay-support.zendesk.com/hc/article\_attachments/360040158652/icon-edit.png).

![Figure 5: Editing an image allows you to enter a URL, select an image from Documents and Media or set a link for the image.](https://liferay-support.zendesk.com/hc/article\_attachments/360040163971/fragment-image-editor.png)

From here, you can click _Select_ to upload an image from Docs and Media or define an image URL. Click _Clear_ to reset the image. You can also specify an image description.

You can also specify a background image for a layout from Section Builder. Click the Layout, select _Layout Background Image_, and define the image to display.

For more information on developing editable images, see Making Images Editable.

You can also define a link for your image. You’ll learn about this next.

#### EDITABLE LINKS <a href="#editable-links" id="editable-links"></a>

Editable links can be associated with entities that redirect you to a content type or Page (e.g., buttons).

To edit a link from the content page editor,

1. Click on the link or button that you want to edit.
2. Click on ![Edit](https://liferay-support.zendesk.com/hc/article\_attachments/360040158652/icon-edit.png) to edit the link text.
3. Click on ![Link](https://liferay-support.zendesk.com/hc/article\_attachments/360040163951/icon-link.png) to edit the link properties.
4. Click on ![Map](https://liferay-support.zendesk.com/hc/article\_attachments/360040158632/icon-map.png) to edit the link mapping (described earlier).

From the Link Properties popup, you can define the following link options:

_Manual:_ defines a manual link or map it to an existing content field.

* _URL:_ sets the link’s URL.
* _Target:_ set the link’s behavior.

_From Content Field:_

* _Content:_ sets the content type.
* _Field:_ sets the field to display for the selected content.

Some of the content fields include

* Categories
* Tags
* Display Page URL
* Description
* Publish Date
* Summary
* Title
* Last Editor Name
* Author Name
* Basic Web Content

For more information on developing editable links, see Creating Editable Links.
