# Securing Site Documents Content

By default, your Site’s Documents and Media files and folders are not visible in the web server directory index, so you can’t view them through the browser. To enable browsing a Site’s documents through the web server directory index, you can enable this setting for your Site.

Once enabled, your can browse your Site’s Documents and Media files at `http://localhost:8080/documents/site-name` (e.g. `http://localhost:8080/documents/marketing` for a Site called _Marketing_).

### [Configuring Document Visibility in the Web Server Directory Index](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-content-configurations/securing-site-documents-content#configuring-document-visibility-in-the-web-server-directory-index) <a href="#configuring-document-visibility-in-the-web-server-directory-index" id="configuring-document-visibility-in-the-web-server-directory-index"></a>

1. Access the Enable Directory Indexing option:
   *   In Liferay DXP 7.4+

       1. From the Site Menu, go to _Configuration_ → _Site Settings_.
       2. In the Content and Data section, click _Documents and Media_.

       ![In Liferay DXP 7.4+, change the Enable Directory Indexing option from the Site Settings section.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-content-configurations/securing-site-documents-content/images/02.png)
   *   In previous Liferay DXP versions

       1. From the Site Menu, go to _Configuration_ → _Settings_.
       2. Under the General area, expand the _Documents and Media_ section.

       ![In previous Liferay DXP versions, change the Enable Directory Indexing option from the Settings section.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-content-configurations/securing-site-documents-content/images/01.png)
2.  Toggle the _Enable Directory Indexing_ setting to enable or disable this setting.

    IMPORTANT

    Once enabled, only Administrators and Users with view permission can browse your Site’s Documents and Media files in the web server directory index.
3. Click _Save_.

\
