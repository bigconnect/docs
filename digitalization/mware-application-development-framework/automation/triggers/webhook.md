# Webhook

> A webhook is an HTTP-based callback function that allows lightweight, event-driven communication between two APIs.

Contextually, webhooks allow the system to listen for an external application event, and trigger an automation. A payload will be included in the trigger that gives the system information about the event.

### Setup

Create a new automation and select `Webhook` as the trigger.

<figure><img src="https://files.readme.io/859449a-Screenshot_2022-08-31_at_13.38.49.png" alt=""><figcaption></figcaption></figure>

This will present you with a box that contains two URLs:

<figure><img src="https://files.readme.io/aeacc2a-Screenshot_2022-08-31_at_14.16.40.png" alt=""><figcaption></figcaption></figure>

The **Schema URL** is required to tell the system what payload to expect from the external application.

Click the `Set Up Webhook` button. This will bring up a dialog with the schema URL. Copy the schema URL.

Let's assume for this example we want to save a row to a table any time a new issue is created in our GitHub repo.

Conveniently we can grab an example payload from the GitHub webhook docs.

Next open Postman, or any other REST query tool, and POST the example payload to the schema URL.

<figure><img src="https://files.readme.io/2151db6-Screenshot_2022-08-31_at_14.17.38.png" alt=""><figcaption></figcaption></figure>

After you receive the `200 OK` response, you should now see that the system has successfully picked up on the schema. Click _Finished_.

<figure><img src="https://files.readme.io/0ca3d13-Screenshot_2022-08-31_at_14.18.04.png" alt=""><figcaption></figcaption></figure>

Finally click the `Publish` button in the top right of the screen to make the automation live.

### Link the trigger application

Copy the **Trigger URL** from your automation and paste it into the _Payload URL_ of your GitHub webhook.

Make sure **Content type** is _application/json_

<figure><img src="https://files.readme.io/33f3c2c-Screenshot_2022-08-31_at_16.55.44.png" alt=""><figcaption></figcaption></figure>

Click `Add webhook`.

To test, add an new issue in your GitHub repo. Within a minute, you will be able to see that the automation was triggered via the Automtion history.

![](https://files.readme.io/ec0b85c-Screenshot\_2022-08-31\_at\_14.25.13.png)

<figure><img src="https://files.readme.io/b687c2dfa2817d868b6d6cc0577f8a1d5f950bc9af598ea496c3f4f34b411865-Screenshot_2024-10-28_at_14.19.12.png" alt=""><figcaption></figcaption></figure>

### Trigger body

Now that we know that GitHub events are being picked up by our application, lets do something with the event data. For example, we may want to create a new row in our own 'Issues' table.

Create a new 'Issues' table with a text _Name_ column, a number _Number_ column, and an options type _Tag_ column.

<figure><img src="https://files.readme.io/528f978653f746bf77cac3efc73fbbeab0c0aad72f897d74d3b65da9a056f770-Screenshot_2024-10-28_at_14.05.32.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/3092c7b9e71eb9a2e9b3207d800e89383a9fde311b258d2a5bd173cc936487bc-Screenshot_2024-10-28_at_14.05.42.png" alt=""><figcaption></figcaption></figure>

Next within the _Automation_ tab, add an additional Condition step. This is because we only want to create a row if the GitHub action type is _opened_.

Add the `{{ trigger.action }}` binding to the _Reference Value_ field.

<figure><img src="https://files.readme.io/41b9c096f5a96fc77746977de7cf2dc973852cb3458224f356b9e60a183f1fee-Screenshot_2024-10-28_at_14.11.57.png" alt=""><figcaption></figcaption></figure>

Select condition _Equals_ comparison value 'opened'.

<figure><img src="https://files.readme.io/5a2f7e4e04226c4a406857460c669d37fe53f4b07fdf334864403dddab342b74-Screenshot_2024-10-28_at_14.18.40.png" alt=""><figcaption></figcaption></figure>

Add another automation step for **Create Row**. Select _Use bindings_ as well as the 'Issues' table. Fill in the bindings.

<figure><img src="https://files.readme.io/a925cffe6d92fd5e6ce6fb6bcdaa6053ad7f785ed27952f543eeb8d81b7876c7-Screenshot_2024-10-28_at_14.17.18.png" alt=""><figcaption></figcaption></figure>

You can click `Finish and test automation` to verify your automation. Don't forget to press **Publish** when you're done.

Now when a new issue is created in your GitHub repo, you should see a new row in your table.

<figure><img src="https://files.readme.io/cd4d76d-Screenshot_2022-08-31_at_16.59.39.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/100dc297852ac5f63451bd8e05fe458a049f8363b704aa323ddef10c1c5fbe14-Screenshot_2024-11-06_at_13.29.14.png" alt=""><figcaption></figcaption></figure>
