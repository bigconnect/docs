# Managing Workflows

Like other resources, workflow definitions can be added, edited, and deleted. But that’s just the beginning of workflow management.

* Because workflow definitions can be complex works in progress, they can be versioned.
* Unpublished drafts can be saved.
* Because workflow definitions are XML files, they’re portable. Thus, they can also be uploaded.

Start by learning basic workflow management.

### Workflow Definition Publication Permissions <a href="#workflow-definition-publication-permissions" id="workflow-definition-publication-permissions"></a>

Users with permission to edit or publish workflow definitions can add Groovy scripts to the workflow. Access to the scripting engine means access to the Java Virtual Machine (JVM) of the server. Users who publish (or edit) workflow definitions containing scripts, therefore, can get access to any data within the reach of the JVM, such as data contained in a separate Virtual Instance of the portal itself.

Because of this far-reaching access, permission to create or edit workflow definitions is limited to Regular Administrators of the Default Virtual Instance.

To grant Users with these Roles the workflow publication access in additional Virtual Instances,

1. Make sure you understand the access you’re granting these admins.
2. Navigate to Control Panel → System Settings → Workflow → Workflow Definition.
3. Check the box for the setting _Allow Administrators to Publish and Edit Workflows_.

This only applies to Virtual Instances that have been added to the system. The Default Virtual Instance provides workflow publication access to Regular Administrators (via Control Panel → Configuration → Workflow), and, if running the portal, to Site Administrators and other Users with access to the Kaleo Forms Admin applications.

### Adding, Editing, and Deleting <a href="#adding-editing-and-deleting" id="adding-editing-and-deleting"></a>

To add a workflow definition,

1. Go to Control Panel → Workflow → Process Builder.
2. In the default view (Workflows), click the Add button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360029928832/icon-add.png)).
3. From here you’re either writing an XML definition, designing a definition in in the visual designer, or uploading an existing definition.

To edit a definition,

1. Go to Control Panel → Workflow → Process Builder.
2. Click the _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029928792/icon-actions.png)) for the workflow, and click _Edit_.

To delete a definition,

1. Go to Control Panel → Workflow → Process Builder.
2.  Click the _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029928792/icon-actions.png)) for the workflow, and click _Unpublish_.

    A published workflow cannot be deleted, so you must unpublish its workflow definition first.

    You can’t unpublish a definition if it’s activated for an asset. First dissociate the workflow definition from any assets that use it. See [here](https://help.liferay.com/hc/en-us/articles/360029042051-Activating-Workflow) for more information.

### Uploading Workflow Definitions <a href="#uploading-workflow-definitions" id="uploading-workflow-definitions"></a>

If you have a local XML definition file (perhaps you want to create a new workflow based on one of the embedded workflows), upload it to the portal:

1. Navigate to Control Panel → Workflow → Process Builder.
2. Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360029928832/icon-add.png)).
3. Name the workflow.
4.  In the _Source_ tab, click the hyperlink _import a file_ in the sentence

    `Write your definition or import a file`
5. Find the XML file and upload it. Once uploaded, the definition’s XML appears in the workflow editor.
6. If it’s ready to publish, click _Publish_. Otherwise, _Save_ it and it stays Unpublished.

What’s the difference between saving and publishing?

### Published Versus Unpublished <a href="#published-versus-unpublished" id="published-versus-unpublished"></a>

The difference between a published and unpublished workflow is important:

**Published:** Validation is complete, and the workflow can be assigned to assets.

**Unpublished:** Validation is not performed on the unpublished workflow, and it cannot be assigned to assets until it’s published.

### Workflow Versions <a href="#workflow-versions" id="workflow-versions"></a>

You’re making a simple edit to a workflow, when suddenly you remember you have a meeting with your boss. Quickly you save the workflow and hurry off to your meeting. Congratulations! You were promoted to Director of Business Productivity! You have no time to edit workflows now, so your colleague must finish editing and publishing the workflow. Unfortunately, in all the excitement of your promotion, you forgot what you changed in the workflow. It’s best to revert to the prior version and start editing it from scratch.

1. Open the workflow editor. Go to _Control Panel_ → _Workflow_ → _Process Builder_, and select the workflow from the list (click its title to open the editor).
2. Click the _Information_ button (![Information](https://liferay-support.zendesk.com/hc/article\_attachments/360029912591/icon-information.png))
3.  There are two information panel sections: Details and Revision History.

    The Details screen shows information about the creation of the workflow and its last modification, and a summary of the total modifications.

    The Revision History screen shows the workflow’s current and prior, restorable versions. To view an old workflow or to restore it if you’re sure it’s the right version, click the _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029928792/icon-actions.png)) and select either _Preview_ or _Restore_.
4. When you click _Restore_ and see the success message, the prior version is the current version of the workflow. You can now edit the restored version of the workflow.
5. If edits are necessary, edit and click _Update_. This creates another version of the workflow.

![Figure 1: View and restore prior versions of a workflow.](https://liferay-support.zendesk.com/hc/article\_attachments/360029928812/workflow-revisions.png)

Alternatively, you can refer to the embedded definitions to get workflow definition ideas.
