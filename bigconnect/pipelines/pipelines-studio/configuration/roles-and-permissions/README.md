# Roles and Permissions

BigConnect Pipelines allows you to assign roles and pipeline permissions to users and groups.

Roles, such as Creator or Manager, enable users to perform different BigConnect Pipelines tasks. Each user needs at least one role to access BigConnect Pipelines.

Permissions determine read, write, and execute access to individual pipelines. Users generally need a combination of roles and permissions to perform pipeline-related tasks. For example, to edit a pipeline, you need the Creator role as well as read and write permission for the pipeline.

Use groups to easily assign pipeline permissions to sets of users. When using LDAP authentication, you can also assign roles to LDAP groups. When a user belongs to a group, the user has the combination of the access assigned to the group and to the user individually.

When necessary, you can transfer permissions from a user or group to another user or group.

Note that users with the Admin role have full access to every pipeline. And the owner of a pipeline has full access to the pipeline. These users can also change who owns the pipeline.
