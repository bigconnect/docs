# Event reference

Mware Application Development Framework produces a number of events when operations of interest occur within the application. For example when a new app is created an `app:created` event is created, when the app is published an `app:published` event is created. Events are used for a number of purposes, such as audit logging and analytics that help create a better product for our users.

### Identities

Every event has an associated identity to indicate who or what triggered the event.

**User**

An event triggered within the context of an authenticated user.

Example: An API request

A user has attached metadata:

JSON

```json
{
   "accountHolder": false,
   "admin": false,
   "builder": true,
   "environment": "docker-compose",
   "hosting": "self",
   "id": "us_2d714b1745b1-40d7b36f539e214d8170",
   "type": "user",
   "installationId": "2d714b1745b1-40d7b36f539e214d8170",
   "tenantId": "2d714b1745b1-40d7b36f539e214d8170_default",
   "verified": true
}
```

**Tenant**

An event triggered as a result of a background process, within the scope of a single tenant.

Example: An Automation

A tenant has attached metadata:

JSON

```json
{
   "id": "acmecorp",
   "type": "tenant",
   "hosting": "cloud",
   "environment": "kubernetes",
   "installationId": "2d714b1745b1-40d7b36f539e214d8170",
   // Cloud Only
   "profession": "software_engineer", 
   "companySize": "1+" 
}
```

**Installation**

An event triggered as a result of a background process, outside the scope of any one tenant.

Example: Version updated.

An installation has attached metadata:

JSON

```json
{
   "id": "2d714b1745b1-40d7b36f539e214d8170",
   "type": "installation",
   "hosting": "cloud",
   "environment": "kubernetes",
   "version": "1.0.203"
}
```

### Event properties

There are some properties that are included in every event:

**version**

The software version at the time of sending the event.

Example: `1.0.201`

**service**

The service that sent the event.

Example: `app-service`

**environment**

The environment of the budibase instance.

Example: `docker-compose`

**appId (optional)**

The current App ID relating to the event.

Example: `app_2d714b1745b1-40d7b36f539e214d8170`

**installationId**

The globally unique Installation ID.

Example: `2d714b1745b1-40d7b36f539e214d8170`

**tenantId**

The globally unique Tenant ID.

Example: `2d714b1745b1-40d7b36f539e214d8170_default`

**hosting**

Indicator of the hosting type. The value can be either `self` for self hosted installs or `cloud` for the budibase cloud environment.

### Types of events

All events that are currently produced are listed below. In addition to the common event properties from above each event may also contain additional properties.

#### User

* `user:created`

JSON

```json
{
   "userId": "string"
}
```

* `user:updated`

JSON

```json
{
   "userId": "string"
}
```

* `user:deleted`

JSON

```json
{
   "userId": "string"
}
```

* `user:admin:assigned`

JSON

```json
{
   "userId": "string"
}
```

* `user:admin:removed`

JSON

```json
{
   "userId": "string"
}
```

* `user:builder:assigned`

JSON

```json
{
   "userId": "string"
}
```

* `user:builder:removed`

JSON

```json
{
   "userId": "string"
}
```

* `user:invited`
* `user:invite:accepted`

JSON

```json
{
   "userId": "string"
}
```

* `user:password:force:reset`

JSON

```json
{
   "userId": "string"
}
```

* `user:password:updated`

JSON

```json
{
   "userId": "string"
}
```

* `user:password:reset:requested`

JSON

```json
{
   "userId": "string"
}
```

* `user:password:reset`

JSON

```json
{
   "userId": "string"
}
```

#### Email

* `email:smtp:created`
* `email:smtp:updated`

#### Auth

* `auth:sso:created`

JSON

```json
{
   "type": "string" // "oidc", "google"
}
```

* `auth:sso:updated`

JSON

```json
{
   "type": "string" // "oidc", "google"
}
```

* `auth:sso:activated`

JSON

