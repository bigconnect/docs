# Activating Workflow

You can activate a workflow process for an asset type either across the DXP instance or only on a specific site. When a workflow process is activated across the instance for a particular asset type such as _Wiki Pages_, all _Wiki Page_ submitted for publication must go through the review process regardless of which Site the _Wiki Page_ is created on.

WARNING

Workflow cannot be used with [Publications](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications) in Liferay 7.3.

You can enable or disable workflow on individual sites. By default, workflow is disabled on a site.

### [Activating Workflow Across a DXP Instance](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/activating-workflow#activating-workflow-across-a-dxp-instance) <a href="#activating-workflow-across-a-dxp-instance" id="activating-workflow-across-a-dxp-instance"></a>

1. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and click _Control Panel_.
2. Click _Workflow_ then _Process Builder_.
3.  Click the _Configurations_ tab.

    ![The Instance Wide Settings are found in the Configurations tab](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/activating-workflow/images/02.png)
4.  Click _Edit_ next to the Asset Type which will go through the workflow process (for example, _Knowledge Base Article_).

    ![Activating Workflow Across the Instance for Knowledge Base articles](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/activating-workflow/images/03.png)
5. Select the workflow desired for this asset type (_Single Approver_ is available out of the box).
6. Click _Save_.
7. Repeat for all the other asset types which require workflow.

### [Activating Workflow on a Site](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/activating-workflow#activating-workflow-on-a-site) <a href="#activating-workflow-on-a-site" id="activating-workflow-on-a-site"></a>

When a workflow process is enabled on a site, other sites on the same instances remain disabled. To enable a workflow process for each asset type per site:

1. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) then click the compass icon (![Compass](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-compass.png)) on the _Site Administration_ menu.
2. Select the site where the form will be created (for example, _Community Site_).
3. In the _Site Administration_ menu, click _Configuration_ → _Workflow_.
4. Click _Edit_ next to the Asset Type which will go through the workflow process.
5.  Select the workflow desired for this asset type.

    ![Activating Blog Entry on Community Site](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/activating-workflow/images/01.png)
6. Click _Save_.
7. Repeat for all the other asset types which require workflow.

Once enabled, the selected asset types always require a review process before publication on this site. If you have previously activated workflow for a particular asset type, those settings are reflected here.

TIP

You can always disable workflow for an asset type on a site even if it was enabled instance wide.

### [Activating Workflow For Specific Applications](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/activating-workflow#activating-workflow-for-specific-applications) <a href="#activating-workflow-for-specific-applications" id="activating-workflow-for-specific-applications"></a>

There are several assets types where workflow is enabled in their respective applications.

#### [Web Content Folders](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/activating-workflow#web-content-folders) <a href="#web-content-folders" id="web-content-folders"></a>

To activate a workflow for a _Web Content Folder_:

1. Navigate to the _Site Administration_ menu for your site.
2. Click _Content & Data_ → _Web Content_.
3.  Click (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) then _Edit_ next to the desired folder.

    ![Activating workflow on Web Content folders from the folder's edit screen.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/activating-workflow/images/04.png)
4. Expand the _Structure Restrictions and Workflow_ menu.
5.  Click the radio button for _Default Workflow for This Folder (Lunar Resort Holiday Getaway Content)_.

    ![Selecting workflow for a Web Content Folder.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/activating-workflow/images/05.png)
6. Select the workflow definition (for example, _Single Approver_).
7. Click the _Save_ button.

#### [Documents and Media Folders](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/activating-workflow#documents-and-media-folders) <a href="#documents-and-media-folders" id="documents-and-media-folders"></a>

Workflow can be enabled for the entire _Documents and Media_ application in the _Root_ folder or for individual folders.

To activate a workflow for a _Documents and Media Folder_:

1. Navigate to the _Site Administration_ menu for your site.
2. Click _Content & Data_ → _Documents and Media_.
3.  Click (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) then _Edit_ next to the desired folder.

    ![Selecting workflow for a DM Folder.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/activating-workflow/images/06.png)
4. Expand the _Document Type Restrictions and Workflow_ menu.
5.  Click the radio button for _Default Workflow for This Folder (DM Folder 1)_.

    ![Selecting workflow for a DM Folder.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/activating-workflow/images/07.png)
6. Select the workflow definition (for example, _Single Approver_).
7. Click the _Save_ button.

#### [Dynamic Data Lists](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/activating-workflow#dynamic-data-lists) <a href="#dynamic-data-lists" id="dynamic-data-lists"></a>

To activate a workflow for a _DDL Form_:

1. Navigate to the _Site Administration_ menu for your site.
2. Click _Content & Data_ → _Dynamic Data Lists_.
3. Click (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to add a new form.
4. Enter a name for the DDL form.
5. Click _Select_ to choose a Data Definition associated with this form.
6. Select the Data Definition (_Contacts_).
7.  Under _Workflow_, select the desired workflow (_Single Approver_).

    ![Activate workflow for each individual Dynamic Data List.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/activating-workflow/images/08.png)
8. Click _Save_.

This particular DDL form now goes through the submission process.

#### [Forms](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/activating-workflow#forms) <a href="#forms" id="forms"></a>

To activate workflow for each individual form’s entries

1. Navigate to the _Site Administration_ menu for your site.
2. Click _Content & Data_ → _Forms_.
3.  In the _Form Builder_ view (it does not matter whether this is a new form or an existing one), click _Options_ (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) then _Settings_.

    ![Navigating to Form Settings](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/activating-workflow/images/09.png)
4.  Under _Select a workflow_, select the desired workflow (_Single Approver_).

    ![Activate workflow on each form's entries from the Form Settings window.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/activating-workflow/images/10.png)
5. Click _Done_ then close the dialog window.

The form now requires approval before the information is submitted.

\
