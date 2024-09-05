# Designing and Managing Workflows

With the workflow framework, Users manage asset publication in custom-built workflow processes. Users with permission to access workflow process definitions in the Control Panel can upload new workflow definitions, edit existing definitions, and delete definitions for processes no longer in use.

### Workflow Definition Publication Permissions <a href="#workflow-definition-publication-permissions" id="workflow-definition-publication-permissions"></a>

Users with permission to edit or publish workflow definitions can add Groovy scripts to the workflow. Access to the scripting engine means access to the server’s Java Virtual Machine (JVM). Users with permission to publish (or edit) can therefore access any data within the reach of the JVM, such as data contained in a separate Virtual Instance of the portal.

Because of this far-reaching access, permission to create or edit workflow definitions is limited to Regular Administrators of the Default Virtual Instance. For the portal customers, Users (e.g., Site Administrators) can also edit and publish workflows with their access to the Kaleo Designer and Kaleo Forms Admin applications.

To grant Users with these Roles the workflow publication access in additional Virtual Instances,

1. Make sure you understand the access you’re granting these administrators.
2. Navigate to Control Panel → System Settings → Workflow → Workflow Definition.
3. Check the box for the setting _Allow Administrators to Publish and Edit Workflows_.

![Explicit permission must be granted before administrators are allowed to publish and edit workflow definitions.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/managing-workflows/images/06.png)

This only applies to Virtual Instances that have been added to the system. The Default Virtual Instance provides out of the box workflow publication access to Regular Administrators, via Control Panel → Workflow → Process Builder and for the portal subscribers via the Kaleo Forms Admin application.

### Uploading a New Workflow Definition <a href="#uploading-a-new-workflow-definition" id="uploading-a-new-workflow-definition"></a>

Workflow process definitions can be built in the portal or uploaded in `XML` format. Portal workflow definitions are written in `XML` format; see the Introduction to Crafting XML Workflow Definitions to learn more.

To upload a new definition:

1. Navigate to the _Control Panel_ → _Workflow_ → _Process Builder_.
2. Click the _New Workflow_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3. If you have a local XML definition file based on an embedded workflow, you can upload it to the Portal. Click the _Source_ tab.
4. Enter a name for the new definition.
5.  Copy the content of the XML file and and overwrite the sample code or click “_import a file_” to upload the file.

    ![Adding a new definition by uploading one](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/managing-workflows/images/01.png)
6. Click the _Save_ button.
7. Click the _Publish_ button.

The newly added definition is ready to be used as a workflow process.

### Editing a Workflow Definition <a href="#editing-a-workflow-definition" id="editing-a-workflow-definition"></a>

To edit a workflow:

1. Navigate to the _Control Panel_ → _Workflow_ → _Process Builder_.
2.  Click the _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the workflow then _Edit_.

    ![Editing a definition](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/managing-workflows/images/03.png)

Changes to Workflow Definitions are tracked in the _Details_ and _Revision History_ menus.

#### Viewing and Restoring Workflow Revisions <a href="#viewing-and-restoring-workflow-revisions" id="viewing-and-restoring-workflow-revisions"></a>

To view the workflow definition details and the revision history:

1. Click the _Information_ button (![Information](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png))
2.  Next to a previous version on the _Revision History_ tab, click the _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select either _Preview_ or _Restore_.

    ![View and restore prior versions of a workflow.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/managing-workflows/images/02.png)
3. When you click _Restore_ and see the success message, the prior version is the current version of the workflow. You can now edit the restored version of the workflow.
4. If edits are necessary, edit and click _Update_. This creates another version of the workflow.

### Deleting a Workflow Definition <a href="#deleting-a-workflow-definition" id="deleting-a-workflow-definition"></a>

There are two statuses for a workflow:

* **Published:** Validation is complete, and the workflow can be assigned to assets.
* **Unpublished:** Validation is not performed on the unpublished workflow, and it cannot be assigned to assets until it is published.

Only _Unpublished_ workflows can be deleted.

_Published_ workflow definitions cannot be deleted if they are current associated with an asset type. Users have to first deactivate the workflow process and then place the workflow in the _Unpublished_ status.

1. To deactivate an asset type’s workflow, reverse the steps found in the [Activating Workflow](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/activating-workflow) article for each asset type and select _No Workflow_.
2. Navigate to the _Control Panel_ → _Workflow_ → _Process Builder_.
3.  Click the _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the workflow then _Unpublish_.

    ![Unpublishing a Workflow](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/managing-workflows/images/04.png)
4.  The selected definition is now in the _Unpublished_ section. Click the _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the workflow then _Delete_.

    ![Deleting a Workflow](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/managing-workflows/images/05.png)
