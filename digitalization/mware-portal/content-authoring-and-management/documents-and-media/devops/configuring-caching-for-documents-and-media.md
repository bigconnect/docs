# Configuring Caching for Documents and Media

By default, Documents and Media files are not cached by browsers or servers. This is because file visibility generally depends on dynamic properties and configurations (e.g., permissions). However, you can enable and configure caching if desired.

IMPORTANT

Caching configurations only apply to documents that are visible to the Guest role.

Follow these steps to configure caching for your Liferay system or instance:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _System Settings_ or _Instance Settings_. See [Understanding Configuration Scope](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope) to learn more about scoping configurations.
2.  Go to _Documents and Media_ → _Cache Control_.

    ![Go to Cache Control under Documents and Media settings.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/configuring-cache-control-for-documents-and-media/images/01.png)
3. Select a _Cache Control_ policy.
   * Private: Allows caching by a user’s browser
   * Public: Allows caching by a CDN or proxy server
4. Set the _Max Age_ for cached documents. This value determines the number of seconds a response is considered fresh. After the set time, the cached document version expires and requires refreshing. Leaving the value at `0` is equivalent to not specifying a value.
5.  (Optional) Enter a list of MIME types to exclude from caching (e.g., `application/pdf`, `image/jpeg`, `video/mp4`).

    TIP

    If you have MIME types that are frequently updated, you can disable caching for that type to ensure the latest version of the file is displayed to users.
6. Click _Save_.

\
