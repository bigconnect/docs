# Staging

Staging application provides a working environment for making changes to a site or asset library before publishing your changes to your live environment. With it, you can modify staged application data and pages behind the scenes and then publish your changes when ready, whether individually or all together, while the live environment handles incoming user traffic.

![MWARE Portal creates a separate copy of the original site or library.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/images/01.png)

When setting up Staging, determine whether to host your site or asset library locally (i.e., on the same server) or remotely (i.e., on separate servers connected by a network). Then customize which application data is staged. For sites, you can also enable page versioning for widget pages and content pages.

IMPORTANT

Staging is supported for asset libraries since version 7.4 and 7.3.10. However, you should only connect staged sites with staged asset libraries, and vice versa. See Using Staging in Asset Libraries for more information.

Publications is recommended over Staging in most use cases. See Comparing Publishing Tools for more information.

### Configuration Options <a href="#configuration-options" id="configuration-options"></a>

There are two options for configuring Staging: _Local Live_ and _Remote Live_.

**Local Live Staging**: You can host both your staging and live environments on the same portal server. When enabled, the portal creates a local clone of your site or asset library that serves as the staging environment, while your original site or asset library becomes your live environment.

**Remote Live Staging**: You can host your staging and live environments on separate portal servers. When enabled, the site or asset library used to configure Staging becomes your staging environment, while the configured remote server becomes your live environment.

While both options use the same interface for managing and publishing staged pages, applications, and content, they differ in their setup. Once you’ve chosen the configuration option that best suits your needs, see Configuring Local Live Staging or Configuring Remote Live Staging for more information and setup instructions.

#### Staged Data and Content Types <a href="#staged-data-and-content-types" id="staged-data-and-content-types"></a>

As part of Staging configuration, you can select which content groups and application data are staged. If staged, the selected data and content are managed by your staging environment. Also, when an application or content group is selected, all contained entities (e.g., application folders) are staged as well. See Managing Data and Content Types in Staging for more information.

#### Page Versioning <a href="#page-versioning" id="page-versioning"></a>

When configuring Staging for sites, you can enable the _page versioning_ feature. With this feature, you and your team can develop variations of a site’s content pages and widget pages simultaneously. You can see the complete history of page versions and revert pages to an earlier version if needed. See Page Versioning to learn how to create and manage variations and Managing Staging Permissions to learn about managing permissions for each variation.

### Understanding the Publishing Process <a href="#understanding-the-publishing-process" id="understanding-the-publishing-process"></a>

From a low level perspective, staging is an equivalence relation where entities are mirrored to a different location. From a high level perspective, the staging process is executed in three sequential phases: Export, Validation, and Import.

Understanding this process can improve efficiency and help you plan ahead to achieve a seamless publishing experience. See Understanding the Publishing Process for more information.

