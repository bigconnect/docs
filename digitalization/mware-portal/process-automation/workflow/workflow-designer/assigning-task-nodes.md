# Assigning Task Nodes

Task nodes require interaction by people. To alert and associate tasks with the right people, use notifications and assignments, respectively. You can assign task nodes to

* Specific roles
* Multiple roles of a role type (organization, site, asset library, account, or regular role types)
* The content creator
* Resource actions
* Specific users

Additionally, you can write a script to define the assignment (see Scripted Assignments).

![You can add an assignment to a task node.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/workflow-designer/assigning-task-nodes/images/01.png)

### Resource Action Assignments <a href="#resource-action-assignments" id="resource-action-assignments"></a>

You can assign a workflow task to a _resource action_. Resource actions are operations performed by users on an application or entity. For example, a user might have permission to update message boards messages. This is called an UPDATE resource action, because the user can update the resource. If your workflow definition specifies the UPDATE action in an assignment, anyone with permission to update the asset type being processed in the workflow is assigned to the task. You can configure multiple assignments for a task.

Here’s what the assignment looks like in the source (XML) view:

```xml
<assignments>
    <resource-actions>
        <resource-action>UPDATE</resource-action>
    </resource-actions>
</assignments>
```

You can assign the workflow to the appropriate workflow enabled asset.

Now when the workflow proceeds to the task with the resource action assignment, Users with `UPDATE` permission on the resource (for example, message boards messages) are notified of the task and can assign it to themselves (if the notification is set to task assignees). Specifically, users see the tasks in their _My Workflow Tasks_ application under the tab _Assigned to My Roles_.

NOTE

The My Workflow Tasks application is accessible from a user’s personal menu:

![My Workflow Tasks is where users manage content in the workflow.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/workflow-designer/assigning-task-nodes/images/02.png)

Use all upper case letters for resource action names. To enter more than one, use a comma-separated list. Here are some common resource actions:

* UPDATE
* ADD
* DELETE
* VIEW
* PERMISSIONS
* SUBSCRIBE
* ADD\_DISCUSSION

You can determine the probable resource action name from the permissions screen for that resource. For example, in message boards, one of the permissions displayed on that screen is _Add Discussion_. Convert that to all uppercase and replace the space with an underscore, and you have the action name.

### Scripted Assignments <a href="#scripted-assignments" id="scripted-assignments"></a>

You can use a script to manage the assignment. Set a single user in the `user` variable, or add users to a list in the `users` variable. To specify role assignments, add a list of roles (even if there’s just one) to the `roles` variable.

In the XML source, scripted assignments are written in the `<scripted-assignment>` XML element:

```xml
<assignments>
   <scripted-assignment>
      <script>
         <![CDATA[
            ...
         ]]>
      </script>
      <script-language>groovy</script-language>
   </scripted-assignment>
</assignments>
```

#### Assigning to Roles <a href="#assigning-to-roles" id="assigning-to-roles"></a>

The script for the review task assignment in the Scripted Single Approver workflow definition (`single-approver-scripted-assignment-workflow-definition.xml`) sets `roles`:

```groovy
import com.liferay.portal.kernel.model.Group;
import com.liferay.portal.kernel.model.Role;
import com.liferay.portal.kernel.service.GroupLocalServiceUtil;
import com.liferay.portal.kernel.service.RoleLocalServiceUtil;
import com.liferay.portal.kernel.util.GetterUtil;
import com.liferay.portal.kernel.workflow.WorkflowConstants;

long companyId = GetterUtil.getLong((String)workflowContext.get(WorkflowConstants.CONTEXT_COMPANY_ID));

long groupId = GetterUtil.getLong((String)workflowContext.get(WorkflowConstants.CONTEXT_GROUP_ID));

Group group = GroupLocalServiceUtil.getGroup(groupId);

roles = new ArrayList<Role>();

Role adminRole = RoleLocalServiceUtil.getRole(companyId, "Administrator");

roles.add(adminRole);

if (group.isOrganization()) {
    Role role = RoleLocalServiceUtil.getRole(companyId, "Organization Content Reviewer");

    roles.add(role);
}
else {
    Role role = RoleLocalServiceUtil.getRole(companyId, "Site Content Reviewer");

    roles.add(role);
}
```

This script assigns the task to the administrator role, then checks if the asset’s group is an organization. If so, the organization content reviewer role is added. If not, the site content reviewer role is added.

Note the `roles = new ArrayList<Role>();` line. In a scripted assignment, the `roles` variable is where you assign roles to tasks. For example, when `roles.add(adminRole);` is called, the administrator role is added to the assignment.

The `roles` variable assignment defines the assignable candidates for the task. Once the entry is submitted to the workflow, one of the candidate users must assign the task to themselves. See Reviewing Assets for more information.

#### Assigning to Users <a href="#assigning-to-users" id="assigning-to-users"></a>

Assign a user or list of users to a task. These assignments appear in the My Workflow Tasks application under Assigned to Me. To assign a list of users as possible candidates, use code like this:

```groovy
import com.liferay.portal.kernel.service.UserLocalServiceUtil;
import com.liferay.portal.kernel.util.GetterUtil;
import com.liferay.portal.kernel.workflow.WorkflowConstants;

users = new ArrayList();

long companyId = GetterUtil.getLong((String)workflowContext.get(WorkflowConstants.CONTEXT_COMPANY_ID));

users.add(UserLocalServiceUtil.getUserByEmailAddress(companyId, "user1@liferay.com"));
users.add(UserLocalServiceUtil.getUserByEmailAddress(companyId, "user2@liferay.com"));
```

The `users` variable assignment, like `roles`, defines the assignable candidates for the task. Once the entry is submitted to the workflow, one of the candidate users must assign the task to themselves. See Reviewing Assets for more information.

To assign a single user to the task,

```groovy
import com.liferay.portal.kernel.service.UserLocalServiceUtil;
import com.liferay.portal.kernel.util.GetterUtil;
import com.liferay.portal.kernel.workflow.WorkflowConstants;

long companyId = GetterUtil.getLong((String)workflowContext.get(WorkflowConstants.CONTEXT_COMPANY_ID));

user = UserLocalServiceUtil.getUserByEmailAddress(companyId, "user1@liferay.com");
```

In this case, the `user` variable assignment is immediate since there is only one candidate.

