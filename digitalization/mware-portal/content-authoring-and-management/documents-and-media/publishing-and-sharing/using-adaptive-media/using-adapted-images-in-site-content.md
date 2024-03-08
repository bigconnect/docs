# Using Adapted Images in Site Content

Adaptive Media works in the background creating different resolutions for the images you upload to Liferay DXP. When Blog, Web Content, and Content Page creators use these images, Adaptive Media chooses the best resolution for the target screen size automatically. Adaptive Media identifies each adapted image in the content’s HTML with a `data-fileentryid` attribute that is replaced with the latest adapted image when the user views the content. This lets Adaptive Media deliver the latest adapted images to your content, even if the content existed prior to those images.

NOTE

If you [disable Adaptive Media for an image](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions), the original image appears in the Blog entries, Web Content articles, and Content Pages.

### [Including Adapted Images in Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/using-adapted-images-in-site-content#including-adapted-images-in-content) <a href="#including-adapted-images-in-content" id="including-adapted-images-in-content"></a>

When [adding images to Content Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference#image-source-settings), Adaptive Media works for images you select directly and mapped images. When uploading images in a Blog or Web Content, Adaptive Media works only with images added from the _Blog Images_, _Documents and Media_, and _Upload_ tabs. Additionally, adapted images can only be applied to a blog entry’s content, not cover images. Adaptive Media works for images added to a blog entry via drag and drop, as the image is uploaded automatically to the Blog Images repository, adapted, and then included in the blog entry’s content. You can see this by inspecting the HTML and checking that the image contains the `<img>` tag and `data-fileentryid` attribute.

For web content articles, Adaptive Media works only with images added from the file selector’s _Documents and Media_ tab. Unlike blogs, Adaptive Media doesn’t deliver adapted images for images added to web content articles via drag and drop.

For both blog entries and media content articles, Adaptive Media doesn’t work with images added from the file selector’s _URL_ tab. This is because the image is linked directly from the URL and therefore provides no image file for Adaptive Media to copy.

Note that you can see the `<img>` tag and `data-fileentryid` attribute in the HTML of a blog entry or a web content article while you’re writing it. When the content is displayed, the HTML is automatically replaced and looks similar to this:

```html
    <picture>

        <source media="(max-width:850px)" srcset="/o/adaptive-media/image/44147/med/photo.jpeg">

        <source media="(max-width:1200px) and (min-width:850px)" srcset="/o/adaptive-media/image/44147/hd/photo.jpeg">

        <source media="(max-width:2000px) and (min-width:1200px)" srcset="/o/adaptive-media/image/44147/ultra-hd/photo.jpeg">

        <img src="/documents/20140/0/photo.jpeg/1992-9143-85d2-f72ec1ff77a0">

    </picture>
```

This example uses three different images, each with a different resolution. A `source` tag defines each of these images. Also note the original image (`img`) is used as a fallback in case the adapted images aren’t available.

### [Using Adapted Images in Structured Web Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/using-adapted-images-in-site-content#using-adapted-images-in-structured-web-content) <a href="#using-adapted-images-in-structured-web-content" id="using-adapted-images-in-structured-web-content"></a>

To use adapted images in [structured web content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/creating-structures), content creators must manually include an image field in the web content’s structure. Then they can reference that image field in the matching template by selecting it on the left side of the editor. Here’s an example snippet of an image field named `Imagecrrf` included in a template:

```markup
    <#if Imagecrrf.getData()?? && Imagecrrf.getData() !="">
      <img data-fileentryid="${Imagecrrf.getAttribute("fileEntryId")}" alt="${Imagecrrf.getAttribute("alt")}" src="${Imagecrrf.getData()}" />
    </#if>
```

This snippet includes the `data-fileentryid` attribute to ensure that Adaptive Media replaces the image with an adapted image. If you inspect the resulting web content’s HTML in the editor’s code view, you should see a tag like this:

```html
    <img data-fileentryid="37308" src="/documents/20143/0/photo.jpeg/85140258-1c9d-89b8-4e45-d79d5e262318?t=1518425" />
```

Note the `<img>` tag with a `data-fileentryid` attribute. Adaptive Media uses the file entry ID to replace the `<img>` element automatically with a `<picture>` element that contains the available adapted images for each resolution (see the `<picture>` example above).

### [Staging Adapted Images](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/using-adapted-images-in-site-content#staging-adapted-images) <a href="#staging-adapted-images" id="staging-adapted-images"></a>

Adaptive Media is integrated with Liferay’s [content staging](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management) and [export/import](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/exporting-importing-site-pages-and-content) features. Adapted images are included in staged content when published.

Similarly, when you export content with adapted images, those images are included in the LAR file. You can then import them along with the original image.

To improve performance, Adaptive Media reuses the existing adapted images during publishing and export/import.

\
