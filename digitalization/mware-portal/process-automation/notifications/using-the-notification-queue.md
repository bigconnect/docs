# Using the Notification Queue

Liferay provides the Notification Queue application for reviewing email notifications triggered by an Object action. You can verify the status of notifications and resend them if needed.

![Review email notifications in the Notification Queue application.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/using-the-notification-queue/images/01.png)

The application lists the following details for each notification:

| Column     | Description                                                                                                                                                                                                                  |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Subject    | The notification’s subject, which is set in the [notification template](https://learn.liferay.com/web/guest/w/dxp/process-automation/notifications/creating-notification-templates).                                         |
| Trigger By | The Object responsible for triggering the [notification action](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions/defining-object-actions#notification-actions). |
| From       | The notification’s sender, which is set in the [notification template](https://learn.liferay.com/web/guest/w/dxp/process-automation/notifications/creating-notification-templates).                                          |
| To         | The notification’s recipients, which are set in the [notification template](https://learn.liferay.com/web/guest/w/dxp/process-automation/notifications/creating-notification-templates).                                     |
| Status     | Indicates the notification status, whether it was sent (_Sent_) or is in the process of sending (_Unsent_).                                                                                                                  |

Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for an entry to _delete_ it or _resend_ the notification.

![Delete entries or resend notifications.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/using-the-notification-queue/images/02.png)

### [Clearing the Notification Queue](https://learn.liferay.com/web/guest/w/dxp/process-automation/notifications/using-the-notification-queue#clearing-the-notification-queue) <a href="#clearing-the-notification-queue" id="clearing-the-notification-queue"></a>

By default, Liferay clears the notification queue every `43200` minutes (i.e., 30 days). This deletes all notifications in the queue, regardless of status.

To configure the deletion interval,

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Control Panel_ tab, and click _System Settings_.
2. Under Platform, click _Notifications_.
3.  Under Notification Queue, enter the number of minutes between deletion operations.

    The interval must be greater than `15` minutes.

    ![Under Notification Queue, enter the number of minutes between deletion operations.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/notifications/using-the-notification-queue/images/03.png)
4. Click _Save_.

\
