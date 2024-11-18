# Application actions

### Navigate To

Use this action if you want to go to another web page or app screen.\
For example: Navigation on row button click

When the _Destination_ is set to 'URL', you have the option to open the link in a new tab.

<figure><img src="https://files.readme.io/8f018e8-new_tab.png" alt=""><figcaption></figcaption></figure>

Alternatively, if the _Destination_ is set to 'Screen', then you can select a screen to navigate to from the dropdown. The screen can optionally be opened in the current tab, or in a [modal](https://docs.budibase.com/docs/screens#modal-screen).

You can learn more about using URL parameters with the `Navigate To` action [here](https://docs.budibase.com/docs/url-parameters).

<figure><img src="https://files.readme.io/a455654-open_modal.png" alt=""><figcaption></figcaption></figure>

### Trigger Automations

Often there are times where you would like to trigger a automation after a user has performed an action. There are a range of different Automations. You are able reference any created automations by clicking the `Use an existing automation` radio button and then selecting an existing previously created automations to use and run.

<figure><img src="https://files.readme.io/d09d42c-224bfaf-Screenshot_2023-05-19_at_11.27.45.png" alt=""><figcaption></figcaption></figure>

Lastly, creating fields within the automation itself allows data/bindings to be sent to it, this can then be used in your automation to perform whatever task you require. An example of this would be passing an `_id` of a row to the automation to then update the row's state e.g. from `Pending` to `In progress`.

<figure><img src="https://files.readme.io/15a00b5-6bfc5bc-Screenshot_2023-05-19_at_11.50.58.png" alt=""><figcaption></figcaption></figure>

### Logout

If you want to log a user out, you can use this action. It has redirect functionality built in meaning you can redirect to whatever url you desire.

### Close Screen Modal

If the current screen has been opened as a Modal, then close it. Additionally, you can navigate to a new screen or open a different screen modal afterwords also using this action.

### Close Modal

This action takes no settings, and will close any modal component open on screen.

### Open Modal

Opens a modal component on a screen - different to a "Navigate to" action with "Open screen in modal", which opens an entire screen in a modal, this action opens a modal component that is already on the current screen.&#x20;

### Show Notification

This allows you to show a popup notification, of which there are four different types; `Success`, `Warning`, `Error` and `Info`. Changing the type will change the styling of the popup for example: success will be have a green background where as error will have a red background. Additionally you can make use of the bindings to output more informative messages. E.g. `Hi {{ Current User.email }} welcome.`

![](https://files.readme.io/5045b379a19194c916869fcab88a0fde2a2bd7e099318bb67e02e715fcbb66d0-Screenshot\_2024-09-04\_at\_16.03.18.png)

#### Logic within notifications

It is possible to use some logic with the notification - if the result of the logic running is falsy, no message will show. This can be used in conjunction with a "Continue if/Stop If" step to show the user some feedback explaining why the action has stopped.

### Prompt User

This action adds an additional step before other actions are run. The title, message, and button text can all be customised. It provides a prompt to the user, and won't run any other action after it unless they click the confirm button. This is very useful for a longer chain of actions, as it provides context and gives the option to cancel the process before it starts

![](https://files.readme.io/8499167ec5c02eb383158fb8bf283960a3e4426483ae9edb667a76707e8179df-image.png)

\


### Open Side Panel

Provides a list of all side panels within a current screen, selecting one will open that side panel whenever the action is hit.

### Close Side Panel

Closes the currently open side panel.
