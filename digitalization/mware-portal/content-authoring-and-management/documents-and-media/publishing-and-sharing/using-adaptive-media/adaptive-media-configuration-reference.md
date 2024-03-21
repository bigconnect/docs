# Adaptive Media Configuration Reference

With Adaptive Media, you can configure supported media types, determine the resolutions used for previews and thumbnails in the document library, and more. To access these settings,

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Control Panel_ tab, and click _System Settings_.
2.  Click _Adaptive Media_ under Content and Data.

    ![Access the System Settings](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adaptive-media-configuration-reference/images/01.png)

Configuration options appear in four tabs:

* System Images Resolutions: Configure resolutions used for document library thumbnails and previews.
* Images: Configure which media types are supported by Adaptive Media.
* ImageMagick: Configure which media types are generated using ImageMagick.
* Processes: Configure asynchronous processing.

### Systems Images Resolutions <a href="#systems-images-resolutions" id="systems-images-resolutions"></a>

Here you can override Adaptive Media’s default behavior for document library previews and thumbnails. By default, Adaptive Media chooses the best image resolution for previews and thumbnails based on the closest dimensions to the user’s screen type and resolution. But you can enter resolution IDs to override this process (e.g., `Preview-1000x0`).

![Access the System Settings](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adaptive-media-configuration-reference/images/02.png)

| Field                            | Description                                                                                                    |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Preview Configuration            | Enter a resolution ID to override the default adaptive media behavior for document library previews.           |
| Thumbnail Configuration          | Enter a resolution ID to override the default adaptive media behavior for document library thumbnails.         |
| Custom Thumbnail Configuration 1 | Enter a resolution ID to override the default adaptive media behavior for document library custom thumbnail 1. |
| Custom Thumbnail Configuration 2 | Enter a resolution ID to override the default adaptive media behavior for document library custom thumbnail 2. |

### Images <a href="#images" id="images"></a>

Here you can determine the media types processed by Adaptive Media and enable or disable Gifsicle.

![Determine supported media types.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adaptive-media-configuration-reference/images/03.png)

| Field               | Description                                                                                                                                                                                                                                                                                  |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Supported MIME Type | Determine the media types processed by Adaptive Media. If an image is uploaded and its media type is not listed, Adaptive Media ignores the image. By default, this list includes `bmp`, `gif`, `jpeg`, `pjpeg`, `png`, `svg+xml`, `x-citrix-jpeg`, `x-citrix-png`, `x-ms-bmp`, and `x-png`. |

![Enable or disable Gifsicle.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adaptive-media-configuration-reference/images/04.png)

| Field    | Description                                                                                                  |
| -------- | ------------------------------------------------------------------------------------------------------------ |
| Gifsicle | You can enable Gifsicle to adapt gif files. See Enabling Optimization of Animated GIFs for more information. |

### ImageMagick <a href="#imagemagick" id="imagemagick"></a>

Here you can determine which media types are processed using ImageMagick. To use ImageMagick, you must first install it on your portal server. See Enabling ImageMagick and Ghostscript for more information.

![Determine which media types are processed using ImageMagick.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adaptive-media-configuration-reference/images/05.png)

| Field               | Description                                                                                                                                                                                                                                                                                                                                                                              |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Supported MIME Type | Determine the media types processed by ImageMagick in Adaptive Media. If an image is uploaded and its media type isn’t in this list, Adaptive Media does not use ImageMagick for generating its preview. By default, this list contains `avif`, `gif`, `heic`, `tiff`, and `webp`, but you can add other media types. See ImageMagick documentation for a list of supported media types. |

### Processes <a href="#processes" id="processes"></a>

Here you can configure Adaptive Media’s asynchronous processing. Modify these values to improve performance.

![Configure Adaptive Media's image processing.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-adaptive-media/adaptive-media-configuration-reference/images/06.png)

| Field          | Description                                                                                                                                                    |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Max Processes  | Set the maximum number of processes for generating adapted media. The default value is `5`.                                                                    |
| Core Processes | Set the number of processes always available for generating adapted media. The default value is `2`. This value cannot exceed the maximum number of processes. |

WARNING

Larger values for Max Processes and Core Processes can consume large amounts of memory and result in out of memory errors. Out of memory errors can also occur if source images are large. You should run performance tests to optimize these settings for the amount of memory available on your system.

\
