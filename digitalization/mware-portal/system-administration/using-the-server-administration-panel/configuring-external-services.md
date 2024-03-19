# Configuring External Services

Liferay provides integration with external services for converting files and generating file previews. Three of these services can be enabled in the Server Administration settings: [ImageMagick](https://www.imagemagick.org/script/index.php), [Ghostscript](https://www.ghostscript.com/), and [Xuggler](http://www.xuggle.com/xuggler/).

IMPORTANT

As of Liferay 7.3.x, the Xuggler integration is deprecated. Users are advised to use Liferay’s FFmpeg integration as an alternative. See [Enabling FFmpeg for Audio and Video Previews](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/devops/configuring-documents-and-media-previews) for more information.

![Enable ImageMagick, Ghostscript, and Xuggler from the External Services tab.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/using-the-server-administration-panel/configuring-external-services/images/01.png)

Each external service must be installed on the server before it can be enabled for your Liferay instance. When installing the service, be sure to use the latest stable version for your operating system, as older versions may not run properly with Liferay DXP.

After installing these services, configure your Liferay instance to use them via the Control Panel’s Server Administration page.

NOTE

As of Liferay 7.1, OpenOffice/LibreOffice is configured in OSGi Configuration Admin instead of Server Administration or portal properties. See [Enabling OpenOffice/LibreOffice Integration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/devops/enabling-openoffice-libreoffice-integration) for more information.

### [Enabling ImageMagick and Ghostscript](https://learn.liferay.com/web/guest/w/dxp/system-administration/using-the-server-administration-panel/configuring-external-services#enabling-imagemagick-and-ghostscript) <a href="#enabling-imagemagick-and-ghostscript" id="enabling-imagemagick-and-ghostscript"></a>

By default, Documents and Media uses [PDFBox](https://pdfbox.apache.org/) to generate previews. [ImageMagick](https://www.imagemagick.org/script/index.php) and [Ghostscript](https://www.ghostscript.com/) provide faster and higher-quality previews and conversions and support more image file types. To work, both services must be installed and enabled together.

NOTE

Depending on your OS, these services may already be installed. If you’re on Linux, both are likely already installed. However, they are not likely to be installed on Windows, while they may be on macOS.

Once both ImageMagick and Ghostscript are installed on the server, follow these steps to enable these services for your Liferay instance:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), and go to _Control Panel_ → _Configuration_ → _Server Administration_.
2. Click on the _External Services_ tab.
3. Check _Enabled_ for ImageMagick and Ghostscript.
4.  Verify the _Path_ field has the correct path(s) to ImageMagick and Ghostscript executables. Use a semicolon as a separator if they are in separate paths.

    For example, depending on where you installed them, the correct paths may look like this on a Windows server:

    ```log
    C:\\Program Files\\gs\\gs10.1.0\\bin;C:\\Program Files\\ImageMagick
    ```

    By default, the executables for both are installed in `/usr/bin/` on Linux.
5. Configure resource limits.
6. Click _Save_ when finished.

### [Enabling Xuggler](https://learn.liferay.com/web/guest/w/dxp/system-administration/using-the-server-administration-panel/configuring-external-services#enabling-xuggler) <a href="#enabling-xuggler" id="enabling-xuggler"></a>

By default, Documents and Media doesn’t generate previews for audio and video files. Using Liferay’s Xuggler integration, you can convert and generate previews for these files. If you don’t already have Xuggler installed on your server, you can install it when you enable it in Server Administration.

TIP

Xuggler requires glibc version 2.6 or later on Linux.

Follow these steps to install and enable Xuggler for your Liferay instance:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), and go to _Control Panel_ → _Configuration_ → _Server Administration_.
2. Click on the _External Services_ tab.
3. Check _Enabled_ for Xuggler.
4.  (If Xuggler is not installed) Select the correct JAR for the OS, and click _Install_.

    Restart the server to apply changes. Once installed, return to the External Services tab to enable Xuggler.

    ![If Xuggler isn't installed, you'll be prompted to install it.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/using-the-server-administration-panel/configuring-external-services/images/02.png)
5. Click _Save_.

### [Using a `portal-ext.properties` File to Enable External Services](https://learn.liferay.com/web/guest/w/dxp/system-administration/using-the-server-administration-panel/configuring-external-services#using-a-portal-extproperties-file-to-enable-external-services) <a href="#using-a-portal-extproperties-file-to-enable-external-services" id="using-a-portal-extproperties-file-to-enable-external-services"></a>

In addition to the Control Panel, you can enable these external services using a `portal-ext.properties` file.

#### [Enabling ImageMagick and Ghostscript](https://learn.liferay.com/web/guest/w/dxp/system-administration/using-the-server-administration-panel/configuring-external-services#enabling-imagemagick-and-ghostscript-1) <a href="#enabling-imagemagick-and-ghostscript-1" id="enabling-imagemagick-and-ghostscript-1"></a>

To enable ImageMagick and Ghostscript using a `portal-ext.properties`, add the `imagemagick.enabled` and `imagemagick.global.search.path` properties to your file. Ensure the search path points to the directories containing the ImageMagick and Ghostscript executables. You may also need to configure the path for fonts used by Ghostscript when in macOS or Unix environments.

```properties
imagemagick.enabled=true
imagemagick.global.search.path[apple]=/opt/local/bin:/opt/local/share/ghostscript/fonts:/opt/local/share/fonts/urw-fonts
imagemagick.global.search.path[unix]=/usr/local/bin:/usr/local/share/ghostscript/fonts:/usr/local/share/fonts/urw-fonts
imagemagick.global.search.path[windows]=C:\\Program Files\\ImageMagick
```

#### [Enabling Xuggler](https://learn.liferay.com/web/guest/w/dxp/system-administration/using-the-server-administration-panel/configuring-external-services#enabling-xuggler-1) <a href="#enabling-xuggler-1" id="enabling-xuggler-1"></a>

To enable Xuggler using a `portal-ext.properties` file, add the following code to the file.

```properties
xuggler.enabled=true
```

Restart the application server to finish enabling Xuggler.

\
