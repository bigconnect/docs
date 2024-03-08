# Managing Image Resolutions

You can manage adapted image resolutions via the Adaptive Media application. This includes manually generating adapted images, disabling/enabling resolutions, as well as deleting resolutions with their images.

### [Editing Image Resolutions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions#editing-image-resolutions) <a href="#editing-image-resolutions" id="editing-image-resolutions"></a>

You cannot edit a resolution’s size and identifier if Adaptive Media has used it to generate adapted images. This prevents inconsistencies in generated images. If you must change the values of a resolution with adapted images, you must [delete](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions#deleting-image-resolutions) the resolution and [create](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adding-image-resolutions) a new one.

### [Manually Generating Adapted Images](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions#manually-generating-adapted-images) <a href="#manually-generating-adapted-images" id="manually-generating-adapted-images"></a>

If you’ve uploaded images before adding or enabling resolutions, you must manually generate adapted images for each one. The percentage of adapted images appears in the application table.

![The percentage of adapted images appears in the application table.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions/images/01.png)

To manually generate images for a resolution,

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Control Panel_ tab, and click _Adaptive Media_.
2.  Click the _Actions_ button ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the desired resolution and select _Adapt Remaining_.

    ![Select Adapt Remaining.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions/images/02.png)

    Alternatively, you can click the _Actions_ button ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) in the Application Bar and select _Adapt All Images_ to begin generating images for all enabled resolutions.

    ![Select Adapt All Images.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions/images/03.png)

Adaptive Media begins generating the images for each resolution asynchronously according to your [processing configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adaptive-media-configuration-reference#processes).

### [Disabling and Enabling Image Resolutions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions#disabling-and-enabling-image-resolutions) <a href="#disabling-and-enabling-image-resolutions" id="disabling-and-enabling-image-resolutions"></a>

Image resolutions are enabled by default, but you can disable them at any time. Disabling a resolution prevents Adaptive Media from using it to generate images, but images previously generated for the resolution remain available.

To disable/reenable a resolution,

1. Click the _Actions_ button ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the desired resolution.
2.  Select _Disable_/_Enable_.

    ![Select Disable/Enable.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions/images/04.png)

While disabled, Adaptive Media doesn’t use the resolution to generate adapted images.

If you upload images and later reenable a resolution, you must [manually generate](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions#manually-generating-adapted-images) adapted images for them.

### [Deleting Image Resolutions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions#deleting-image-resolutions) <a href="#deleting-image-resolutions" id="deleting-image-resolutions"></a>

If you need to delete an Image Resolution, you must first disable it. You _cannot_ delete enabled resolutions. This prevents the accidental deletion of image resolutions.

WARNING

Be careful when deleting image resolutions. Once deleted, the resolution’s adapted images are irretrievably lost and are not automatically replaced by new image resolutions you create.

To delete an image resolution,

1. Click the _Actions_ button ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for a disabled resolution.
2.  Select _Delete_.

    ![Select Delete.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions/images/05.png)

The resolution has been permanently removed along with its adapted images.

### [The Recycle Bin and Adapted Images](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/managing-image-resolutions#the-recycle-bin-and-adapted-images) <a href="#the-recycle-bin-and-adapted-images" id="the-recycle-bin-and-adapted-images"></a>

You can’t move adapted images directly to the [Recycle Bin](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/recycle-bin/recycle-bin-overview). But if the original image is in the Recycle Bin, the corresponding adapted images behave as if they are in the Recycle Bin and users can’t view them. URLs pointing to these adapted images return a `404` error code.

If the original image is restored from the Recycle Bin, the adapted images are accessible again.

\
