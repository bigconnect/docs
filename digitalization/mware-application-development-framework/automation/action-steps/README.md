# Action steps

Actions are the core of automations. Actions can carry out updates on a table, call to an external service, send emails, and so on. We have built our action system to be easily extensible so actions can be added to the system at any time! Your automation can have many actions, allowing you to perform many different tasks as part of a single trigger.

<figure><img src="https://files.readme.io/cef1c9e7aeba7e317cb957760e53fe3249161fffea6c52d629686e507c3b593b-Screenshot_2024-11-07_at_08.27.43.png" alt=""><figcaption></figcaption></figure>

Below is a few examples of actions that exist today however there may be many more when you look in the builder. To get a full list install the builder and check it out!

1. **Create row** - this will create a new row in a specified table
2. **Update row** - can be used to update a row that was triggered upon
3. **Delete row** - delete a specified row from a table, like something that was triggered upon
4. **Send email** - as it suggests, send an email based on the specified inputs
5. **Send Discord notification** - send a notification to Discord
6. Slack - send a notification to Slack
7. **Connect to Integromat** - connect to Integromat
8. **Connect to Zapier** - connect to Zapier
9. **JS scripting** - add JS scripts to an automation
10. **Query rows** - query rows / records

The real power in actions is the ability to make use of outputs from previous steps; how to do this will be covered in the Contextual bindings section.

***

### Adding logic

Sometimes it is necessary for your automation to make decisions as part of the process, for example, if you wanted to send an email to a customer when their order enters the shipped state then you would need a logical step to handle this. An automation can have as many logical steps as you require, if you wish to filter against multiple different outputs from previous steps then this can be done with multiple filters.

Currently, there are two actions that allow you to add logic to an automation

1. **Filter** - Filtering against a specified field, with a particular operator and value to compare against
2. **Delay** - waiting a period of milliseconds before continuing

***

### Naming automation steps

When you have many steps in an automation, it can be difficult to keep track of the step numbers, particularly in your Bindings.

To make your automations easier to manage, you can give your steps a name, which will automatically update any bindings as well!

For example, consider this example automation:

* Step 1: Create Row
* Step 2: Log the response of step 1

<figure><img src="https://files.readme.io/2fff9c76d79802b237aade8030e2c1d9b2502aa6c1c939ab345349239b430551-Screenshot_2024-11-07_at_08.54.19.png" alt=""><figcaption></figcaption></figure>

The binding `{{ steps.Create row.response }}` isn't very informative. We can however change the name of _Step 1_ to 'Add Expense' which will automatically update the binding to something more useful:

<figure><img src="https://files.readme.io/1d9fed009c179dc18a24089dcacaf9d2985e60ddb8ec65ed38a04add92501f07-Screenshot_2024-11-07_at_08.53.59.png" alt="The binding &#x27;steps.1.response&#x27; has automatically been updated"><figcaption><p>The binding 'steps.Create row.response' has automatically been updated</p></figcaption></figure>
