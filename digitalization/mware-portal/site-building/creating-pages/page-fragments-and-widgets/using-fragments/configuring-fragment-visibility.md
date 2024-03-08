# Configuring Fragment Visibility

[Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments) can be used to assemble [Content Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages) with a variety of layouts and information. Starting with Liferay DXP 7.4, you can configure the Fragments’ visibility based on a user’s screen size. For instance, a Fragment can be hidden for a page rendered from a computer while visible from the same Page on a smartphone.

You configure the Fragment visibility for each screen size from the [Content Page editor](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages/content-page-editor-ui-reference), using the viewport controls on the top of the Page editor.

![Configure the Fragment's visibility on your Content Page using the Viewports control.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility/images/01.png)

### [Understanding Fragment Visibility and Styles Inheritance](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility#understanding-fragment-visibility-and-styles-inheritance) <a href="#understanding-fragment-visibility-and-styles-inheritance" id="understanding-fragment-visibility-and-styles-inheritance"></a>

The Fragment’s visibility and styles you apply to a viewport on the left-hand side are applied to all the viewports on the right-hand side, unless you specify different styles for one of the right-hand side viewports. For example, if you hide a Fragment on the Desktop viewport, the Fragment is also hidden on all other viewports. This table summarizes this configuration:

| Viewport            | Visible? | Fragment Visibility Setting |
| ------------------- | -------- | --------------------------- |
| Desktop (A)         | No       | Hidden                      |
| Tablet (B)          | No       | Default / Inherited         |
| Landscape Phone (C) | No       | Default / Inherited         |
| Portrait Phone (D)  | No       | Default / Inherited         |

![Viewport on the right side inherit the configuration from viewports in the left side.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility/images/02.png)

From this setup, if you now make the Fragment visible on the Landscape Phone (for example), the Fragment stays visible on that viewport and the Portrait Phone viewport. This table summarizes the new configuration:

| Viewport            | Visible? | Fragment Visibility Setting |
| ------------------- | -------- | --------------------------- |
| Desktop (A)         | No       | Hidden                      |
| Tablet (B)          | No       | Default / Inherited         |
| Landscape Phone (C) | Yes      | Visible                     |
| Portrait Phone (D)  | Yes      | Default / Inherited         |

In this example, the Fragment is hidden on a computer, but visible when using a smartphone.

### [Content Page Editor Viewports Behavior](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility#content-page-editor-viewports-behavior) <a href="#content-page-editor-viewports-behavior" id="content-page-editor-viewports-behavior"></a>

This table summarizes each viewport’s behavior:

| Viewport        | Description                                                                                                                                                                                                   |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Desktop         | The default viewport. The Fragment styles and visibility you define here apply to all other viewports, unless you specify another style or visibility in another viewport.                                    |
| Tablet          | The viewport for tablet-size screens. The Fragment styles and visibility you define here apply to the phone viewports, unless you specify a style or visibility in the landscape or portrait phone viewports. |
| Landscape Phone | The Fragment styles and visibility you define on this viewport apply to portrait phone viewport as well, unless you specify another style or visibility in the portrait phone viewport.                       |
| Portrait Phone  | This viewport inherits the style and visibility changes you make in any other viewports, unless you specify another style or visibility here.                                                                 |

### [Changing Fragment Visibility](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility#changing-fragment-visibility) <a href="#changing-fragment-visibility" id="changing-fragment-visibility"></a>

You can change the Fragment visibility on your Content Pages in three different ways:

1.  From the Content Page editor sidebar, under the _Browser_ (![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-browser.png)) → _Page Elements_ area, click the _Hide_ (![Hide](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hide.png)) or _Unhide_ (![Unhide](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-preview.png)) control next to the Page Fragment.

    ![Change the Fragment's visibility from the Page Elements area in the Content Page editor sidebar.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility/images/03.gif)
2.  From the Content Page editor sidebar, under the _Browser_ (![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-browser.png)) → _Styles_ area, check or uncheck the _Hide Fragment_ box:

    ![Change the Fragment's visibility from the Browser → Styles area in the Content Page editor sidebar.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility/images/04.gif)
3.  From the Fragment’s Actions menu (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-widget-options.png)), select the _Hide Fragment_ (![Hide Fragment](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hide.png)) option:

    ![Change the Fragment's visibility from the Fragment's Actions menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility/images/05.gif)

TIP

To restore a hidden Fragment, from the Browser Page Elements area, click the _Unhide_ (![Unhide](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-preview.png)) control next to the Page Fragment.

### [Example: Using Fragment Visibility](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility#example-using-fragment-visibility) <a href="#example-using-fragment-visibility" id="example-using-fragment-visibility"></a>

Consider this example. Your insurance company wants to promote a new mobile app that lets customers manage their insurance policies. You want to promote this app when users access your Page from a smartphone. In this case, you can create a promotional banner for your new smartphone app and make this banner visible only on the Smartphone viewports:

1. From the default Desktop viewport, add a new Fragment with your promotional text for the smartphone app.
2. Hide the Fragment with your promotional text. The Fragment is now hidden in all viewports.
3. Click the _Landscape Phone_ viewport. Changes on this viewport affect the Portrait Phone viewport as well.
4. [Change the Fragment visibility](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility#changing-fragment-visibility) to unhide your promotional text in the Phone viewports.
5. Click on the different viewports to see the effect. The promotional text is now available in the Landscape Phone and Portrait Phone viewports only.

The following video illustrates this example:

![Changing a Fragment's visibility for a specific viewport in the Content Page editor.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility/images/06.gif)

TIP

To restore the default viewport visibility, click on the viewport and click the Restore Default (![Restore Default](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-restore.png)) button under the Fragment Styles area:

![Restore the default viewport Fragment visibility using the Restore Default button.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragment-visibility/images/07.png)

\
