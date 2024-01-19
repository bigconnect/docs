# Structure Settings

After you have created a structure and configured its fields, there is additional configuration to consider including permissions and remote access to managing structures.

#### ASSIGNING PERMISSIONS <a href="#assigning-permissions" id="assigning-permissions"></a>

Permissions on structures can be set just like any other permission. Most users should not be able to edit structures. Structures can be coupled with templates or used on display pages, which require some web development knowledge to create. This is why only trusted developers should be able to create structures. Users, of course, should be able to view structures. The _View_ permission enables them to make use of the structures to create content.

![Figure 1: Youre able to assign structure permissions via the Actions button.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159831/web-content-structure-permissions.png)

The best practice for structure permissions is to grant or deny them based on Roles.

#### WEBDAV URL <a href="#webdav-url" id="webdav-url"></a>

The WebDAV URL feature is available for web content structures and templates so users can upload and organize resources from both a web interface and the file explorer of their desktop operating system. With the WebDAV URL, site administrators can add, browse, edit, and delete structures and templates on a remote server. After you complete your structure, you can access the WebDAV URL by re-opening the structure or template and clicking the _Details_ section.&#x20;

**Note:** Some operating systems require a WebDAV server to be class level 2 (to support file locking) before allowing files to be read or written. The Documents and Media library uses a class level 2 WebDAV server but Web Content structures and templates do not. This means that the portal’s Document and Media library supports WebDAV file locking but Web Content structures and templates do not. However, on operating systems which require WebDAV servers to be class level 2, it’s possible to avoid the restriction by using third-party WebDAV clients (e.g., [Cyberduck](http://cyberduck.ch/)).
