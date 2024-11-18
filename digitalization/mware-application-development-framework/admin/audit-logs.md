# Audit logs

Audit logs are a feature that allow admin users to track events that are happening across the installation, as well as view data that are associated with these events such as the user that triggered it, or the app where it originates from.

To access the Audit logs feature you can navigate to the Account section of your application, then Audit logs.

### Viewing Audit logs

<figure><img src="https://files.readme.io/ccbbebc-image.png" alt=""><figcaption></figcaption></figure>

After navigating to the Audit logs section, you are presented with the above table, where you can monitor, filter and view more details about each of your logs.

You can see here that all of these logs are associated with the `Car Rental Admin Panel` application, however, some logs like `User created` will have no application attached to them.

You can also drill down to see more information and metadata ona particular log by clicking on the `Details`button. This will expand a side panel like the image below.

<figure><img src="https://files.readme.io/3afb50e-image.png" alt=""><figcaption></figcaption></figure>

You can see numerous pieces of metadata including the IP address, User Agent as well Application and User IDs. Each event type will have different metadata and properties associated with it.

### Filtering Audit logs

As shown in the above image, there are a number of different ways to filter your audit logs.

#### 📘 Filter types

**User**

Filter by a single or multiple users. Use the search box within the dropdown to search through all users on the system

**App**

Filter by a single or multiple apps. All apps available on the system will be displayed in the dropdown and can be searched

**Event**

Filter by single or multiple events.

**Date Range**

You can filter by any date range. The default is set to 30 days, but any range is permissible. Selecting one date will display all results from then until the current date.

**Search**

The search input allows a full text search on all of the properties within an audit log. For example, you could search for an App ID such as `app_dev_uuid1` and any audit logs that have that as a value within any property will be returned.

Any combination of these filters will work, and the filters will be applied in the order they are applied.

### Downloading your audit logs

<figure><img src="https://files.readme.io/6f92348-image.png" alt=""><figcaption></figcaption></figure>

By clicking the download button in the above image, you will be able to download all of the logs that are assosicated with your current filter(s). To download _all_ of your audit logs, please ensure that no filters are applied to the table.

### Audited events

Below you can see a list of the events that are currently audited and logged by the system.

#### User logs

| Event                               | Display Name                                          |
| ----------------------------------- | ----------------------------------------------------- |
| USER\_CREATED                       | User "\{{ email \}}" created                          |
| USER\_UPDATED                       | User "\{{ email \}}" updated                          |
| USER\_DELETED                       | User "\{{ email \}}" deleted                          |
| USER\_PERMISSION\_ADMIN\_ASSIGNED   | User "\{{ email \}}" admin role assigned              |
| USER\_PERMISSION\_ADMIN\_REMOVED    | User "\{{ email \}}" admin role removed               |
| USER\_PERMISSION\_BUILDER\_ASSIGNED | User "\{{ email \}}" builder role assigned            |
| USER\_PERMISSION\_BUILDER\_REMOVED  | User "\{{ email \}}" builder role removed             |
| USER\_INVITED                       | User "\{{ email \}}" invited                          |
| USER\_INVITED\_ACCEPTED             | User "\{{ email \}}" accepted invite                  |
| USER\_PASSWORD\_UPDATED             | User "\{{ email \}}" password updated                 |
| USER\_PASSWORD\_RESET\_REQUESTED    | User "\{{ email \}}" password reset requested         |
| USER\_PASSWORD\_RESET               | User "\{{ email \}}" password reset                   |
| USER\_GROUP\_CREATED                | User group "\{{ name \}}" created                     |
| USER\_GROUP\_UPDATED                | User group "\{{ name \}}" updated                     |
| USER\_GROUP\_DELETED                | User group "\{{ name \}}" deleted                     |
| USER\_GROUP\_USERS\_ADDED           | User group "\{{ name \}}" \{{ count \}} users added   |
| USER\_GROUP\_USERS\_REMOVED         | User group "\{{ name \}}" \{{ count \}} users removed |
| USER\_GROUP\_PERMISSIONS\_EDITED    | User group "\{{ name \}}" permissions edited          |

#### Email logs

| Event                | Display Name                |
| -------------------- | --------------------------- |
| EMAIL\_SMTP\_CREATED | Email configuration created |
| EMAIL\_SMTP\_UPDATED | Email configuration updated |

#### Authentication logs