```json
{
   "type": "string" // "oidc", "google"
}
```

* `auth:sso:deactivated`

JSON

```json
{
   "type": "string" // "oidc", "google"
}
```

* `auth:login`

JSON

```json
{
   "userId": "string",
   "source": "string", // "local", "google", "oidc", "google-internal"
}
```

* `auth:logout`

JSON

```json
{
   "userId": "string"
}
```

#### Org

* `org:info:name:updated`
* `org:info:logo:updated`
* `org:platformurl:updated`

#### Installation

* `installation:version:checked`

JSON

```json
{
   "currentVersion": "string"
}
```

* `installation:version:upgraded`

JSON

```json
{
   "from": "string",
   "to": "string",   
}
```

* `installation:version:downgraded`

JSON

```json
{
   "from": "string",
   "to": "string",   
}
```

* `installation:firstStartup`

#### App

* `app:created`
* `app:updated`
* `app:deleted`
* `app:published`
* `app:unpublished`
* `app:template:imported`

JSON

```json
{
   "templateKey": "string"
}
```

* `app:file:imported`

JSON

```json
{
   "appId": "string"
}
```

* `app:version:updated`

JSON

```json
{
   "currentVersion": "string",
   "updatedToVersion": "string"
}
```

* `app:version:reverted`

JSON

```json
{
   "currentVersion": "string",
   "revertedToVersion": "string"
}
```

* `app:reverted`
* `app:exported`

#### Role

* `role:created`

JSON

```json
{
   "roleId": "string",
   "permssionId": "string",
   "inherits": "string"
}
```

* `role:updated`

JSON

```json
{
   "roleId": "string",
   "permssionId": "string",
   "inherits": "string"
}
```

* `role:deleted`

JSON

```json
{
   "roleId": "string",
   "permssionId": "string",
   "inherits": "string"
}
```

* `role:assigned`

JSON

```json
{
   "userId": "string",
   "roleId": "string"
}
```

* `role:unassigned`

JSON

```json
{
   "userId": "string",
   "roleId": "string"
}
```

#### Serve

* `served:builder`
* `served:app`

JSON

```json
{
   "appVersion": "string"
}
```

* `served:app:preview`

JSON

```json
{
   "appVersion": "string"
}
```

#### Datasource

* `datasource:created`

JSON

```json
{
   "datasourceId": "string",
   "source": "string" // e.g. "REST", "POSTGRES"
}
```

* `datasource:updated`

JSON

```json
{
   "datasourceId": "string",
   "source": "string" // e.g. "REST", "POSTGRES"
}
```

* `datasource:deleted`

JSON

```json
{
   "datasourceId": "string",
   "source": "string" // e.g. "REST", "POSTGRES"
}
```

#### Query

* `query:created`

JSON

```json
{
   "queryId": "string",
   "datasourceId": "string",   
   "source": "string", // e.g. "REST", "POSTGRES"
   "queryVerb": "string" // "create", "read", "update", "delete"
}
```

* `query:updated`

JSON

```json
{
   "queryId": "string",
   "datasourceId": "string",   
   "source": "string", // e.g. "REST", "POSTGRES"
   "queryVerb": "string" // "create", "read", "update", "delete"
}
```

* `query:deleted`

JSON

```json
{
   "queryId": "string",
   "datasourceId": "string",   
   "source": "string", // e.g. "REST", "POSTGRES"
   "queryVerb": "string" // "create", "read", "update", "delete"
}
```

* `query:import`

JSON

```json
{
   "datasourceId": "string",   
   "source": "string", // "REST"
   "count": "number",
   "importSource": "string" // "curl", "openapi2.0", "openapi3.0"
}
```

* `queries:run`

JSON

```json
{
   "count": "number"   
}
```

* `query:previewed`

JSON

```json
{
   "queryId": "string",
   "datasourceId": "string",   
   "source": "string", // e.g. "REST", "POSTGRES"
   "queryVerb": // "create", "read", "update", "delete"
}
```

