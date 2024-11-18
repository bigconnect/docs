# Synchronous automations

Synchronous automations allow users to receive feedback from their automation, in the Design section or via a webhook response.

Automations are usually asynchronous, this means that when an automation runs, it does so in the background and the user is unable to access its result, but with synchronous automations the result is available to the user.

> ### 📘Licensing🔒
>
> The following features are available to:
>
> * **All installations** with a Business or Enterprise license\
>

### Creating a Synchronous Automation

You can make an automation synchronous by adding a **Collect Data** step.

<figure><img src="https://files.readme.io/64c0a86-Screenshot_2023-06-06_at_09.19.31.png" alt=""><figcaption></figcaption></figure>

A **Collect** block will simply return whatever binding or value is passed to it.

Once this block has been added, you can access the result of your automation as a user, or external user depending on whether your automation trigger is an App action or Webhook.

> ### 📘Last step
>
> As the **Collect Data** action returns data from your automation, it must be the last step. Otherwise you will see that the action is greyed out:
>
>

<figure><img src="https://files.readme.io/14b06ac-last_step.png" alt=""><figcaption></figcaption></figure>

\


### Webhook

When using the [Webhook](https://docs.budibase.com/docs/webhook) trigger, you can return data to the caller of the trigger URL.

For example consider this webhook automation that queries an Employees table, filtering on the _City_ that is passed into the trigger:

![](https://files.readme.io/6b9c9d8-filtering.png)

We can then specify data that should be returned to the caller of the trigger, in this case simply all of the rows matching the city provided.

![](https://files.readme.io/d53ae9d-collect\_data.png)

After publishing this automation, we can see the result in our POST request:

![](https://files.readme.io/6bee45d-string\_trigger.png)

You can now see the response from the **Collect Data** step has been provided in the response of the trigger call. Note that value in this case is a string - this is because **Handlebars always return strings.**

If you want to ensure that the correct typing is returned, such as for numbers and arrays, then you can use a JavaScript binding instead.

<figure><img src="https://files.readme.io/49c7141-return.png" alt="Making sure an array is returned"><figcaption><p>Making sure an array is returned</p></figcaption></figure>

And now the result:

\


<figure><img src="https://files.readme.io/e096211-obj_trigger.png" alt=""><figcaption></figcaption></figure>

### App action

For users to access the result of your automations in a published app, you will need to configure an [App action](https://docs.budibase.com/docs/app-action) trigger within the _Design_ section. Click on `Define actions` to open the actions drawer to create a new or use an existing synchronous automation.

<figure><img src="https://files.readme.io/bd467f0-image.png" alt=""><figcaption></figcaption></figure>

> ### 🚧Timeout
>
> As shown in the screenshot, a synchronous automation can run for up to 120 seconds, or less if required.

Once added this provides a binding with the result of the automation to any following action, through Chaining actions.

In this example, we will use the binding to add the result of an automation into App state and then display it in a paragraph component.

<figure><img src="https://files.readme.io/be17c80-image.png" alt=""><figcaption></figcaption></figure>

When the button with these actions attached is clicked, state will now be updated with a key called `result` that contains the result of our automation.

To display this we now simply to need bind the state to the paragraph.

<figure><img src="https://files.readme.io/8aa0d6e-Screenshot_2023-06-06_at_09.45.53.png" alt="" width="375"><figcaption></figcaption></figure>

We can now preview our app and click the [Button](https://docs.budibase.com/docs/button) for which our actions are defined.

<figure><img src="https://files.readme.io/35dd737-image.png" alt="" width="375"><figcaption></figcaption></figure>

The automation result is stored in to state, and due to the binding of that state to the paragraph, appears on screen whenever the `Trigger` button is clicked.

Instead of displaying the result in a paragraph, you could just as easily use the **Show Notification** action, or any other combination of actions.

> ### 📘Automation Result
>
> The automation result binding will only be available if your automation has a **Collect Data** step.
