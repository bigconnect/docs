# Accessing Documents with WebDAV

You can access the Document Library from your desktop file manager via [WebDAV](https://en.wikipedia.org/wiki/WebDAV). WebDAV is a protocol based on HTTP for managing files stored on web servers. WebDAV is supported by most major operating systems and desktop environments, including Linux, macOS, and Windows.

To access the Document Library folder from a file browser, you must use your log-in credentials and the WebDAV URL of the folder you want to access.

### [Generating WebDAV Password](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/accessing-documents-with-webdav#generating-webdav-password) <a href="#generating-webdav-password" id="generating-webdav-password"></a>

Liferay DXP 2023.Q4+/Portal 7.4 GA98+

In the latest Liferay versions, you must generate a WebDAV password from your account settings to use in your WebDAV client. This is to avoid storing weak hashes used in digest authentication by WebDAV. Digest authentication is insecure and deprecated in Liferay. When trying to access a document through WebDAV for the first time, you’re prompted to generate a WebDAV password.

![You must generate a WebDAV password before using it for the first time.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/accessing-documents-with-webdav/images/03.png)

To generate a WebDAV password,

1. Click on the user-icon from your site’s home page and select _Account Settings_.
2. Select _Password_ from the left-menu.
3.  Scroll down to the _WEBDAV PASSWORD_ section and click _Generate WebDAV Password_.

    ![Generate a WebDAV password to use with your client.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/accessing-documents-with-webdav/images/04.png)

    This displays your WebDAV username and an auto-generated password. This is a read-only field, so you must copy it for future use. If you click the button to generate the WebDAV password again, it generates a new one, rendering the existing one useless.

Now, [follow the instructions below](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/accessing-documents-with-webdav#using-the-webdav-client-to-access-documents) and use the WebDAV username and password when prompted.

### [Using the WebDAV Client to Access Documents](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/accessing-documents-with-webdav#using-the-webdav-client-to-access-documents) <a href="#using-the-webdav-client-to-access-documents" id="using-the-webdav-client-to-access-documents"></a>

1. Open the Documents and Media application in the desired Site or Asset Library.
2.  Click the _Actions_ button for the desired folder (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Access from Desktop_.

    ![Select Access from Desktop to get the folder's WebDAV URL.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/accessing-documents-with-webdav/images/01.png)
3.  Copy the folder’s unique _WebDAV URL_.

    ![Copy the folder's WebDAV URL.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/accessing-documents-with-webdav/images/02.png)
4. Follow the instructions for your operating system.
   * **Windows:** Map a network drive to the WebDAV URL. Enter your credentials when prompted. Once authenticated, the Document Library folder appears as a network drive.
   * **macOS:** Open Finder and select _Go_ → _Connect to Server_. In the Server Address field, enter the WebDAV URL of the folder you want to access, then click _Connect_ and enter your credentials when prompted.
   * **Linux:** For Linux, you must slightly modify the folder’s WebDAV URL. For KDE Dolphin, change the URL’s protocol to `webdav://` instead of `http://`. For GNOME Files, change the URL’s protocol to `dav://` instead of `http://`. Paste the modified URL in your file manager, press _Enter_, and enter your credentials when prompted.

Once set up, you can use your operating system’s file browser to add, edit, and delete files in the connected Documents and Media folder. If you edit a file in this folder on your file system, all changes appear in Liferay and the file’s minor version is incremented.

\
