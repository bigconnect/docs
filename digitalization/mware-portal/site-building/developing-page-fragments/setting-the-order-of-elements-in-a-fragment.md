# Setting the Order of Elements in a Fragment

The Slider or Tab fragments are typically used to show different areas of information. Sliders present this information in a carousel of slides. Tabs have multiple tabs, each one containing different information.

When configuring Slider or Tab Fragments in a Page or Template, their sub-elements are listed in the _Selection_ panel (![Selection](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-pages-tree.png)) in the sidebar menu. Before version 7.4, these elements were not presented in order. This made it difficult to identify how the Slider or Tab elements were grouped, especially for Fragments with many elements.

![Version 7.4+ shows the order of elements in a Fragment in order.](<../../../../.gitbook/assets/01 (2).png>)

Starting with version 7.4, the Slider and Tab Fragments show their elements in order. Additionally, version 7.4 includes the new `data-lfr-priority` HTML attribute to set the order of elements in a Fragment. Elements with a lower `data-lfr-priority` value have precedence in the Selection list, independently of their order in the HTML code.

### Customizing the Order of Elements in the Slider Fragment <a href="#customizing-the-order-of-elements-in-the-slider-fragment" id="customizing-the-order-of-elements-in-the-slider-fragment"></a>

1. Go to _Site Administration_ → _Design_ → _Fragments_.
2. Under _Fragment Sets_, click the Set with the Fragment you want to customize.
3.  Click on the Slider or Tab Fragment’s _Actions_ (![Action](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) button and select _Edit_ to open the Fragments Editor.

    ![Edit the imported Fragment to open the Fragment Editor.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/setting-the-order-of-elements-in-a-fragment/images/06.png)
4.  In the HTML code area, you can Edit the Slider Fragment code. Add the `data-lfr-priority` attribute to the Editable or Drop Zone areas. This example configures the carousel elements to use this order:

    1. `First Slide Title`, with `data-lfr-priority="1"`
    2. `First Slide Subtitle`, with `data-lfr-priority="2"`
    3. `First Responsive Image`, with `data-lfr-priority="3"`

    ```html
        <div class="carousel-inner text-break" id="${fragmentEntryLinkNamespace}-carouselInner" role="group">
            <div class="carousel-item active">
                <img
                    alt="First Responsive Image"
                    class="w-100"
                    data-lfr-editable-id="01-01-image"
                    data-lfr-editable-type="image"
                    data-lfr-priority="3"
                    src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAJCAYAAAA7KqwyAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAkSURBVHgB7cxBEQAACAIwtH8Pzw52kxD8OBZgNXsPQUOUwCIgAz0DHTyygaAAAAAASUVORK5CYII="
                />

                <div class="carousel-caption d-none d-md-block">
                    <h2
                        data-lfr-editable-id="01-02-title"
                        data-lfr-editable-type="rich-text"
                        data-lfr-priority="1"
                    >
                        First Slide Title
                    </h2>

                    <p
                        data-lfr-editable-id="01-03-subtitle"
                        data-lfr-editable-type="rich-text"
                        data-lfr-priority="2"
                    >
                        First Slide Subtitle
                    </p>
                </div>
            </div>
    ```

    This is the result you see in the Content Page Editor when you click the _Selection_ (![Selection](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-pages-tree.png) button:

    ![Elements in the Slider Fragment use the order you define in the HTML code.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/setting-the-order-of-elements-in-a-fragment/images/07.png)

    NOTE

    You don’t need to edit other parts of the Fragment’s code, like JavaScript, CSS, or configuration.

### Verifying the New Order of Elements in the Content Page Editor <a href="#verifying-the-new-order-of-elements-in-the-content-page-editor" id="verifying-the-new-order-of-elements-in-the-content-page-editor"></a>

1. Go to _Site Administration_ → _Site Builder_ → _Pages_.
2. Edit the Content Page where you want to add the Slider Fragment sample (or add a new Page).
3. From the Content Page sidebar, click _Fragments and Widgets_ (![Fragments and Widgets](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add-widget.png)).
4. Under the Fragments column, find the Slider or Tab Fragment you want to show.
5. Drag and drop the Fragment onto the editing area.
6.  In the Content Page Sidebar, click the _Selection_ (![Selection](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-pages-tree.png)) button to show the list of elements in your Fragment.

    NOTE

    Slider or Tab Fragments that you import from a version prior to version 7.4 appear in order in the Content Page Editor sidebar but don’t include the `data-lfr-priority` HTML attribute. You must edit the HTML Fragment code to include this attribute.

\
