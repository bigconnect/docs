# Audit Event Types Reference

The [`EventTypes`](https://github.com/liferay/liferay-portal/blob/7.4.3.112-ga112/modules/apps/portal-security-audit/portal-security-audit-event-generators-api/src/main/java/com/liferay/portal/security/audit/event/generators/constants/EventTypes.java) class lists the different event categories logged by the audit framework. It specifies the action performed on the resource listed in the audit application. There are 10 event types logged by the audit framework.

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
