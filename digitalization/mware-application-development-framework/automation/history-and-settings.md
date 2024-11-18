# History and settings

It is possible to see a list of automation runs for your published apps.\
To do so, click the `Settings` tab in the app builder.

This will take you to the app overview screen where you can click on the `Automations` tab to see a list of your automation runs.

<figure><img src="https://files.readme.io/5bcd099-Screenshot_2024-02-23_at_11.08.18.png" alt=""><figcaption></figcaption></figure>

You can filter and order the logs by automation name, data range, and status.

There are three statuses:

* **Success**: The automation run was successful
* **Stopped**: The automation run was correctly terminated by a [Condition](https://docs.budibase.com/docs/conditions-1)
* **Error**: The automation run failed to complete due to an error
* **Stopped - Error**: Specific to Cron jobs. If the cron automation errors five times back-to-back, it will be disabled until you re-publish your app.

### Investigating errors

Whenever an automation run errors, you will see a red banner at the top of your app list with the automation name and the number of times it failed in parentheses.

<figure><img src="https://files.readme.io/c66db1c-Screenshot_2022-07-15_at_09.42.14.png" alt=""><figcaption></figcaption></figure>

There is a convenient 'View error' link which will take you to the automation history, or you can click the 'X' to dismiss the notification.

<figure><img src="https://files.readme.io/6c190fa-Screenshot_2024-02-23_at_11.13.54.png" alt=""><figcaption></figcaption></figure>

When selecting an error, you will be able to see which automation step failed as well as the error output.

Furthermore, you can click the `Edit automation` button to be taken to the automation which may need fixed.

> ### 📘
>
> Automation history will only show for published apps, and you must publish your app to apply any changes to your automations.

### Expiration 🔒

Logs are kept for a single day on the free plan, but you can expand the duration of time that logs are kept significantly by upgrading.

### Chain automations

On a per app basis, it is possible to toggle the _Enable chaining_ setting on/off.

By default, this setting if **off** in cloud, and **on** in self-host.

When this setting is **on**, it allows any automation within the app that has any of the **Row** Triggers (_Created, Updated, Deleted_) to be triggered when another automation fires a **Row** action (_Create, Update, Delete_).

Whether to have this setting on or off depends on your use-case:

* If you want to restrict your automations to only trigger upon End User table writes, then toggle **off**.
* If you want your automation row triggers to fire when an End User _OR_ an automation writes to a table, then toggle **on**.

**Example**

<figure><img src="https://files.readme.io/298a7ec-Screenshot_2024-02-23_at_13.08.34.png" alt="An automation that creates a row in the &#x27;Employee&#x27; table"><figcaption><p>An automation that creates a row in the 'Employee' table</p></figcaption></figure>

<figure><img src="https://files.readme.io/7dd34b7-Screenshot_2024-02-23_at_13.10.00.png" alt="An automation that logs a message when an &#x27;Employees&#x27; table row is created"><figcaption><p>An automation that logs a message when an 'Employees' table row is created</p></figcaption></figure>

In this basic example, the first automation would only trigger then second if the _Enable chaining_ toggle was **on**.

> ### 📘Additional info
>
> As it could be possible to accidentally get into an infinite loop, the maximum number of automations that can be triggered in a sequence is **five**.
>
> Also, chaining automation row events should not be confused with the Trigger automation action, which will always fire if used.
