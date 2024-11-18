# Row triggers

### Row created

This trigger type allows users to create automations based on newly created rows. When a row is created, the entire row’s object will be passed to the automation and referenced through trigger Bindings. Various steps can be added to manipulate the data, such as updating information in another table or sending the data to a REST API endpoint.

<figure><img src="https://files.readme.io/6d7ae47868cd03ee5b4880c34a26e5d7ddc7bf51b55e2cbd2c8c5f5599c561d1-Screenshot_2024-11-06_at_13.36.39.png" alt=""><figcaption></figcaption></figure>

### Row deleted

This trigger works similarly to created rows. It will pass in a row’s object whenever a row is deleted. This can then be used to do other operations e.g. removing it from an API or deleting a related row elsewhere within Mware Application Development Framework.

<figure><img src="https://files.readme.io/3b82cc80ec39b78e2a6375207cf1fd68331f2d597d1ddbdf8d89c5f6a5de80cf-Screenshot_2024-11-06_at_13.36.32.png" alt=""><figcaption></figcaption></figure>

### Row updated

Like the other two triggers listed above, this automation will run whenever a row within the chosen table is updated.

<figure><img src="https://files.readme.io/0556174b8d675f5bfe8593a087b9de34e3d8033eeda681903ed9e682087ff3c3-Screenshot_2024-11-06_at_13.36.26.png" alt=""><figcaption></figcaption></figure>

It will provide both the old row's object (before the change). Additionally you also have access to the current values these are displayed as Bindings the names will reflect the columns of your table.

<figure><img src="https://files.readme.io/31a8a56f890e14714797941097b640851a11c396c3aa109a9a0efa68e9d0d487-Screenshot_2024-11-06_at_13.35.38.png" alt=""><figcaption></figcaption></figure>

With the addition of the oldRow object, you can now perform more complex operations, such as testing if a specific value has changed and then continuing or ending the automation based on that condition.

This trigger can also be used similarly to the creation and deletion triggers, such as updating additional related records or posting updates to an API or query.
