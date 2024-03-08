# Using Adaptive Media

and Media, Blogs, Web Content, and Content Pages to serve the optimal image resolution for the end user’s device. This reduces page size, improves load times, and saves bandwidth.

By default, Adaptive Media includes preview (1000px auto) and thumbnail (300px/300px) resolutions, but you can [define additional resolutions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adding-image-resolutions). Adaptive Media then listens for when users upload [supported media types](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adaptive-media-configuration-reference#images) and begins parallel, asynchronous processes for generating the alternative image sizes. The new images are linked to the original image and scaled to fit your set resolutions while maintaining the same aspect ratio.

NOTE

Adaptive Media supports these Commerce widgets: [Search Results](https://learn.liferay.com/commerce/latest/en/creating-store-content/commerce-storefront-pages/search.html), [Product Details, Product Publisher](https://learn.liferay.com/commerce/latest/en/creating-store-content/commerce-storefront-pages/product-details.html), [Mini Cart](https://learn.liferay.com/commerce/latest/en/creating-store-content/liferay-commerce-widgets/using-the-mini-cart-widget.html) and component, and [Checkout](https://learn.liferay.com/commerce/latest/en/creating-store-content/commerce-storefront-pages/checkout.html).

If you have upgraded to the latest DXP version from a legacy version that did not support Adaptive Media, you can migrate existing Documents and Media thumbnails to new Adaptive Media defined Image Resolutions; see [Migrating Documents and Media Thumbnails](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/migrating-documents-and-media-thumbnails).
