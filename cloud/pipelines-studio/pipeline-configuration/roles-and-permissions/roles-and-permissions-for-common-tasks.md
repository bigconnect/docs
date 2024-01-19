# Roles and Permissions for Common Tasks

Performing BigConnect Pipelines tasks generally requires a combination of roles and permissions. The following table outlines the requirements to perform common tasks.

**Note**: The pipeline owner and users with the Admin role can perform any pipeline-related task regardless of permissions.

| Task                                                          | Role             | Pipeline Permissions |
| ------------------------------------------------------------- | ---------------- | -------------------- |
| View a pipeline                                               | Any              | Read                 |
| Create a pipeline                                             | Creator or Admin | None                 |
| Edit a pipeline                                               | Creator or Admin | Read and write       |
| Preview a pipeline                                            | Creator or Admin | Read and execute     |
| Start and stop a pipeline                                     | Manager or Admin | Read and execute     |
| View existing snapshot data                                   | Manager or Admin | Read                 |
| Monitor a pipeline and take snapshots                         | Manager or Admin | Read and execute     |
| Duplicate a pipeline                                          | Creator or Admin | Read                 |
| Import a pipeline to a new pipeline                           | Creator or Admin | None                 |
| Import a pipeline to an existing pipeline                     | Creator or Admin | Read and Write       |
| Export a pipeline                                             | Any              | Read                 |
| Share a pipeline and configure permissions                    | Admin            | None                 |
| View pipeline permissions                                     | Any              | Read                 |
| Start or stop BigConnect Pipelines                            | Admin            | Not applicable       |
| View log data and BigConnect Pipelines metrics                | Admin            | Not applicable       |
| Install or uninstall stage libraries with the Package Manager | Admin            | Not applicable       |