| Event                  | Display Name                    |
| ---------------------- | ------------------------------- |
| AUTH\_SSO\_CREATED     | SSO configuration created       |
| AUTH\_SSO\_UPDATED     | SSO configuration updated       |
| AUTH\_SSO\_ACTIVATED   | SSO configuration activated     |
| AUTH\_SSO\_DEACTIVATED | SSO configuration deactivated   |
| AUTH\_LOGIN            | User "\{{ email \}}" logged in  |
| AUTH\_LOGOUT           | User "\{{ email \}}" logged out |

#### Organisation logs

| Event                       | Display Name                      |
| --------------------------- | --------------------------------- |
| ORG\_NAME\_UPDATED          | Organisation name updated         |
| ORG\_LOGO\_UPDATED          | Organisation logo updated         |
| ORG\_PLATFORM\_URL\_UPDATED | Organisation platform URL updated |

#### Application logs

| Event                   | Display Name                         |
| ----------------------- | ------------------------------------ |
| APP\_CREATED            | App "\{{ name \}}" created           |
| APP\_UPDATED            | App "\{{ name \}}" updated           |
| APP\_DELETED            | App "\{{ name \}}" deleted           |
| APP\_PUBLISHED          | App "\{{ name \}}" published         |
| APP\_UNPUBLISHED        | App "\{{ name \}}" unpublished       |
| APP\_TEMPLATE\_IMPORTED | App "\{{ name \}}" template imported |
| APP\_FILE\_IMPORTED     | App "\{{ name \}}" file imported     |
| APP\_VERSION\_UPDATED   | App "\{{ name \}}" version updated   |
| APP\_VERSION\_REVERTED  | App "\{{ name \}}" version reverted  |
| APP\_REVERTED           | App "\{{ name \}}" reverted          |
| APP\_EXPORTED           | App "\{{ name \}}" exported          |
| APP\_BACKUP\_RESTORED   | App backup "\{{ name \}}" restored   |
| APP\_BACKUP\_TRIGGERED  | App backup "\{{ name \}}" triggered  |

#### Datasource logs

| DATASOURCE\_CREATED | Datasource created |
| ------------------- | ------------------ |
| DATASOURCE\_UPDATED | Datasource updated |
| DATASOURCE\_DELETED | Datasource deleted |

#### Query logs

| Event          | Display Name  |
| -------------- | ------------- |
| QUERY\_CREATED | Query created |
| QUERY\_UPDATED | Query updated |
| QUERY\_DELETED | Query deleted |
| QUERY\_IMPORT  | Query import  |

#### Table logs

| Event                 | Display Name                  |
| --------------------- | ----------------------------- |
| TABLE\_CREATED        | Table "\{{ name \}}" created  |
| TABLE\_UPDATED        | Table "\{{ name \}}" updated  |
| TABLE\_DELETED        | Table "\{{ name \}}" deleted  |
| TABLE\_EXPORTED       | Table "\{{ name \}}" exported |
| TABLE\_IMPORTED       | Table "\{{ name \}}" imported |
| TABLE\_DATA\_IMPORTED | Data imported to table        |

#### Automation logs

| Event                     | Display Name                           |
| ------------------------- | -------------------------------------- |
| AUTOMATION\_CREATED       | Automation "\{{ name \}}" created      |
| AUTOMATION\_DELETED       | Automation "\{{ name \}}" deleted      |
| AUTOMATION\_STEP\_CREATED | Automation "\{{ name \}}" step added   |
| AUTOMATION\_STEP\_DELETED | Automation "\{{ name \}}" step removed |

#### Screen logs

| Event           | Display Name                  |
| --------------- | ----------------------------- |
| SCREEN\_CREATED | Screen "\{{ name \}}" created |
| SCREEN\_DELETED | Screen "\{{ name \}}" deleted |

#### Component logs

| Event              | Display Name      |
| ------------------ | ----------------- |
| COMPONENT\_CREATED | Component created |
| COMPONENT\_DELETED | Component deleted |

#### Environment variable logs

| Event                          | Display Name                 |
| ------------------------------ | ---------------------------- |
| ENVIRONMENT\_VARIABLE\_CREATED | Environment variable created |
| ENVIRONMENT\_VARIABLE\_DELETED | Environment variable deleted |

#### Plugin logs

| Event            | Display Name    |
| ---------------- | --------------- |
| PLUGIN\_IMPORTED | Plugin imported |
| PLUGIN\_DELETED  | Plugin deleted  |
