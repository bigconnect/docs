# Site Staging UI Reference

After enabling Local or Remote Live Staging for your site, most page configuration options are only accessible in your Staging environment. This prevents users from directly editing live pages.

Once staging is enabled, additional options appear in your Staging environment. These options are in the Staging Bar, Publish to Live menu, and Staging Page.

### Staging Bar and Editing Tools <a href="#staging-bar-and-editing-tools" id="staging-bar-and-editing-tools"></a>

When Staging is enabled for a site, you can access additional options via the staging bar on each of your site’s pages. In this bar, you can toggle between Staging and Live environments and click on _Publish to Live_ for staged pages.

![When Staging is enabled for a site, you can access the staging bar on each of your site's pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/01.png)

The following table and pictures describe the staging bar options and editing tools for widget pages:

| Option                      | Widget Page                                                                                                                                                                                                                                                                                                                                                             | # |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| _Site Pages Variation_      | <ul><li>Toggle between variations of page sets in your staged site using the drop-down menu.</li><li>Manage and create page set variations by clicking on the <em>Actions</em> button in the Staging Bar and selecting <em>Site Pages Variation</em>. See Page Versioning for more information.</li></ul>                                                               | 1 |
| _Page Variations_           | <ul><li>Toggle between variations of a single page in your staged site using the drop-down menu.</li><li>Manage and create page variations by clicking on the <em>Actions</em> button in the Staging Bar and selecting <em>Page Variations</em>. See Page Versioning for more information.</li></ul>                                                                    | 2 |
| _Undo/Redo_                 | <ul><li>Click on the <em>Actions</em> button in the Staging Bar and select <em>Undo</em>/<em>Redo</em>.</li><li>Once you’ve marked a page as <em>Ready for Publish Process</em>, you can’t undo or redo recent changes.</li></ul>                                                                                                                                       | 3 |
| _History_                   | <ul><li>Manage the page revision history by clicking on the <em>Actions</em> button in the Staging Bar and selecting <em>History</em>.</li><li>Revert a page to an earlier version by clicking on the <em>Actions</em> button for the page version you want to restore and selecting <em>Mark as Ready for Publishing Process</em>.</li></ul>                           | 4 |
| _Ready for Publish Process_ | <ul><li>Click on <em>Ready for Publish Process</em> to change the page’s status from <em>Draft</em> to <em>Ready for Publish Process</em>.</li><li>Click <em>Publish to Live</em> to configure and initiate publishing for your changes.</li><li>When you publish a page to Live, only the version marked as <em>Ready for Publish Process</em> is published.</li></ul> | 5 |
| Edit Content                | Use the _Add_ button in the portal application bar to edit your Widgets.                                                                                                                                                                                                                                                                                                | 6 |

![Use the staging bar and editing tools in widget pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/02.png)

### Publish to Live <a href="#publish-to-live" id="publish-to-live"></a>

When you click on _Publish to Live_, a modal window appears with additional options for configuring your publish process. You can publish using _Simple_ or _Advanced_ settings.

![Click on Publish to Live to access the publishing modal window.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/03.png)

### Simple Publishing <a href="#simple-publishing" id="simple-publishing"></a>

**Name**: Enter a descriptive name for your publish process into the _Name_ field.

**Changes Since Last Publish Process**: View staged changes made since your last publish process.

**Pages to Publish**: View the number of pages included in your current publish process.

NOTE

This displays the number of page deletions tracked by the Staging framework. This number counts the page deletions on the Staging site, not how many pages will be deleted on the on the Live site. There can be inconsistency between the number of page deletions to be published and the actual number of pages present on either of the Staging and Live sites.

When ready, click on _Publish to Live_ to initiate publishing.

![Click on Publish to Live to initiate publishing.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/04.png)

### Advanced Publishing <a href="#advanced-publishing" id="advanced-publishing"></a>

With Advanced publishing, you can access information and settings that you can use to customize your publish process. In the _Custom_ tab, you can specify content, dates, pages, and more. Alternatively, you click on the _Publish Template_ tab to select a pre-configured template for your publish process. To access advanced settings, click on _Switch to Advanced Publish Process_ in the modal window.

![Use advanced setting to customize your publishing process.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/05.png)

Custom advanced settings are divided into the following sections:

**Name**: Enter a descriptive name for your publish process into the _Name_ field.

**Date**: Select whether to publish your changes immediately, or _schedule_ your changes to be published at a later date and time. You can also determine whether and how frequently the publish process is repeated.

![Select when to publish your changes.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/06.png)

**Deletions**: Select whether you want to _Delete Application Data Before Importing_, and whether you want to _Replicate Individual Deletions_.

![Select whether you want to Delete Application Data Before Importing and whether you want to Replicate Individual Deletions.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/07.png)

**Pages**: Select which page set variation and individual pages to publish, as well as the look and feel of your selected pages. You can also select _Delete Missing Pages_, which deletes all pages from the Live site that are not present on the Staging site. If you choose a page to be published from the pages menu, the widgets and their references are always published.

![Select which page set variation and individual pages to publish, as well as the look and feel of selected pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/08.png)

If you want to publish pages with a custom theme, you must check the Theme Settings option under the _Look and Feel_ heading for your staging configuration. Otherwise, the default theme is applied.

**Content**: Select the content you want to include in your publish process. Using the radio buttons, you can publish _all_ content of the selected types, or specify a date and time parameter to limit the content published. You can also use this option to check the content that changed in Staging and is awaiting publication.

![Select the content you want to include in your publish process.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/09.png)

| Option                   | Description                                                 |
| ------------------------ | ----------------------------------------------------------- |
| _All_                    | Shows all the content available for publication.            |
| _From Last Publish Date_ | Only shows the content updated since the last publication.  |
| _Date Range_             | Shows the content updated during a specific period of time. |
| _Last_                   | Shows the content updated in the last hours or days.        |

NOTE

After selecting your Content option, click _Refresh Counts_ to update the number of results.

You can also click on _Change_ for each content type to manage items and other settings included in each type, such as _version history_, _vocabularies_, and _previews and thumbnails_. See Managing Data and Content Types in Staging for more information.

IMPORTANT

By default, content displayed using the following widgets is always included in publishing processes: Dynamic Data Lists Display, Form, Web Content Display, and Wiki Display. For versions 7.4+, you can disable this behavior so that displayed content is only published when explicitly included in the publishing process’s configuration. See Configuring Automatic Publishing of Displayed Content for more information.

**Permissions**: Select whether to include permissions for the pages and widgets when your changes are published.

![Select whether to include permissions for the pages and widgets when your changes are published.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/10.png)

Instead of configuring advanced options manually every time you publish a page or content, you can save your staging configuration as a _publish template_ for future use. To view and select a saved template for your publish process, click on the _Publish Templates_ tab.

![To view and select a saved template for your publish process, click on the Publish Templates tab.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/11.png)

When ready, click on _Publish to Live_ to initiate publishing.

### Staging Page <a href="#staging-page" id="staging-page"></a>

Once you’ve enabled Staging, the options available from the _Publishing_ tab are modified. When in the Live environment, you can only access the _Export_ feature. When in the Staging environment, you can only access the _Import_ and _Staging_ features.

You can also access new options in the Staging page: view publishing processes, create and manage publish templates, and modify or disable Staging for your site.

![You can also access new options in the Staging page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/12.png)

#### Previous and Future Publishing Processes <a href="#previous-and-future-publishing-processes" id="previous-and-future-publishing-processes"></a>

When you publish with Staging, it’s captured as a _process_ and stored for future reference. To view and manage these processes, go to _Site Menu_ → _Publishing_ → _Staging_.

In the _Processes_ tab is a list of staging processes that have been completed. From here, you can _relaunch_, _clear_ or view a _summary_ of any previous publications by clicking on a process’s _Actions_ button ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ).

