# Configuring Documents and Media Previews

By default, MWARE Portal uses PDFBox to generate previews for files added to the document library. This is because PDFBox is the only 100% Java-based tool that can be distributed with DXP.

However, since PDFBox only supports a limited number of file types, the portal also provides integration with third-party services to support additional file types. These services can be used to generate high quality file previews more quickly for more of your files.

IMPORTANT

To use third-party services to generate previews, you must first install them on the server running your portal instance.

MWARE Portal currently supports integration with these services:

* LibreOffice or OpenOffice: These programs can be used in server mode to generate thumbnails and previews for supported file types (`.pdf`, `.docx`, `.odt`, `.ppt`, `.odp`, etc.). You can also use them to convert documents and view them in your browser. See Enabling OpenOffice/LibreOffice Integration to learn more.
* ImageMagick (also requires Ghostscript): These programs provide fast, high-quality previews and conversions for image files. See Enabling ImageMagick and Ghostscript for more information.
*   FFmpeg: This program provides support for audio and video files. In addition to generating file previews, you can play multimedia in your browser’s native HTML 5 player. See Enabling FFmpeg for Audio and Video Previews for more information.

    NOTE

    Version 7.3.x and earlier versions use Xuggler for generating audio and video previews. See Enabling Xuggler for more information.

With these services installed and configured, Documents and Media can provide in-application previews of most file types.

### Configuring Supported Media Types for ImageMagick <a href="#configuring-supported-media-types-for-imagemagick" id="configuring-supported-media-types-for-imagemagick"></a>

If you’ve installed ImageMagick, Adaptive Media can use it to generate alternative resolutions for images stored in the document library. By default, ImageMagick supports `avif`, `gif`, `heic`, `tiff`, and `webp` files, but you can add additional media types if needed. See Adaptive Media Configuration Reference for more information.

### Manually Generating Previews and Thumbnails <a href="#manually-generating-previews-and-thumbnails" id="manually-generating-previews-and-thumbnails"></a>

Once you’ve enabled third-party services, the portal automatically uses them to generate previews and thumbnails for supported media types. However, if you’ve uploaded assets before enabling these services, you must generate the thumbnails and previews manually.

To generate them for image files, you can use Adaptive Media. See Managing Image Resolutions for more information.

For audio, video, PDF, and OpenOffice files, you can regenerate previews and thumbnails via the Server Administration application:

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Control Panel_ tab, and click _System Administration_.
2. Go to the _Resources_ tab.
3.  Under Regeneration Options, click _Execute_ for the desired media type.

    ![Under Regeneration Options, execute the desired media type.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/configuring-documents-and-media-previews/images/01.png)

This begins generating missing images for the documents. Running this process does not affect existing previews and thumbnails. If you want to regenerate all previews and thumbnails, first execute this clean-up action: _Reset preview and thumbnail files for documents and media_.

![Execute Reset preview and thumbnail files for documents and media.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/configuring-documents-and-media-previews/images/02.png)

This process does not affect Adaptive Media previews and thumbnails for image files. To manage them, use Adaptive Media.

\
