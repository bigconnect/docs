# Discord

The Discord notification is a webhook that allows you to send notifications from your Budibase app to a Discord channel.

### Settings

The Discord Action has four settings:

| Setting              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Incoming Webhook URL | <p>Add your Discord Webhook URL.<br><br>To find your Discord Webhook URL:<br><em>Open your Server Settings and head into the Integrations tab.</em><br>Navigate to the Webhooks section and either View Webhooks or Create a new webhook.<br><br>Further information and directions on how to find your Discord webhook URL:<br><br><a href="https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks">https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks</a></p>                                                                          |
| Bot Name             | Copy the name of your bot from Discord                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Bot Avatar           | <p>To give your bot an avatar, add your image link to the Bot Avatar field. For example:<br><br><a href="https://res.cloudinary.com/daog6scxm/image/upload/v1632818347/logo/budibase-icon-500_svrfwf.png">https://res.cloudinary.com/daog6scxm/image/upload/v1632818347/logo/budibase-icon-500_svrfwf.png</a><br><br>On Discord, the avatar size is 128 x 128 pixels. However, you'll actually want to upload a larger square image. Discord will automatically scale your image down to the appropriate dimensions, but you'll retain high-quality resolution in the process.</p> |
| Message              | The content you would like to send to your Discord channel when your automation is triggered.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

&#x20;

***

&#x20;

### Tutorial - When a new record is added, send a Discord notification

The following tutorial will demonstrate how to send a Discord notification when a new record is created. We will assume you have your data setup and design screens built.

Our Discord notification automation will consist of two automation blocks:

1. Trigger - Row created
2. Action - Discord notification

Click the add automation icon.

![502](https://files.readme.io/d784500b5858e7d8d2606aa6ed88113bb9d297cc22cca65b7f989e973cebbebd-Screenshot\_2024-11-07\_at\_09.09.38.png)

&#x20;

#### Add Trigger - Row created

Give your new automation a name - we will name ours `Discord notification`. Select the 'Row Created' trigger. Then click 'Save'.

<figure><img src="https://files.readme.io/59b0157-CleanShot_2022-04-27_at_12.32.282x.png" alt=""><figcaption></figcaption></figure>

Now click the 'SETUP' dropdown on your new automation block, and select your table - in our case Sales.

<figure><img src="https://files.readme.io/d75581cdc8e9964300c4c6a29793413c89bfacbe0389e88594c4cc8db89440bc-Screenshot_2024-11-07_at_09.18.08.png" alt="597"><figcaption><p>trigger setup</p></figcaption></figure>

&#x20;

#### Add Action - Discord

Now click the small plus icon button under the Trigger automation block, select Discord, then click Save.

<figure><img src="https://files.readme.io/9b5c2f7a0c8d6b72c80e954d7c2b328bb7f84a2bfa4f33e0e04c191b87a01593-Screenshot_2024-11-07_at_09.19.22.png" alt="1200"><figcaption><p>Add discord action to budibase</p></figcaption></figure>

Now it's time to set up our Discord action. If you don't have a Discord channel, [please set one up](https://discord.com/blog/starting-your-first-discord-server).

**Get your incoming Webhook**

Open your Server Settings and head into the Integrations tab. Click on View Webhooks, select your Webhook, and click 'Copy Webhook Url'.

<figure><img src="https://files.readme.io/6449cc6-CleanShot_2022-04-27_at_12.16.062x.png" alt=""><figcaption></figcaption></figure>

**Create a new Discord Webhook**

If you don't have a Webhook to connect to, click the "Create Webhook" button to create a new webhook.

You'll have a few options here. You can:

* **Edit the avatar:** By clicking the avatar next to the Name in the top left
* **Choose what channel the Webhook posts to:** By selecting the desired text channel in the dropdown menu
* **Name your Webhook:** Good for distinguishing multiple webhooks for multiple different services.

Discord will generate you a webhook. Click the 'Copy Webhook URL' button.

<figure><img src="https://files.readme.io/dad41fa-CleanShot_2022-04-27_at_12.15.162x.png" alt=""><figcaption></figcaption></figure>

Paste your Discord Webhook URL into the Incoming Webhook Url input within your [Action](https://docs.budibase.com/docs/automation-actions) automation block.

In the Bot name field, add the name of your Bot. We've not got an avatar, so we'll leave the Bot avatar field blank.

<figure><img src="https://files.readme.io/c64145c21725dbf7c20e78e2b597e58475a8b8cbf85aa8f5eab46ef21d142db1-Screenshot_2024-11-07_at_09.22.29.png" alt=""><figcaption></figcaption></figure>

In the Message input, add the message you would like Budibase to post to your Budibase channel when a new row is created. You can bind values from the newly created record to your message. For our use case, I want to send the following notification to our channel:

Handlebars

```html
We just made a new sale in {{ trigger.row.Country }}

Check out the sale using the link below:
https://joe.budibase.app/app/tutorials#/sales/{{ trigger.id }}
```

From the above, you can see I have used automation bindings to pull data into our message. Check out the following documentation to learn more about how to use Data in automations.

<figure><img src="https://files.readme.io/b47b9f41e91161c5674057785e50a490c47e46a40e8fdc0197f4b300e8a83681-Screenshot_2024-11-07_at_09.30.20.png" alt=""><figcaption></figcaption></figure>

**Trigger output bindings - Trigger.Row**

To pull data from your Trigger record, you can use Trigger Output helpers (see Gif above). Using the helper `trigger.row` I now have access to that data from our newly created record. I add the field/column title to the binding to pull a specific field back: `trigger.row.Country`.

To allow users to access the specific record page, we must add a link to our message.

**To create a link, we must first Publish our application.** Save the automation, click the Publish app button. Now, go back to the app portal page (Click the Budibase icon). Locate your app, click the more icon, and click View published app. Copy your base URL - We will improve this experience soon.

**Trigger output bindings - Trigger.ID**

Now, click into your app and go back to the automation we were just creating and into the message section (where we were above). Paste your URL, and append it with the Trigger ID output. Click Save.

#### Test automation

Click `Finish and test automation`. **Then select your table from the dropdown.**

<figure><img src="https://files.readme.io/cffe57f32098b6c6519734c732de274a9554b10613ddc4dd1177beeb0238fe68-Screenshot_2024-11-07_at_09.53.09.png" alt=""><figcaption></figcaption></figure>

In the Column/Field you referenced in your trigger.row binding above (we used Country), add a value - we'll add `Budiland` to the 'Country field'. Click 'Test'. You should now see a message on your Discord channel.

If you are happy, click 'Publish app'. Add a new record within your app, and like magic, watch as a new Discord notification appears within your channel.

<figure><img src="https://files.readme.io/839f2a1-CleanShot_2022-04-27_at_12.25.212x.png" alt=""><figcaption></figcaption></figure>
