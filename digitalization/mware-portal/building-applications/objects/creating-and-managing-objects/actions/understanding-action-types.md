# Understanding Action Types

Actions define custom operations triggered by object entry events. The portal provides these action types:

| Type                   | Description                                                   |
| ---------------------- | ------------------------------------------------------------- |
| Notification           | Send email or user notifications using a predefined template. |
| Add an Object Entry    | Create entries in an active object.                           |
| Update an Object Entry | Update fields in the current object entry.                    |
| Webhook                | Deliver a payload to a URL.                                   |
| Groovy Script          | Execute [Groovy](https://groovy-lang.org/) scripts.           |

IMPORTANT

Groovy script actions are only available for MWARE Portal PaaS and Self-Hosted.

### Notification <a href="#notification" id="notification"></a>

Use a notification action to send user or email notifications for object entries. Example use cases include

* Automatically sending email notifications for new orders
* Automatically sending user notifications for ticket updates within a custom ticketing system
* Manually sending email notifications to remind users of tasks and events

You must select a notification template to determine the sender, recipient, and content for each message. For email notifications, your instance also requires a configured mail server.

![Select a notification template.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/understanding-action-types/images/01.png)

If the out-of-the-box notification types don’t meet your needs, you can create custom notification types using client extensions. See Microservice Client Extensions for more information.

### Add an Object Entry <a href="#add-an-object-entry" id="add-an-object-entry"></a>

Create an entry in a specified object. Example use cases include

* Automatically adding entries in a custom Shipments object when orders are paid
* Automatically adding a default user for new accounts
* Manually adding a child ticket to the current ticket within a custom ticketing system

You must select an active object. You can then set predefined values for the entry’s fields. Enter these values directly or click _Code_ ( ![Code](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-code.png) ) to use expressions that set values dynamically. If the object includes required fields, you must set predefined values for them.

![Select an active object and enter predefined values for the created entries.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/understanding-action-types/images/02.png)

### Update an Object Entry <a href="#update-an-object-entry" id="update-an-object-entry"></a>

Update one or more fields in the current object entry. Example use cases include

* Automatically updating an entry’s boolean field to `true` after users download its attached file
* Manually updating a ticket’s assignee to the current user when clicking a button
* Automatically updating an entry’s boolean field to `true` after users favorite it

You must select the field(s) to update in the current entry and enter new values. You can enter these values directly or click _Code_ ( ![Code](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-code.png) ) to use expressions that set values dynamically.

![Select the fields from the current object entry that you want to edit and enter default values.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/understanding-action-types/images/03.png)

### Webhook <a href="#webhook" id="webhook"></a>

Webhook actions deliver a data payload to a set URL. You must enter a target URL. If applicable, enter a secret.

![Enter a webhook URL and secret.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/understanding-action-types/images/04.png)

### Groovy Script <a href="#groovy-script" id="groovy-script"></a>

Groovy Script actions execute Groovy scripts. Enter your script into the provided code window. For version 7.4 +, the portal uses the [GroovyShell class](https://docs.groovy-lang.org/latest/html/api/groovy/lang/GroovyShell.html) to validate Groovy script syntax when you click Save. If the script is invalid, the portal displays a general error message.

![Enter your Groovy script.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/understanding-action-types/images/05.png)
