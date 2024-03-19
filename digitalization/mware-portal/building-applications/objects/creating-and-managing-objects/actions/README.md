# Actions

Object actions define custom business logic that you can run manually or automatically for object entry events. Examples include delivering entry event data to a webhook URL, sending email notifications after entry updates, and executing custom Groovy scripts after entry creation.

When using automatic triggers, Liferay listens for the event and executes the operation automatically according to predefined conditions.

When using the standalone trigger, Liferay adds the action to the Actions menu ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for object entries and generates a headless API to trigger it. The action must be run manually, but you can map the action to a button fragment. See [Using Manual Actions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions/using-manual-actions) to learn more about manual actions.

![Add actions to object definitions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/images/01.png)

Liferay Objects provides these action types:

| Type                   | Description                                                   |
| ---------------------- | ------------------------------------------------------------- |
| Notification           | Send email or user notifications using a predefined template. |
| Add an Object Entry    | Create entries in an active object.                           |
| Update an Object Entry | Update fields in the current object entry.                    |
| Webhook                | Deliver a payload to a URL.                                   |
| Groovy Script          | Execute [Groovy](https://groovy-lang.org/) scripts.           |
