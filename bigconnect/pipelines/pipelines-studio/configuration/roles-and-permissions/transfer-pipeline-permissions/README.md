# Transfer Pipeline Permissions

You can transfer pipeline permissions from a user or group to another user or group. When you transfer permissions, all pipeline permissions are passed to the target user or group. Pipeline ownership transfers only from a user to another user.

Transfer permissions when a user account or group becomes obsolete, such as when a user leaves the company or when you register BigConnect Pipelines with BigConnect Pipelines Control Hub. You might also transfer permissions when a user changes positions within the company.

For example, say a JD user account belongs to a pipeline developer who has created several pipelines and is now transitioning to operations. As the pipeline creator, JD has full rights to the pipelines that she created. The pipelines are about to go into production, so she needs read and execute permission to run the pipelines, but she should no longer be able to edit them.

To handle this situation, you can transfer all permissions associated with the JD account to another development user or a development group. Then assign JD to an Ops group with the Manager role and assign the Ops group the read and execute permissions for the pipelines they need to run. Or, without an Ops group, you simply assign the Manager role to the JD user account and edit the pipeline permissions to grant read and execute permissions to JD.

When you transfer permissions, the Transfer User and Group Permissions dialog box lists any users or groups that no longer exist but are still associated with pipeline permissions. This allows you to easily transfer permissions from obsolete users and groups.
