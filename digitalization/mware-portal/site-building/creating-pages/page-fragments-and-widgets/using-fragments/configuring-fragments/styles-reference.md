# Styles Reference

All fragments have a Styles settings section except for the Collection Display fragment. Use the style settings to configure your fragments’ look and feel. Many of these options correspond to CSS classes and have defined tokens supported by style books. See General Settings Reference and Fragment Sub-Elements Reference for additional fragment configuration options.

NOTE

By default, style configurations apply to the entire fragment, so all elements in the fragment inherit these changes if their CSS allows it; otherwise, the inner elements are ignored. You can configure custom fragments to apply style changes only to some inner elements. See Applying Styles to Fragments for more information.

To access a fragment’s Styles menu,

1. Begin editing a page or template that supports fragments.
2.  Open the _Browser_ panel (![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png)) in the sidebar menu and select the desired fragment.

    Alternatively, click the desired fragment in the page or template.
3. Click the _Styles_ tab in the Page Elements side menu.

![Access a fragment's Styles settings in the Browser panel of the editing sidebar menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/styles-reference/images/01.png)

Many styles have defined tokens supported by style books. The value of tokens can be inherited from style books, which is indicated by a blue dot or _value from stylebook_ icon (![Value from Stylebook](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-theme.png)) next to the displayed value. See Creating a Style Book and Style Book Token Definitions for more information.

![Visual representation of inherited token values with blue dots](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/styles-reference/images/07.png)

NOTE

You can configure fragment styles for individual viewports by clicking the device buttons at the top of the screen or setting up adaptive media for background images. For earlier versions, you can only configure text and spacing styles for different devices.

### Spacing <a href="#spacing" id="spacing"></a>

Click the button for the spacing dimension to configure. The inner buttons determine the fragment’s padding, while the outer buttons determine its margins. In this context, _padding_ refers to the space between a fragment’s frame and its content, while _margin_ refers to the space between the fragment’s frame and the end of its surrounding context.

![When you click any of the regions, choose the desired spacing from the list of sizes shown.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/styles-reference/images/02.png)

Clicking the padding or margin buttons lets you select from existing tokens or enter a custom value. The predefined tokens come from the numbered Spacer fields in the style book’s Spacing category. If you enter a custom value, select its unit (e.g., `px`, `rem`). To restore the field to its initial value, click the _Reset_ button ( ![Reset](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-restore.png) ).

NOTE

For 7.4 and earlier versions, if your style book uses a unit other than pixel to define spacing values (e.g., `rem`), they are converted to pixels during fragment configuration.

### Text <a href="#text" id="text"></a>

Format the fragment’s text.

| Field                                                                                                                                                                                                                                                                                                                                                                                                                            | Description                                                                                                                                                            |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Font Family ( ![Font Family](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-font-family.png) )                                                                                                                                                                                                                                                                                                             | Specifies the font for an element.                                                                                                                                     |
| Font Weight ( ![Font Weight](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-font-weight.png) )                                                                                                                                                                                                                                                                                                             | Sets how thick or thin characters in text should be displayed.                                                                                                         |
| Font Size ( ![Font Size](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-font-size.png) )                                                                                                                                                                                                                                                                                                                   | Sets the size of a font.                                                                                                                                               |
| Text Color ( ![Font Color](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-color-circle.png) )                                                                                                                                                                                                                                                                                                              | Choose a color for the text. The available color palette can be configured by the fragment developer. See Fragment Configuration Types Reference for more information. |
| Text Align ( ![Align Left](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-align-left.png) ![Align Center](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-align-center.png) ![Align Right](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-align-right.png) ![Justify](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-align-justify.png) ) | Specifies the horizontal alignment of text in an element. Choose from _Left_, _Center_, _Right_, or _Justify_.                                                         |

![Format the fragment's text.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/styles-reference/images/03.png)

### Background <a href="#background" id="background"></a>

Configure the fragment’s background.

| Field                                                                                                                           | Description                                                                                                                                                                                                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Background Color ( ![Background Color](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-color-circle.png) ) | Choose a background color from the palette. The available color palette can be configured by the fragment developer. See Fragment Configuration Types Reference for more information.                                                                                                                        |
| Image Source                                                                                                                    | If set to _Direct_, users can upload a background image. If set to _Mapping_, users can choose from Documents and Media, Web Content, Blogs, or Categories.                                                                                                                                                  |
| Background Image (_Direct_)                                                                                                     | Select an image from your library or enter an image URL.                                                                                                                                                                                                                                                     |
| Item (_Mapping_)                                                                                                                | Map the background to a specific item from Documents and Media, Web Content, Blogs, or Categories. Once selected, click the _Actions_ button (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) to edit the item and its permissions, or view its usages. |
| Field (_Mapping_)                                                                                                               | Determine which of the item’s fields is used for the background image.                                                                                                                                                                                                                                       |

![Configure the fragment's background.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/styles-reference/images/04.png)

NOTE

When mapping the background to an item’s field, you can only select from fields in its structure. For example, if you’ve selected a Web Content item, you can only select from image fields in its structure and not from images in it’s content field.

### Borders <a href="#borders" id="borders"></a>

Determine the width, radius, and color of the fragment’s border.

| Field                                                                                                                      | Description                                                                                                                                                                                                                                                                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Border Width ( ![Border Width](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-border-width.png) )    | Sets the width of an element’s four borders. Click the _Units_ button ( ![Units](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-code.png) ) to select border width unit (e.g., `px`, `rem`). Click the _Reset_ button ( ![Reset](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-restore.png) ) to revert the field to its initial value. |
| Border Radius ( ![Border Radius](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-corner-radius.png) ) | Defines the radius of the element’s corners and allows you to add rounded corners.                                                                                                                                                                                                                                                                                                   |
| Border Color ( ![Border Color](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-color-circle.png) )    | Sets the color of an element’s four borders. The available color palette can be configured by the fragment developer. See Fragment Configuration Types Reference for more information.                                                                                                                                                                                               |

![Determine the width, radius, and color of the fragment's border.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/styles-reference/images/05.png)

### Effects <a href="#effects" id="effects"></a>

Set the fragment’s opacity and determine whether it has a shadow.

| Field                                                                                                 | Description                                                                 |
| ----------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| Opacity                                                                                               | Sets the transparency level; 100 is default and 0 is transparent.           |
| Shadow ( ![Shadow](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-shadow.png) ) | Adds a shadow around the box. Choose from a small, medium, or large shadow. |

![Set the fragment's opacity and determine whether it has a shadow.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/styles-reference/images/06.png)

For 7.4 and earlier versions, spacing configurations are divided into the Margin and Padding sections.

#### Margin <a href="#margin" id="margin"></a>

Configure the space between a fragment’s frame and the end of its surrounding context (e.g., between the frame and the edge of the page, or between the frame and the edge of its Container or Grid).

| Field         | Description                           |
| ------------- | ------------------------------------- |
| Margin Top    | Sets the top margin of an element.    |
| Margin Bottom | Sets the bottom margin of an element. |
| Margin Left   | Sets the left margin of an element.   |
| Margin Right  | Sets the right margin of an element.  |

#### Padding <a href="#padding" id="padding"></a>

Configure the space between a fragment’s frame and its content.

| Field          | Description                                 |
| -------------- | ------------------------------------------- |
| Padding Top    | Sets the top padding (space) of an element. |
| Padding Bottom | Sets the bottom padding of an element.      |
| Padding Left   | Sets the left padding of an element.        |
| Padding Right  | Sets the right padding of an element.       |
