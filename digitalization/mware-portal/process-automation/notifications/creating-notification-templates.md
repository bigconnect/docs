# Creating Notification Templates

With Notification Templates, you can design automated email and user notifications for Liferay services. Each template identifies a sender, recipients, and message content. While designing the template, you can search supported entities and add field references that insert their data dynamically into emails.

Currently, notification templates only support object actions. However, Commerce Channels include built-in notification template capabilities for channel events. See Store Emails for more information.

IMPORTANT

To use email notifications, your instance must have a configured mail server. Once configured, you can access additional email notification settings at the instance level.

If the out-of-the-box notification types don’t meet your needs, you can create custom notification types using client extensions. See Microservice Client Extensions for more information.

### Adding Email Notification Templates <a href="#adding-email-notification-templates" id="adding-email-notification-templates"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Notification Templates_.
2.  Click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select _Email_.

    ![Click the Add button and select Email.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/creating-notification-templates/images/01.png)

    You can use field references to populate messages dynamically with entry and user data. In the Definition of Terms section you can select an object definition to reference its supported fields. In the General Terms section you can find terms for referencing fields for the user who triggers the notification action. Use these reference terms in the template fields as desired. See Definition of Terms Reference for more information.

    ![Search entities to add references to supported fields.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/creating-notification-templates/images/02.png)
3.  In Basic Info, enter a name and description for the template.

    ![Under Basic Info, enter a name and description.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/creating-notification-templates/images/03.png)
4.  In Settings, enter contact information for the email sender and recipients.

    To add multiple recipients, separate email addresses with a comma or space (e.g., `test@liferay.com,learn@liferay.com`, `[%TICKET_AUTHOR_EMAIL_ADDRESS%] [%CURRENT_USER_EMAIL_ADDRESS%]`).

    ![Under Settings, enter details for the sender and desired recipients.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/creating-notification-templates/images/04.png)
5.  Determine whether to send emails separately or together.

    When sent together, emails include the full list of recipients from the To field. Sending emails separately hides the list of recipients.

    TIP

    Sending emails together can facilitate transparency and collaboration. Sending emails separately can help preserve privacy.
6.  In Content, enter a subject, select an editor type, and create the body of the email.

    For type, select Rich Text or FreeMarker Template.

    ![Under Content, enter a subject and craft the body of the email.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/creating-notification-templates/images/05.png)
7.  (Optional) Select object attachment fields to include their assets in email notifications.

    IMPORTANT

    You can only attach assets from object entries involved in the notification action.

    ![Select attachment fields to include assets in email notifications.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/creating-notification-templates/images/06.png)
8. Click _Save_.

Once created, you can use notification templates with object actions. See [Defining Object Actions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions/defining-object-actions) for more information.

### Adding User Notification Templates <a href="#adding-user-notification-templates" id="adding-user-notification-templates"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Notification Templates_.
2.  Click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select _User Notification_.

    ![Click the Add Button and select User Notification.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/creating-notification-templates/images/07.png)

    You can use field references to populate messages dynamically with entry and user data. In the Definition of Terms section you can select an object definition to reference its supported fields. In the General Terms section you can find terms for referencing fields for the user who triggers the notification action. Use these reference terms in the template fields as desired. See [Definition of Terms Reference](https://learn.liferay.com/web/guest/w/dxp/process-automation/notifications/creating-notification-templates#definition-of-terms-reference) for more information.

    ![Search entities to add references to supported fields.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/creating-notification-templates/images/08.png)
3.  In Basic Info, enter a name and description for the template.

    ![Under Basic Info, enter a name and description.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/creating-notification-templates/images/09.png)
4.  In Settings, determine the notification’s recipients.

    You can use defined terms, user names, or user roles. To add multiple recipients, separate values with a comma or space (e.g., `[%TICKET_AUTHOR_ID%]`, `[%TICKET_R_ASSIGNEE_USERID%]`).

    IMPORTANT

    If you set the recipients dynamically, only users with [permission to add object entries](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/understanding-object-integrations/permissions-framework-integration#creating-entries) can receive the notification.

    ![Determine recipients by entering terms, individual user names, or user roles.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/creating-notification-templates/images/10.png)
5.  In the Content section, enter the notification’s message.

    You can use defined terms to populate values dynamically.

    ![Enter the notification's message in the Content section.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/creating-notification-templates/images/11.png)
6. Click _Save_.

Once created, you can use notification templates with object actions. See [Defining Object Actions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions/defining-object-actions) for more information.

### Definition of Terms Reference <a href="#definition-of-terms-reference" id="definition-of-terms-reference"></a>

You can use reference terms to access user and entry data, dynamically populating a template’s fields. You can reference any entries and users involved in the event triggering the [notification action](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions/understanding-action-types#notification). You can also reference fields in related object entries if the current entry is on the child side of a one-to-many relationship. References to related entry fields use this pattern, `[%RELATIONSHIPNAME_PARENTOBJECTNAME_OBJECTFIELD%]`.

Consider a scenario with a custom Ticket object that is on the child side of a one-to-many relationship with the User system object. This relationship determines the ticket’s assignee. If you add a notification action to the Ticket object for notifying the assignee on entry update, the template used for this action can access data for both the updated ticket and the assignee.

Terms are available for both custom and default object fields.

IMPORTANT

You can only reference fields for entries and users directly involved in the event that triggers the notification action.

#### Default Terms for Current Entry <a href="#default-terms-for-current-entry" id="default-terms-for-current-entry"></a>

Use these terms to access information about the current object entry.

| Label                   | Term                                   |
| ----------------------- | -------------------------------------- |
| Author Email Address    | `[%OBJECTNAME_AUTHOR_EMAIL_ADDRESS%]`  |
| Author First Name       | `[%OBJECTNAME_AUTHOR_FIRST_NAME%]`     |
| Author ID               | `[%OBJECTNAME_AUTHOR_ID%]`             |
| Author Last Name        | `[%OBJECTNAME_AUTHOR_LAST_NAME%]`      |
| Author Middle Name      | `[%OBJECTNAME_AUTHOR_MIDDLE_NAME%]`    |
| Author Prefix           | `[%OBJECTNAME_AUTHOR_PREFIX%]`         |
| Author Suffix           | `[%OBJECTNAME_AUTHOR_SUFFIX%]`         |
| Create Date             | `[%OBJECTNAME_CREATEDATE%]`            |
| External Reference Code | `[%OBJECTNAME_EXTERNALREFERENCECODE%]` |
| ID                      | `[%OBJECTNAME_ID%]`                    |
| Modified Date           | `[%OBJECTNAME_MODIFIEDDATE%]`          |
| Status                  | `[%OBJECTNAME_STATUS%]`                |

#### Default Terms for Current User <a href="#default-terms-for-current-user" id="default-terms-for-current-user"></a>

Use these terms to access information about the current user triggering the notification action.

| Label                      | Term                             |
| -------------------------- | -------------------------------- |
| Current User Email Address | `[%CURRENT_USER_EMAIL_ADDRESS%]` |
| Current User First Name    | `[%CURRENT_USER_FIRST_NAME%]`    |
| Current User Prefix        | `[%CURRENT_USER_PREFIX%]`        |
| Current User Last Name     | `[%CURRENT_USER_LAST_NAME%]`     |
| Current User Middle Name   | `[%CURRENT_USER_MIDDLE_NAME%]`   |
| Current User ID            | `[%CURRENT_USER_ID%]`            |
| Current User Suffix        | `[%CURRENT_USER_SUFFIX%]`        |
