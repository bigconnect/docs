# Saving Fragment Compositions

Layout and composition are the foundation of your Page design. When you get this right, your Pages are easier to read and navigate, resulting in a richer and more consistent user experience. Using the Container and Grid layout elements, you can define drop zones, create a complex composition, and then save your composition as a new Fragment. It can then be reused in other Pages and Templates that support Fragments.

TIP

When designing compositions, start with the Container Fragment. Add other elements into this Container for maximum flexibility and layout control.

Follow these steps to save a Fragment composition:

1. Begin editing the Page or Template that includes the composition you want to save.
2.  Click the desired _Container_ or _Grid_.

    Alternatively, open the _Browser_ panel (![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png)) in the sidebar menu and select the desired _Container_ or _Grid_.
3.  Click the Fragment’s _Options_ button (![Options Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) and select _Save Composition_.

    ![Open the Fragment's Options menu and select Save Composition.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/01.png)
4. Enter a Name for the Fragment.
5. (Optional) Add a _thumbnail_ and _description_ for the new Fragment.
6. Determine whether to save the composition’s _inline content_ (e.g., links, text, images)
7. Determine whether to save the composition’s [_mapping configuration_](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference#mapping-settings).
8.  Select the desired Fragment Set for the new Fragment.

    If no available Fragment Set exists, the composition is saved automatically to a new Set called Saved Fragments.

    ![Provide the information for the Fragment composition in the dialog that appears.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/02.png)
9. Click _Save_.

The saved Fragment composition appears in the _Fragments_ panel under the selected Fragment Set. You can add it to a Page or Template like other Fragments.

To view and manage saved compositions, open the _Site Menu_ and go to _Design_ → _Fragments_.

NOTE

In Liferay DXP 7.1 and 7.2, open the _Product Menu_, and go to _Site_ → _Site Builder_ → _Page Fragments_.

From here, you can rename saved compositions, change their thumbnail image, or export them as a `.zip` file, move them between Sets, or delete them. See [Managing Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/managing-fragments) for more information.

![The saved Fragment composition can be used immediately.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/03.png)

NOTE

The latest version of the [Liferay Fragments Toolkit](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragments-toolkit-command-reference) supports creation, export/import, and preview of Fragment compositions.

### [Example: Creating a Banner Composition](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#example-creating-a-banner-composition) <a href="#example-creating-a-banner-composition" id="example-creating-a-banner-composition"></a>

This step-by-step example shows you how to create a banner-style composition using nested Container Fragments. The example also illustrates how to save the composition and adjust its settings in the Style Book.

![Example of a sample Fragment composition using the Container Fragment](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/04.png)

* [Step 1: Creating a New Content Page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#step-1-creating-a-new-content-page).
* [Step 2: Configuring the Container Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#step-2-configuring-the-container-fragment).
* [Step 3: Saving the Fragment Composition](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#step-3-saving-the-fragment-composition).
* [Step 4: Setting the Default Container Width in the Style Book](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#step-4-setting-the-default-container-width-in-the-style-book).

#### [Step 1: Creating a New Content Page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#step-1-creating-a-new-content-page) <a href="#step-1-creating-a-new-content-page" id="step-1-creating-a-new-content-page"></a>

1. Under the Site Menu, go to _Site Builder_ → _Pages_.
2. Click _Add_ (![Add Page](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png)) at the level where you want to add the new Page and select _Add Page_.
3. Select the _Blank_ template or one of the existing templates.
4. In the _Add Page_ dialog, type your Page’s _Name_.

#### [Step 2: Configuring the Container Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#step-2-configuring-the-container-fragment) <a href="#step-2-configuring-the-container-fragment" id="step-2-configuring-the-container-fragment"></a>

1.  From the Content Page sidebar, select _Fragments and Widgets_ (![Fragments and Widgets](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png)) and drag and drop the _Container_ Fragment over the default drop-zone.

    ![Drag and drop the Container Fragment on top of the Content Page drop zone.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/05.png)
2. Click the Container Fragment and in the _Configuration_ panel, click the _Styles_ tab.
3. In the _Spacing_ panel, set the four Padding options to `5`.
4.  In the _Background_ panel, use the _Value from Style Book_ button to select a _Gray 200_.

    ![Define the Padding and Background Color value of the Container Fragment.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/06.png)
5. Select _Fragments and Widgets_ (![Fragments and Widgets](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png)) and drop a new Container Fragment over the existing one. This creates a nested Container composition that appears when you click on the _Browser_ (![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png)) button.
6.  From the Fragments and Widgets (![Fragments and Widgets](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png)) panel, under Basic Components, drag and drop a _Heading_ Fragment inside the inner Container in the composition.

    ![Drag and drop a Heading Fragment inside the inner Container Fragment.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/08.png)
7.  Drag and drop a _Paragraph_ Fragment right below the Heading. Select the Paragraph in the composition and under the _Styles_ section, choose a _Margin Top_ of `3`.

    ![Drag and drop a Paragraph Fragment right under the Heading Fragment.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/09.png)
8.  From the Selection (![Selection](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-pages-tree.png)) panel, click the inner Container. Under the _General_ tab, type the URL where you want to redirect users when they click on this area.

    ![Define a target link or Page for the inner Container Fragment.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/10.png)
9. From the Fragments and Widgets (![Fragments and Widgets](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png)) panel, under Basic Components, drag and drop a _Button_ Fragment in the composition, right below the paragraph.
10. Select the _Button_ and under the Styles section, set a _Margin Top_ of `3`. Under the _General_ section, click the _Type_ drop-down menu and choose _Link_.
11. In the Browser (![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png)) panel, click the _link_ for the Button Fragment. Under the Link section, type the URL you want to redirect users when they click the button.

    ![Configure the button Fragment type and top margin.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/11.png)

NOTE

You don’t need to Publish the Content Page. In-progress work on a Content Page is automatically saved as a draft.

#### [Step 3: Saving the Fragment Composition](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#step-3-saving-the-fragment-composition) <a href="#step-3-saving-the-fragment-composition" id="step-3-saving-the-fragment-composition"></a>

Follow the above steps to save the composition as a new fragment.

![Saving your Fragment Composition from the Container Option menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/12.png)

#### [Step 4: Setting the Default Container Width in the Style Book](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#step-4-setting-the-default-container-width-in-the-style-book) <a href="#step-4-setting-the-default-container-width-in-the-style-book" id="step-4-setting-the-default-container-width-in-the-style-book"></a>

By default, the Container Fragment uses a _Fluid_ width configuration. You can change the default setting to _Fixed Width_, and make this property available for other Content Pages based on the same Style Book. In this example with two nested Containers, you may want to configure the outer Container to adapt to the Page width while limiting the maximum width of Container with the text. To learn more about Style Books in Liferay DXP, read [Using a Style Book to Standardize Site Appearance](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance).

![You can change the Container Width property to use a fixed width or adapt to the Page width.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/14.gif)

WARNING

To avoid breaking your Site layout configuration, use a test environment when working with Style Books. Do not update a Style Book configuration for testing purposes on a production Site.

1. Click the _Browser_ (![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png)) button, then click the inner Container Fragment.
2.  In the General tab, set the _Container Width_ property to _Fixed Width_.

    ![Change the inner Container Fragment width setting to Fixed Width.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/15.png)
3.  Click the _Page Design Options_ (![Page Design Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-format.png)) and under the Style Book section, verify the active Style Book.

    ![Verify the default Style Book in use for the Content Page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/16.png)
4.  Under the Site Menu, go to _Design_ → _Style Books_.

    ![Open the Style Books page under the Site Menu and Design section.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/17.png)
5. Click the _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) menu for the Style Book corresponding to your Content Page and select _Edit_.
6.  On the Style Book editor, select your Content Page from the _Page Preview_ drop-down menu.

    ![Select the Content Page you want to modify from the Page Preview drop-down menu in the Style Book editor.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/18.png)
7.  Select the _Layout_ option on the right-hand side drop-down menu, and set a maximum width value (in pixels) for the fixed width elements in the _Extra Large_ option.

    ![Change the Extra Large option under the Style Book Layout setting to update the Container Fragment maximum width.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/19.png)
8. Click _Publish_ to update your Style Book with the new configuration.

### [Creating Advanced Compositions with the Container Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#creating-advanced-compositions-with-the-container-fragment) <a href="#creating-advanced-compositions-with-the-container-fragment" id="creating-advanced-compositions-with-the-container-fragment"></a>

> Available: Liferay DXP/Portal 7.4+.

When you configure one or more Containers inside another Container Fragment, you can define advanced display options using a _Flex_ display.

1. Under the Site Menu, go to _Site Builder_ → _Pages_.
2. [Create a new Content Page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/adding-a-page-to-a-site) or open an existing page with a Container-based composition. You can use [this example](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#example-creating-a-banner-composition) to create a basic composition using the Container Fragment.
3. Click the _Browser_ (![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png)) button.
4. Select the Container Fragment that groups the elements where you want to apply the advanced composition. For example, if you have a Container with Fragments in it, select the outer Container.
5. Under the Styles section, click the _Content Display_ drop-down menu and select one of the available _Flex_ options.
6.  Set the _Align Items_ and _Justify Content_ settings to your preference.

    ![Select the Container grouping the page elements you want to style using the Flex display options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions/images/20.png)
