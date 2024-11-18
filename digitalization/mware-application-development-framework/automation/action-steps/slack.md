# Slack

The Slack notification is a webhook that allows you to send notifications from your app to a Slack channel.

### Settings

The Slack Action has two settings:

| Setting              | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Incoming Webhook URL | <p>Further information and directions on how to find your Slack channel's webhook URL:<br><br><a href="https://slack.com/intl/en-gb/help/articles/115005265063-Incoming-webhooks-for-Slack">https://slack.com/intl/en-gb/help/articles/115005265063-Incoming-webhooks-for-Slack</a><br><br><a href="https://api.slack.com/messaging/webhooks">https://api.slack.com/messaging/webhooks</a></p> |
| Message              | The content you would like to send to your Slack channel when your automation is triggered.                                                                                                                                                                                                                                                                                                    |

### Tutorial

The following tutorial will demonstrate how to send a Slack notification when a new record is created. We will assume you have your data setup and design screens created.

Our Slack notification automation will consist of two automation blocks:

1. Trigger - Row created
2. Action - Slack notification

Click the add automation button.

#### Add Trigger - Row created

Name your automation. We will name ours `Slack notification`. Select the `Row Created` trigger. Then click `Save`.

<figure><img src="https://files.readme.io/3b032dd-CleanShot_2022-04-19_at_15.14.00.png" alt="1253"><figcaption><p>row created trigger</p></figcaption></figure>

Now click the SETUP dropdown on your new automation block, and select your table.

<figure><img src="https://files.readme.io/979de9a-CleanShot_2022-04-19_at_15.16.56.png" alt="597"><figcaption><p>trigger setup</p></figcaption></figure>

#### Add Action - Slack

Now click the small plus icon button under the Trigger automation block, select Slack, then click Save.

<figure><img src="https://files.readme.io/ebd020f-CleanShot_2022-04-19_at_15.18.55.png" alt=""><figcaption></figcaption></figure>

Now it's time to set up our Slack action. If you don't have a Slack channel, please set one up. To get your Incoming Webhook URL, [create a Slack App](https://api.slack.com/apps?new\_app=1).

When creating a Slack app, choose 'From Scratch'.

<figure><img src="https://files.readme.io/6eeea71-CleanShot_2022-04-19_at_15.23.05.png" alt=""><figcaption></figcaption></figure>

Name your Slack app, and choose your Workspace.

<figure><img src="https://files.readme.io/733cc0d-CleanShot_2022-04-19_at_15.24.44.png" alt=""><figcaption></figcaption></figure>

Then select Incoming Webhooks.

<figure><img src="https://files.readme.io/a75e819-CleanShot_2022-04-19_at_15.25.06.png" alt=""><figcaption></figcaption></figure>

Activate incoming webhooks by switching on the toggle.

![673](https://files.readme.io/d101456-CleanShot\_2022-04-19\_at\_15.25.57.png)

Select the Add New Webhook to Workspace button.

<figure><img src="https://files.readme.io/6cee43a-CleanShot_2022-04-19_at_15.26.39.png" alt=""><figcaption></figcaption></figure>

Now select the channel you would like to post to.

<figure><img src="https://files.readme.io/7d817c2-CleanShot_2022-04-19_at_15.27.36.png" alt=""><figcaption></figcaption></figure>

Slack will generate you a webhook. Click the Copy button to copy your webhook.

<figure><img src="https://files.readme.io/e711e43-CleanShot_2022-04-19_at_15.28.33.png" alt=""><figcaption></figcaption></figure>

Paste your Slack Webhook URL into the Incoming Webhook Url input within your Action automation block.

<figure><img src="https://files.readme.io/8a6c8f4-CleanShot_2022-04-19_at_15.30.08.png" alt=""><figcaption></figcaption></figure>

In the Message input, add the message you would like the system to post to your Slack channel when a new row is created. You can bind values from the newly created record to your message. For our use case, I want to send the following notification to our channel:

Handlebars

```html
Woohoo, we just made a new sale in {{ trigger.row.Country }}

Check out the sale using the link below:
https://joe.app/app/tutorials#/sales/{{ trigger.id }}
```

From the above, you can see I have used automation Bindings to pull data into our message.

<figure><img src="https://files.readme.io/22371d7-CleanShot_2022-04-19_at_15.44.02.gif" alt=""><figcaption></figcaption></figure>

**Trigger output bindings - Trigger.Row**

To pull data from your Trigger record, you can use Trigger Output helpers (see Gif above). Using the helper `trigger.row` I now have access to that data from our newly created record. I add the field/column title to the binding to pull a specific field back: `trigger.row.Country`.

To allow users to access the specific record page, we must add a link to our message.

**To create a link, we must first Publish our application.** Save the automation, click the Publish app button. Now, go back to the app portal page (Click the icon). Locate your app, click the more icon, and click View published app. Copy your base URL - We will improve this experience soon.

**Trigger output bindings - Trigger.ID**

Now, click into your app and go back to the automation we were just creating and into the message section (where we were above). Paste your URL, and append it with the Trigger ID output. Click Save.

#### Test automation

Click `Finish and test automation`. Then select your table from the dropdown.

![494](https://files.readme.io/ac82ee9-CleanShot\_2022-04-19\_at\_16.42.26.png)

In the Column/Field you referenced in your trigger.row binding, add a value - to the Country field. Click test. You should now see a message in your Slack channel.

<figure><img src="https://files.readme.io/dde9e37-CleanShot_2022-04-19_at_16.41.362x.png" alt=""><figcaption></figcaption></figure>

If you are happy, click Publish app. Add a new record within your app, and like magic, watch as a new Slack notification appears within your channel.
