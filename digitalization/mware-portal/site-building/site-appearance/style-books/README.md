# Style Books

A _Style Book_ is a set of visual rules that apply to a Site to provide a consistent experience across its pages. They determine various visual settings on the Site, including spacing between elements (such as widgets), colors, and fonts. Each of these settings applies equally to every page using the Style Book.

The theme you assign to your Site’s Pages determines the categories of settings available to the Style Books for your Site. Developers can customize the categories with new options for configuring Style Books. See Style Book Token Definitions for more information.

### Choosing a Style Book for a Page <a href="#choosing-a-style-book-for-a-page" id="choosing-a-style-book-for-a-page"></a>

If you do not choose a specific Style Book for a page, the page uses the Style Book for the Master Page. If neither is set, the page uses the default Style Book for the Site.

NOTE

From version 7.4+, every Site has at least one Style Book available called _Styles from Theme_. This Style Book uses the token definition defined in the theme, with all of the default values. This Style Book cannot be edited, and it is the default used for the Site if no other Style Book is configured.

Follow these steps to set any page’s Style Book:

1. Click _Edit_ (![Edit icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit.png)) near the top of the page.
2. Click _Page Design Options_ (![Page Design Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-format.png)) on the sidebar.
3.  Click to open the _Style Book_ tab.

    ![Open the Style Book tab to choose the desired Style Book from the list.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/01.png)
4. Select the desired Style Book from the list.
5. Click _Publish_.

The page is published using the new Style Book.

### Creating a New Style Book <a href="#creating-a-new-style-book" id="creating-a-new-style-book"></a>

1. Navigate to the _Site Menu_ → _Design_ → _Style Books_.
2. Click _Add_ (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) near the top of the page.
3. Enter a name for the Style Book and click _Save_.
4.  Change the options from the categories in the drop-down box in the menu on the right side of the screen.

    ![Select any of the available categories and customize your Style Book.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/02.png)

    These options come from the theme being used for your Site. See the default Classic theme categories for more information on the categories available for the default Classic theme.
5. Customize any of the provided fields given under the chosen category. Color fields specifically give you multiple ways to choose a color.
6. Click _Publish_.

The Style Book can now be selected as an option for any page when editing.

To make this Style Book the new default for your Site, open the Actions menu (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) beside the newly created Style Book and click _Mark as Default_.

#### Previewing a Style Book <a href="#previewing-a-style-book" id="previewing-a-style-book"></a>

Available 7.4 +

While you are creating or editing a Style Book, you can preview the way it affects the look of various components of your Site, including pages, Page Templates, and Page Fragments.

Use the two drop-down menus at the top of the screen to select what components to preview. The left drop-down menu determines the overall type to display (pages, Page Templates, Master Page Templates, Display Page Templates, or Page Fragments). The right drop-down menu determines which specific instance of the chosen type to display (or category, if applicable); only the four most recently used options are shown in the right drop-down menu, unless you click _More_ from the list.

![Use the two drop-down menus together to select what to display to preview your Style Book.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/03.png)

Previewing a Collection of Page Fragments shows all Fragments in the Collection. The preview displays Fragments with multiple (selectable) configuration options with an instance of each option, so that you can preview your Style Book with any of them.

