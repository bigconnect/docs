# Understanding the Publishing Process

Publishing is the process whereby changes made in the Staging environment are transferred to the Live environment. Understanding this process can improve efficiency and help you plan ahead to achieve a seamless publishing experience.

* [The Staging Process](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#the-staging-process)
* [Planning Ahead for Staging](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#planning-ahead-for-staging)

### [The Staging Process](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#the-staging-process) <a href="#the-staging-process" id="the-staging-process"></a>

From a low level perspective, staging is a relationship between two Sites or Asset Libraries where the same entities are mirrored between them. From a high level perspective, staging involves a publish process that is executed in three sequential phases: Export, Validation, and Import.

IMPORTANT

When using [Custom Fields](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/users/adding-custom-fields-to-users) with Remote Live Staging, you must export and import new custom fields manually to your Live environment before publishing. This is because custom fields are not site-scoped and cannot be exported/imported automatically along with content changes.

#### [Export Phase](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#export-phase) <a href="#export-phase" id="export-phase"></a>

During Export, your publish configuration is processed and necessary referenced entities are gathered. Then everything is processed into the instance’s own file format, and it is either stored locally or transferred to the remote live DXP instance.

#### [Validation Phase](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#validation-phase) <a href="#validation-phase" id="validation-phase"></a>

During Validation, everything is checked to determine whether it’s possible to start the import process. This includes verifying the file versions and integrity, checking for additional system information (e.g., language settings), and ensuring that the files do not reference missing content.

If anything is not verified during the publish process, the transactional database reverts the Site or Asset Library back to its original state, discarding the current publish process. This is necessary to safeguard against publishing incomplete information.

If, however, the Document Library’s file system is not database-stored (e.g., [DBStore](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/other-file-store-types/dbstore)), it’s not transactional and isn’t reverted if a staging failure occurs. This could cause a discrepancy between a file and its reference in the database. To preserve data integrity, ensure that regular backups of both the database and file system are maintained before staging the document library.

#### [Import Phase](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#import-phase) <a href="#import-phase" id="import-phase"></a>

During Import, any necessary updates or additions are made to the Site’s content, layouts, and apps according to the publishing parameters. If everything is verified and correct, the staged content is published to your live Site. The same is true for Asset Libraries.

### [Planning Ahead for Staging](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#planning-ahead-for-staging) <a href="#planning-ahead-for-staging" id="planning-ahead-for-staging"></a>

Staging is a complex subsystem that’s flexible and scalable. Before you begin using it for your Site or Asset Library, it’s important to plan ahead and remember a few tips for a seamless process. There are several factors to evaluate.

#### [Content Types](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#content-types) <a href="#content-types" id="content-types"></a>

Depending on the content in your Site and Asset Library, you can turn on Staging for only the necessary content types during your initial setup. You can also configure your publish process to only include certain types of content. Both of these measures can help to avoid unnecessary work.

IMPORTANT

By default, content displayed using the following widgets is always included in publishing processes: Dynamic Data Lists Display, Form, Web Content Display, and Wiki Display. For Liferay DXP/Portal 7.4+, you can disable this behavior so that displayed content is only published when explicitly included in the publishing process’s configuration. See [Configuring Automatic Publishing of Displayed Content](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-automatic-publishing-of-displayed-content) for more information.

#### [Hardware Environment](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#hardware-environment) <a href="#hardware-environment" id="hardware-environment"></a>

Plan your environment according to your content types. If your Site or Asset Library uses large images and video files, decide if a shared network drive is the best option. Storing many large images in the Document Library usually requires a faster network or local storage. If you’re dealing with web content, however, these are usually smaller and take up very little disk space.

#### [Customizations and Custom Logic for Your Staging Environment](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#customizations-and-custom-logic-for-your-staging-environment) <a href="#customizations-and-custom-logic-for-your-staging-environment" id="customizations-and-custom-logic-for-your-staging-environment"></a>

Your organization’s business logic is most likely implemented in an app, and if you want to support Staging for that app, you must write some code to accomplish this. You can also consider changing default UI settings by writing new JSP code if you want to change your Staging environment’s look and feel.

#### [Publishing Wait Times](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#publishing-wait-times) <a href="#publishing-wait-times" id="publishing-wait-times"></a>

Enable Staging at the beginning of Site or Asset Library creation to avoid huge publishing processes that can take a long time to execute. Small, incremental changes are far more performant than huge re-imaginings.

#### [JVM/Network Configuration](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/understanding-the-publishing-process#jvmnetwork-configuration) <a href="#jvmnetwork-configuration" id="jvmnetwork-configuration"></a>

For Staging, JVMs/networks should be configured with a minimum of 4GB of memory and 20MB/s transfer rate (disk).

\
