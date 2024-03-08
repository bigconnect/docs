# Using Workflow with Documents and Media

Documents and Media is fully integrated with Liferay’s [workflow engine](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/introduction-to-workflow). This means you can define custom review and approval processes for your files. In addition to standard workflow capabilities, Documents and Media also supports using multiple workflows. You can set a default workflow for all document types and configure individual folders to use alternative workflows for specific document types.

Consider a scenario where you’ve defined different [document types](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-document-types) for marketing, legal, and accounting files. Due to their content, each document type must go through a specific review and approval process. To achieve this, you can [create a custom workflow](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/designing-and-managing-workflows/workflow-designer) for each type and then configure your Documents and Media folders to use the custom workflows. Also, if you need all other document types to go through the same review and approval process, you can set a default workflow for your Documents and Media root folder.

### [Enabling a Default Workflow](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-workflow-with-documents-and-media#enabling-a-default-workflow) <a href="#enabling-a-default-workflow" id="enabling-a-default-workflow"></a>

Follow these steps to enable a default Workflow for all document types:

1. Open the _Documents and Media_ application in the desired Site or Asset Library.
2.  From the root folder (i.e., Home), click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the Application Bar and select _Edit_.

    ![Click the Actions button in the Application Bar and select Edit.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-workflow-with-documents-and-media/images/01.png)
3.  Select the desired _Workflow_ from the dropdown menu.

    ![Select a Workflow definition.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-workflow-with-documents-and-media/images/02.png)
4. Click _Save_.

This enables a default workflow for all document types in your Site or Asset Library. Now whenever users upload or edit files, the _Publish_ button is replaced with a _Submit for Workflow_ button. If needed, you can override this workflow for individual folders. See [Enabling Alternative Workflows for Folders and Document Types](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-workflow-with-documents-and-media#enabling-alternative-workflows-for-folders-and-document-types) for more information.

### [Enabling Alternative Workflows for Folders and Document Types](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-workflow-with-documents-and-media#enabling-alternative-workflows-for-folders-and-document-types) <a href="#enabling-alternative-workflows-for-folders-and-document-types" id="enabling-alternative-workflows-for-folders-and-document-types"></a>

Follow these steps to enable or disable a workflow for individual folders and document types:

1. Open the _Documents and Media_ application in the desired Site or Asset Library.
2.  Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired folder and select _Edit_.

    ![Click the Actions button for a folder and select Edit.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-workflow-with-documents-and-media/images/03.png)
3.  Under _Document Type Restrictions and Workflow_, select the desired configuration:

    **Use Document Type Restrictions and Workflow of the Parent Folder (parent-folder)**: Use the parent folder’s configuration. This is the default value.

    **Define Specific Document Type Restrictions and Workflow for this Folder (current-folder)**: Select a _custom workflow_ for individual document types, or select _No Workflow_ to disable the default workflow for individual types.

    **Default Workflow for This Folder (current-folder)**: Select a default Workflow for the current folder. This applies the workflow for all files in the folder, regardless of document type.

    ![Under Document Type Restrictions and Workflow, select the desired configuration.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/using-workflow-with-documents-and-media/images/04.png)
4. Click _Save_.

This enables the chosen workflows for the selected folder and document types.

\
