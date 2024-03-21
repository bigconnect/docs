# Adding Image Resolutions

By default, Adaptive Media includes two image resolutions: preview and thumbnail. You can define additional image resolutions to help optimize site performance for common devices (e.g., desktops, laptops, tablets, mobile phones). When you upload an image, Adaptive Media uses the existing resolutions to generate each version of the image automatically. All generated images maintain the original aspect ratio. If images are uploaded before you add custom resolutions, you must manually generate the resolution version for those images.

IMPORTANT

Generating additional resolutions requires more computational resources and storage space. To optimize performance, you can configure the max number of asynchronous processes and available core processes. See Adaptive Media Configuration Reference for more information.

![Add and manage images resolutions in the Adaptive Media application.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adding-image-resolutions/images/01.png)

### Adding a New Image Resolution <a href="#adding-a-new-image-resolution" id="adding-a-new-image-resolution"></a>

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Control Panel_ tab, and click _Adaptive Media_.
2. Click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ).
3. Enter a unique name and description.
4.  Enter a _Max Width_ and/or _Max Height_ in pixels.

    If you leave a measurement blank, its value is set to `auto`.
5.  Determine whether to add a resolution for high pixel density displays (HiDPI).

    Selecting this option creates an additional resolution that doubles the set size. This resolution uses the same name as the current resolution and adds the `-2x` suffix to it.
6.  Determine the resolution’s identifier:

    **Automatic** (Default): Uses a kebab-case value based on the name field.

    **Custom**: Enter a custom value.

    Third party applications can use this ID to obtain images for the resolution via Adaptive Media’s APIs.

    ![Enter a name, description, size, and identifier for the resolution.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adding-image-resolutions/images/02.png)
7. Click _Save_.

While the resolution is enabled, Adaptive Media uses it to generates images for all new uploads. You can also manually generate adapted images for previously uploaded images.

IMPORTANT

Image resolutions and their identifiers cannot be updated if the resolution has been used to adapt images. This prevents inconsistencies in generated images.

\