![If you are previewing the Style Book's effect on Page Fragments, you can preview any configuration of the Fragments from the chosen Collection.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/04.png)

#### Customizing Your Style Book’s Colors <a href="#customizing-your-style-books-colors" id="customizing-your-style-books-colors"></a>

When you are choosing a color for any color field in your Style Book, you can choose the color in multiple ways:

* Click the color on the left side to open a color picker, and choose any color.
* Enter the hex code for the desired color into the text box to change to any color.
* Click the _Value from Stylebook_ button to open a menu of pre-defined colors to choose from. You can choose to reuse colors from each of the general categories that you can customize.

![You have several options available to change the color for any color option.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/05.png)

If you click the Value from Stylebook button and select a Style Book token value (that is also defined in another category), then that color field is linked to the chosen Style Book token. Any future changes to the token value for this Style Book will also change this color field.

When a color field is tied to a Style Book token, the Value from Stylebook button is replaced with a _Detach Token_ button. Click this button to detach the color field from the token (but keep the same color value).

![Click the Detach Token button to detach a color field from the Style Book token, but keep the same color value.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/06.png)

NOTE

Attaching or detaching a color value to a Style Book token requires version 7.4+.

Click the _Clear Selection_ button on the right side to reset the color back to the default value (defined in the theme).

NOTE

When you are editing colors from a Style Book on a specific page, you have the same options to choose a color. However, clicking the _Clear Selection_ button instead resets the color value to the default defined in the chosen _Style Book_.

#### Using the Color Picker <a href="#using-the-color-picker" id="using-the-color-picker"></a>

While customizing your style book, use the color picker to search for colors and filter your search results.

The use of the color picker is not restricted to style books. You can also find it in other parts of the portal (e.g., while editing a content page).

To access the color picker, click _Value from Style Book_ (![Value from Style Book icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-theme.png)) next to a color value (for backgrounds or borders, for example) or click on the color value when the icon is not available (e.g., text color).

![Access the color picker by clicking the Value from Style Book icon or by clicking on the color value when the icon is not available.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/10.png)

In the color picker, existing colors are divided by categories and token sets. You can see a color’s token value by hovering the cursor over the color.

![See a color's token value by hovering the cursor over the color in the color picker.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/11.png)

Use the search bar to search for a specific color. You may search by category, token set, token value, or hex color values.

When using the search bar, the search system is not case-sensitive. Also, if a search returns no results, a _No Results Found_ message is displayed in the search area.

![If a search returns no results, a No Results Found message is displayed in the search area.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/12.png)

### Default Classic Theme Categories <a href="#default-classic-theme-categories" id="default-classic-theme-categories"></a>

When you are editing a Style Book, the categories available from the drop-down box show different options for styling your pages. The options available in each category are defined by the theme.

TIP

You can place the `Style Guide Sample` widget on your pages to help test out the various options in your Style Book categories. This sample widget contains many different UI elements that leverage most of the features in the Classic theme, so you can use it to test your page styles.

The categories listed here are available as options for the Classic theme.

TIP

When modifying options like width, margins, and padding that aren’t related to colors, you can choose from a set of predefined units or specify a custom unit. Some options include pixels(PX), percentages(%), em width(EM), and root em(REM).

#### Color System <a href="#color-system" id="color-system"></a>

The colors used for fonts and visual elements defined in your theme are customizable in the Color System category. Generally, this means you can modify the body, background, and text colors on your pages.

![Change the colors used for displays or UI elements with options under the Color System category.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/07.png)

NOTE

Changing the colors for alert-related elements (such as _Success_ or _Warning_) may not affect the color of incoming alerts on your Site because each type of alert may use one of many colors. To fully customize the color of alerts, you must define customizable colors and use them in your own theme’s categories.

#### Spacing <a href="#spacing" id="spacing"></a>

Adjust the spacing between the main elements on your pages with the options in the Spacing category. When you configure a Page Fragment using the corresponding Style Book, these values are given as options for the Fragment’s margin and padding sizes.

![Change the colors used for displays or UI elements with options under the Color System category.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/08.png)

TIP

When modifying tokens like width, margins, and padding that aren’t related to colors, you can choose from a set of predefined units or specify a custom unit. This flexibility allows for more precise control over the styling. Some options include pixels(PX), percentages(%), em width(EM), and root em(REM).

#### General <a href="#general" id="general"></a>

These options modify color and spacing on elements that appear on every page of your Site. For example, the _Body Background_ option changes the background color of all pages.

#### Layout <a href="#layout" id="layout"></a>

These options modify the width of containers on your pages. For example, the default container on new Blank pages is configurable via the size of the Extra Large setting.

#### Typography <a href="#typography" id="typography"></a>

These options define new fonts for your pages. You can also adjust the size of headers via the options in this category.

#### Buttons <a href="#buttons" id="buttons"></a>

These options modify the style for the default types of buttons. You can customize the background, foreground, and border colors for each of the buttons used in the Classic theme.

![Change the colors under Button Primary to affect many common buttons for out-of-the-box widgets when using the Classic theme.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance/images/09.png)

\
