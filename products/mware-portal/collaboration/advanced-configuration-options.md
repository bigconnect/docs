# Advanced Configuration Options

Adaptive Media’s advanced configuration options are available in System Settings. Open the Control Panel and go to _Configuration_ → _System Settings_, then select _Adaptive Media_. There are two configurations listed under _SYSTEM SCOPE_:

* Images
* Processes

The _Images_ configuration contains the following options:

**Supported MIME Types:** A list of the image MIME types that Adaptive Media supports. If an image is uploaded and its MIME type isn’t in this list, Adaptive Media ignores the image. By default, this list contains many common MIME types.

**Gifsicle:** To scale animated GIFs, Adaptive Media uses an external tool called [Gifsicle](https://www.lcdf.org/gifsicle/). First install Gifsicle on the server, ensure that it’s on the `PATH` environment variable, and then click the box next to _Gifsicle Enabled_. If Gifsicle isn’t installed and `image/gif` is included as a supported MIME type, Adaptive Media scales only one frame of the GIF, making a static GIF.

**Max Image Size:** Maximum size of the source images that Adaptive Media can use to generate adapted images. Adaptive Media will not generate adapted images for source images larger than this setting. The default value is 10 MB. To generate adapted images for all source images regardless of size, set this to -1. Since generating adapted images from large source images requires significant amounts of memory, you can specify a lower _Max Image Size_ to avoid out of memory errors.

![Figure 1: You can configure Gifsicle and the maximum image size for Adaptive Media.](https://liferay-support.zendesk.com/hc/article\_attachments/360029925132/adaptive-media-config-01.png)

The _Processes_ configuration is related to Adaptive Media’s asynchronous processing. These values can be modified to improve performance for specific scenarios or use cases. The following options are available:

**Max Processes:** The maximum number of processes for generating adapted media. The default value is `5`.

**Core Processes:** The number of processes always available for generating adapted media. The default value is `2`. This setting can’t exceed the _Max processes_ setting.

**Warning:** Larger values for Max Processes and Core Processes may cause out of memory errors, as processing more images at once can consume large amounts of memory. Out of memory errors can also occur if the source images Adaptive Media uses to generate adapted images are large. You can restrict the maximum size of such images via the _Max Image Size_ setting in the _Adaptive Media Image_ configuration, which is described next. You should run performance tests to optimize these settings for the amount of memory available on your system.

![Figure 2: You can also configure Adaptive Medias image processing resources.](https://liferay-support.zendesk.com/hc/article\_attachments/360029925112/adaptive-media-config-02.png)

