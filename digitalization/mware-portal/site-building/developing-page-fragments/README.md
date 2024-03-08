# Developing Page Fragments

Page Fragments are the building blocks for [Content Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/understanding-pages/understanding-pages#content-pages). They’re made from the three components of web pages: CSS, HTML, and JavaScript. To create a page you combine multiple fragments, and together they create the design for the page and its functionality. Fragments can make up a larger page element (such as cards or paragraph elements), or they can stand alone on their own (such as a banner).

![Fragments are combined like puzzle pieces to build a Content Page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/developing-fragments-intro/images/01.png)

NOTE

For Liferay DXP 7.4+, Fragment Collections are called Fragment Sets in the Liferay UI.

### [Fragment Sets](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/developing-fragments-intro#fragment-sets) <a href="#fragment-sets" id="fragment-sets"></a>

Sets organize Fragments so that groups of related Fragments can be managed and shared. Users navigate Sets when selecting Fragments to add to a page, so you’ll create Fragments in Sets that make it easy to combine into Pages. An example appears in the admin page, which shows all the out-of-the-box Fragments (and their code). A Page Fragment’s code can be updated through the admin interface. If you want your Page Fragment’s code instead to be static (locked and unable to be edited through the interface), you can create a [Contributed Fragment Set](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/creating-a-contributed-fragment-set).

### [Tooling](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/developing-fragments-intro#tooling) <a href="#tooling" id="tooling"></a>

Browser-based tools help you create Fragments directly on your system, and CLI-based tools integrate with your own toolset to preserve your productivity:

* [The Built-in Fragments Editor](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor): use the built-in editor to develop Page Fragments inside of Liferay Portal.
* [The Fragments Toolkit](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/using-the-fragments-toolkit): use the Fragments CLI to generate, import, and export Page Fragments and Fragment Sets from your desktop.

### [Features](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/developing-fragments-intro#features) <a href="#features" id="features"></a>

Fragments can be enhanced using the below resources, making them much more powerful than plain HTML, CSS, and JavaScript:

* [Editable Elements](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference): make text, images, links, and “rich” text elements editable, so users can replace the content with custom text and images. This is the key feature of Fragments that makes them reusable. You can also embed widgets.
* [Drop Zones](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/defining-fragment-drop-zones): Add drop zones to Fragments using the `<lfr-drop-zone></lfr-drop-zone>` label. Once added, you can drag and drop Fragments and widgets into the defined area.
* [Configuration Options](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments) : Add configuration options to the application’s Configuration Menu, like modifying the Fragment’s font color.
* [FreeMarker](https://freemarker.apache.org/): FreeMarker is a templating language that enhances HTML with variables, conditional statements, and more. From your HTML, you can use the [alternative (square bracket) syntax](https://freemarker.apache.org/docs/dgui\_misc\_alternativesyntax.html) for FreeMarker.

IMPORTANT

Grant permission to Create or Edit templates in FreeMarker to trusted users only. See [Assigning Permissions to Web Content Structures and Templates](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/assigning-permissions-to-structures-and-templates#security-considerations-for-web-content-templates) for information about Web Content Templates security.

### [Best Practices](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/developing-fragments-intro#best-practices) <a href="#best-practices" id="best-practices"></a>

We recommend these best practices when writing your Fragment code:

* Make your code semantic and highly reusable.
* Namespace your Fragments properly so they don’t interfere with other elements on the page outside of the Fragment.
* Use the autogenerated Fragment class as the basis for all CSS selectors you add to avoid impacting other Fragments.
* Use JavaScript sparingly, since it isn’t easily reusable. Instead, reference external JavaScript libraries.
* Escape Fragment configuration text values.

\