![View and manage previous publishing processes from the Staging page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/13.png)

You can also click on the _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) button to create a new publish process using a custom configuration or pre-defined publishing template.

In the _Scheduled_ tab, you can view all scheduled publishing processes.

#### Publish Templates <a href="#publish-templates" id="publish-templates"></a>

If you regularly create similar staging configurations, you can create your own _publish templates_ to save time. To create a template, click _Actions_ ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) in the _Application_ bar of the _Staging_ page. Then select _Publish Templates_.

![Click on the Actions button in the Application bar of the Staging page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/14.png)

From here, you can create and manage templates for frequent publishing configurations. You can then use a template by clicking on its _Actions_ button ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) and selecting _Publish_.

![Create and manage templates for frequent publishing configurations.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/15.png)

You can use a template when publishing a page or content using the _Publish to Live_ button in the Staging Bar. See Advanced Publishing for more information.

#### Staging Configuration <a href="#staging-configuration" id="staging-configuration"></a>

After setting up Staging, you can access the Staging Configuration page by clicking _Actions_ ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) in the _Application_ bar of the _Staging_ page. Then, select _Staging Configuration_.

![Click on the Actions button in the Application bar of the Staging page to access Staging Configuration.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/site-staging-ui-reference/images/16.png)

From here, you can disable Staging for your site, or modify your page versioning settings. However, _Staged Content_ options cannot be modified after the initial setup.

\