#### Table

* `table:created`

JSON

```json
{
   "tableId": "string"
}
```

* `table:updated`

JSON

```json
{
   "tableId": "string"
}
```

* `table:deleted`

JSON

```json
{
   "tableId": "string"
}
```

* `table:exported`

JSON

```json
{
   "tableId": "string",
   "format": "string" // "json", "csv"
}
```

* `table:imported`

JSON

```json
{
   "tableId": "string",
   "format": "string" // "csv"
}
```

#### View

* `view:created`

JSON

```json
{
   "tableId": "string"
}
```

* `view:updated`

JSON

```json
{
   "tableId": "string"
}
```

* `view:deleted`

JSON

```json
{
   "tableId": "string"
}
```

* `view:exported`

JSON

```json
{
   "tableId": "string",
   "format": "string" // "json", "csv"
}
```

* `view:filter:created`

JSON

```json
{
   "tableId": "string"
}
```

* `view:filter:updated`

JSON

```json
{
   "tableId": "string"
}
```

* `view:filter:deleted`

JSON

```json
{
   "tableId": "string"
}
```

* `view:calculation:created`

JSON

```json
{
   "tableId": "string",
   "calculation": "string" // "sum", "count", "stats"
}
```

* `view:calculation:updated`

JSON

```json
{
   "tableId": "string",
   "calculation": "string" // "sum", "count", "stats"
}
```

* `view:calculation:deleted`

JSON

```json
{
   "tableId": "string",
   "calculation": "string" // "sum", "count", "stats"
}
```

#### Rows

* `rows:created`

JSON

```json
{
   "count": "number"
}
```

* `rows:imported`

JSON

```json
{
   "tableId": "string",
   "format": "string", // "csv"
   "count": "number"
}
```

#### Component

* `component:created`

JSON

```json
{
   "name": "string" // "@budibase/standard-components/cardsblock"
}
```

* `component:deleted`

JSON

```json
{
   "name": "string" // "@budibase/standard-components/cardsblock"
}
```

#### Screen

* `screen:created`

JSON

```json
{
   "screenId": "string",
   "layoutId": "string",
   "roleId": "string"
}
```

* `screen:deleted`

JSON

```json
{
   "screenId": "string",
   "layoutId": "string",
   "roleId": "string"
}
```

#### Layout

* `layout:created`

JSON

```json
{
   "layoutId" "string"
}
```

* `layout:deleted`

JSON

```json
{
   "layoutId" "string"
}
```

#### Automation

* `automation:created`

JSON

```json
{
   "automationId": "string",
}
```

`automation:deleted`

JSON

```json
{
   "automationId": "string",
}
```

`automation:tested`

JSON

```json
{
   "automationId": "string",
   "triggerId": "string",
   "triggerType": "string" // "APP", "CRON", "WEBHOOK", "ROW_UPDATED", "ROW_DELETED", "ROW_SAVED"
}
```

`automations:run`

JSON

```json
{
   "count": "number"
}
```

`automation:step:created`

JSON

```json
{
   "automationId": "string",
   "triggerId": "string",
   "triggerType": "string", // "APP", "CRON", "WEBHOOK", "ROW_UPDATED", "ROW_DELETED", "ROW_SAVED"
   "stepId": "string",
   "stepType": "string", // e.g. "SEND_EMAIL_SMTP"
}
```

`automation:step:deleted`

JSON

```json
{
   "automationId": "string",
   "triggerId": "string",
   "triggerType": "string", // "APP", "CRON", "WEBHOOK", "ROW_UPDATED", "ROW_DELETED", "ROW_SAVED"
   "stepId": "string",
   "stepType": "string", // e.g. "SEND_EMAIL_SMTP"
}
```

#### Account

* `account:created`

JSON

```json
{
   "registrationStep": "string"
}
```

* `account:deleted`
* `account:verified`
