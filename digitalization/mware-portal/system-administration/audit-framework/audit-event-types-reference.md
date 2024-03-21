# Audit Event Types Reference

The `EventTypes` class lists the different event categories logged by the audit framework. It specifies the action performed on the resource listed in the audit application. There are 10 event types logged by the audit framework.

| Event Type     | Supported Resources                                   | Description                                                                               |
| -------------- | ----------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| ADD            | Organization, Role, User, UserGroup                   | The listed resource has been added.                                                       |
| ASSIGN         | Organization, Role, User, UserGroup                   | The resource has been assigned to something; for example, a role assigned to a user.      |
| DELETE         | Organization, Role, User, UserGroup                   | The listed resource was deleted.                                                          |
| IMPERSONATE    | User                                                  | A user was impersonated.                                                                  |
| LOGIN          | User                                                  | A user has logged in.                                                                     |
| LOGIN\_DNE     | User                                                  | The username does not exist.                                                              |
| LOGIN\_FAILURE | User                                                  | A login failed.                                                                           |
| LOGOUT         | User                                                  | A user has logged out.                                                                    |
| UNASSIGN       | Organization, Role, User, UserGroup                   | The resource has been unassigned from something; for example, a role removed from a user. |
| UPDATE         | Address, Contact, Organization, Role, User, UserGroup | The listed resource was updated.                                                          |
