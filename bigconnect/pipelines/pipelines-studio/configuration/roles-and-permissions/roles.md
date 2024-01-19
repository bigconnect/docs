# Roles

Roles enable you to perform BigConnect Pipelines tasks. Every user requires at least one role to access BigConnect Pipelines.

When using file-based authentication, you must assign at least one role to each user.

When using LDAP authentication, you can assign roles to users or groups. If the user belongs to a group, any roles associated with the group are granted to the user in addition to the roles assigned directly to the user.

BigConnect Pipelines provides the following roles:

| Role    | Tasks                                                                                                                                                                                                                                                                                  |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| admin   | Perform any BigConnect Pipelines task. Can perform all tasks listed below, as well as activate BigConnect Pipelines, restart and shut down BigConnect Pipelines, and view BigConnect Pipelines. Enable Control Hub. Install libraries using Package Manager. Generate support bundles. |
| manager | Start and stop pipelines, monitor pipelines, configure and reset alerts. Take, review, and manage snapshots.                                                                                                                                                                           |
| creator | Create and configure pipelines and alerts, preview data, and monitor the pipeline. Import pipelines.                                                                                                                                                                                   |
| guest   | View pipelines and alerts, and general monitoring information. Export a pipeline.                                                                                                                                                                                                      |
