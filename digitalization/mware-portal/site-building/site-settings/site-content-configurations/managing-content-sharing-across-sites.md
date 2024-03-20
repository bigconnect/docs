# Managing Content Sharing Across Sites

You can allow child Sites to display content from parent Sites. This content includes

* Web Content Structures
* Web Content Templates
* Document Types
* Vocabularies and Categories
* Application Display Templates
* Data Definitions

You can configure the Content Sharing settings at the Instance or Site scope. The Instance scope configuration determines the default Site scope configuration. For example, if you disable Content Sharing at the Instance scope, the setting is disabled by default at the Site scope.

### Configuring Content Sharing at the Instance Scope <a href="#configuring-content-sharing-at-the-instance-scope" id="configuring-content-sharing-at-the-instance-scope"></a>

1. Open the Product Menu and go to _Control Panel_ → _Configuration_ → _Instance Settings_.
2. Under the Content and Data section, click _Sharing_.
3. Under the Virtual instance scope, click _Content Sharing_.
4. Check the _Allow site administrators to display content from other sites they administer_ box to allow Site Administrators to share content between the Sites they administer.
5. Configure the Content Sharing setting between Sites and Subsites.

### Configuring Content Sharing at the Site Scope <a href="#configuring-content-sharing-at-the-site-scope" id="configuring-content-sharing-at-the-site-scope"></a>

NOTE

If you disable Content Sharing at the Instance scope, the option is unavailable at the Site scope.

1. Access the Content Sharing settings.
   * In version 7.4+
     1. From the Site Menu, go to _Configuration_ → _Site Settings_.
     2. In the Content and Data section, click _Sharing_.
   * In previous versions
     1. From the Site Menu, go to _Configuration_ → _Settings_.
     2. Under the General area, expand the _Documents and Media_ section.
2. Configure the Content Sharing setting between Sites and Subsites.

### Configure the Content Sharing Setting Between Sites and Subsites <a href="#configure-the-content-sharing-setting-between-sites-and-subsites" id="configure-the-content-sharing-setting-between-sites-and-subsites"></a>

In the _Allow subsites to display content from parent sites_ drop-down menu, select one of the following options:

* **Enabled by Default**: Child Sites can display content from parent Sites by default. You can change this option at Site scope.
* **Disabled by Default**: Child Sites cannot display content from parent Sites. You can change this option at Site scope.
* **Disabled**: Child Sites cannot display content from parent Sites. You cannot change this option at Site scope.

![Configure the Content Sharing Settings for your Virtual Instance and Site.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-content-configurations/managing-content-sharing-across-sites/images/01.png)

\
