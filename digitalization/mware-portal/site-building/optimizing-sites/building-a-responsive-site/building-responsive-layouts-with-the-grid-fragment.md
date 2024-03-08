# Building Responsive Layouts with the Grid Fragment

Liferay DXP [uses responsive design](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/building-a-responsive-site) by default, but there are situations where you want additional and more granular control of the content layout. By using the [Grid Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/using-layout-elements) in your Content Page, you gain precise control of the content layout for the different screen sizes in desktop and mobile devices. When you [edit a Content Page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages/adding-elements-to-content-pages), you can use the Grid Fragment (1) to outline your content and define customized layout styles (2) for the different target devices in the Device Display section (3). For example, you can customize the number of modules per row or the grid padding options for smartphone-size screens only.

![Using the Grid Fragment you can customize the layout options for different screen sizes.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/building-a-responsive-site/building-responsive-layouts-with-the-grid-fragment/images/04.png)

Consider the following example. The Services section of your insurance company website provides potential customers an overview of the services you offer. This section uses a series of Card Fragments inside a three-modules Grid Fragment, with information for three different services.

![You can customize your content layout combining the Grid Fragments with other Fragments.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/building-a-responsive-site/building-responsive-layouts-with-the-grid-fragment/images/01.png)

When you access this ‘Services’ Content Page from your computer, the grid layout shows the expected result. However, when you access the page from your smartphone, the result is not optimal because of the amount of text and the width of each Card Fragment. Using the Grid Fragment Styles, you can customize the number of cards to show on each module and modify styles for the best viewing experience on different screen sizes.

![The default grid layout style is not optimized for a smartphone-sized screen.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/building-a-responsive-site/building-responsive-layouts-with-the-grid-fragment/images/02.png)

### [Modifying the Layout of the Grid Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/building-a-responsive-site/building-responsive-layouts-with-the-grid-fragment#modifying-the-layout-of-the-grid-fragment) <a href="#modifying-the-layout-of-the-grid-fragment" id="modifying-the-layout-of-the-grid-fragment"></a>

1. [Configure the Grid Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/using-layout-elements) on the Content Page.
2. In the Editing sidebar, click the _Browser_ (![Simulation](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png)) button and click the Grid Fragment you want to customize.
3.  In the Device Display section of the Editing Toolbar, select the screen size you want to optimize.

    ![Select the screen size you want to customize in the Device Display section.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/building-a-responsive-site/building-responsive-layouts-with-the-grid-fragment/images/06.png)
4. In the Configuration Sidebar, under the _General_ tab, select the preferred layout options for the screen size. For example, one module per row works much better for the _Portrait Phone_ layout option.
5.  Select the image to configure its _Resolution_ for the target screen size (available in Liferay DXP 7.4+), or leave the default _Auto_ option to use [Adaptive Media](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media).

    ![Select the image resolution for the target screen size or leave the Auto option to use Adaptive Media.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/building-a-responsive-site/building-responsive-layouts-with-the-grid-fragment/images/05.png)
6. Click _Publish_.
7. Optionally, click the _Simulation_ (![Simulation](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-simulation.png)) button on the main toolbar to see your changes on the target device.

TIP

You can save and reuse the Grid Fragment configuration of your choice in other Content Pages. For more information, see [Saving Fragment Compositions](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions).

\
