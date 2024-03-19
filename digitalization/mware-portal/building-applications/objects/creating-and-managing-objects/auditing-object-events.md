# Auditing Object Events

You can use Liferay’s audit framework to track events for custom object definitions and their entries. To track entry events, you must [enable entry history](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/auditing-object-events#enabling-entry-history) for the object definition. While enabled, you can view an entry’s history via the [Audit application](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/auditing-object-events#using-the-audit-application) or [REST APIs](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/auditing-object-events#using-rest-apis).

### [Using the Audit Application](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/auditing-object-events#using-the-audit-application) <a href="#using-the-audit-application" id="using-the-audit-application"></a>

Subscription

To access the Audit application, open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) → _Control Panel_ → click _Audit_.

![Click Audit in the Control Panel to view events related to object definitions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/auditing-object-events/images/01.png)

Events appear in a table:

| Column          | Description                                                                  |
| --------------- | ---------------------------------------------------------------------------- |
| User ID         | Acting user’s ID.                                                            |
| User Name       | Acting user’s name.                                                          |
| Resource ID     | Affected resource’s ID.                                                      |
| Resource Name   | Affected resource’s name, including its ID (e.g., `ObjectDefinition#44168`). |
| Resource Action | Action performed (`ADD`, `UPDATE`, or `DELETE`).                             |
| Client IP       | IP address responsible for the action.                                       |
| Create Date     | When the event record was created.                                           |

Click an event field to view a more complete record, including specific action details. You can search and filter results using keywords and other terms.

Object entry events use the `ObjectDefinition#[ObjectID]` naming pattern.

![Click an event field to view more details.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/auditing-object-events/images/02.png)

### [Using REST APIs](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/auditing-object-events#using-rest-apis) <a href="#using-rest-apis" id="using-rest-apis"></a>

You can use the `nestedFields` parameter with the `auditEvents` value to return an entry’s history in GET API calls (e.g., `http://localhost:8080/o/c/tickets/?nestedFields=auditEvents`). See [Using nestedFields to Audit Entry History](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/understanding-object-integrations/using-custom-object-apis/using-nestedfields-to-audit-entry-history) for a basic tutorial.

IMPORTANT

Using REST APIs to view an entry’s history requires both the `View` and `Object Entry History` permissions for the entry. See [Permissions Framework Integration](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/understanding-object-integrations/permissions-framework-integration) for more information.

### [Enabling Entry History](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/auditing-object-events#enabling-entry-history) <a href="#enabling-entry-history" id="enabling-entry-history"></a>

By default, entry history is disabled for custom objects. For 7.4 U72+/GA72+, you can configure this setting at any time. In earlier versions, you can only configure this setting before [publishing the object definition](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#publishing-object-drafts).

To enable entry history,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.
2. Begin editing the desired object definition.
3.  In the Details tab, toggle _Enable Entry History_.

    ![Enable entry history.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/auditing-object-events/images/03.png)
4. Click _Save_.

While enabled, Liferay keeps a history of entry events that you can view in the Audit application or via REST API calls.

\
