# Pipeline Permissions

Pipeline permissions determine the access that users have to a pipeline. The owner of the pipeline and users with the Admin role have full access to a pipeline. As a pipeline owner or a user with the Admin role, you can also assign pipeline permissions to individual users and to groups.

To perform pipeline-related tasks, you must have the appropriate pipeline permissions as well as the role associated with the task. For example, a user with the Guest role can only view a pipeline when granted read permission for it. Similarly, to edit a pipeline with a Creator role, you need both read and write permission on the pipeline.

To easily enable pipeline access to sets of users, grant permissions to groups. For example, say you have five users who require read and execute access to run several pipelines. To allow this, you must share each pipeline with each of these users. But if the users are in a single Operations group, you can simply assign read and execute access to the Operations group instead of assigning the permissions to each user individually.

To use pipeline permissions, enable the pipeline.access.control.enabled BigConnect Pipelines configuration property, and configure the permissions on a pipeline-by-pipeline basis.

**Note**: When enabled, the pipeline owner and users with the Admin role have full access to a pipeline, and other users have no access.

By default, the pipeline.access.control.enabled property is disabled. When pipeline permissions are disabled, access to pipelines is based on the roles assigned to the user and its groups.

You can configure the following pipeline permissions:

| Permission | Description                                                                 |
| ---------- | --------------------------------------------------------------------------- |
| Read       | View and monitor the pipeline, and see alerts. View existing snapshot data. |
| Write      | Edit the pipeline and alerts.                                               |
| Execute    | Start and stop the pipeline. Preview data and take a snapshot.              |

For details about sharing pipelines with users and groups, see Sharing a Pipeline.
