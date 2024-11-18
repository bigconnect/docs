# n8n

### Setup

Within **n8n** create a [Webhook trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.webhook/). Optionally configure the _Authentication_, and select the desired _HTTP Method_.

![](https://files.readme.io/05f78ca-webhook\_n8n.png)

Click on the webhook URL to copy it to clipboard. You can start with the _Test URL_ to make sure things are working as expected before using the _Production URL_.

Connect additional nodes to your **n8n** webhook trigger as desired.

### DB node

It is also possible to connect to the DB within your **n8n** workflow.

Read more on installation: [Install community nodes in the n8n app](https://docs.n8n.io/integrations/community-nodes/installation/gui-install/).

<figure><img src="https://files.readme.io/bd12cde-Screenshot_2024-02-21_at_10.01.16.png" alt="Install the Budibase DB node"><figcaption><p>Install the DB node</p></figcaption></figure>

<figure><img src="https://files.readme.io/0f65f09-Screenshot_2024-02-21_at_10.03.30.png" alt="Budibase DB node will then be available"><figcaption><p>DB node will then be available</p></figcaption></figure>

### Triggering an n8n workflow

With the webhook setup in **n8n**, and the URL copied, you can now add the `n8n` automation action step in your automation.

<figure><img src="https://files.readme.io/f567feb-n8n_step.png" alt="Adding the n8n action step"><figcaption><p>Adding the n8n action step</p></figcaption></figure>

Next enter the _Webhook URL_ that you copied, and make sure the _Method_ matches correctly.

<figure><img src="https://files.readme.io/7d54bb0e2fea49ec74e2725dc02e186ca6cc0b326b2a37464cc0ec19a9b0b7fa-Screenshot_2024-11-07_at_11.46.27.png" alt="Triggering an n8n workflow from a Budibase automation"><figcaption><p>Triggering an n8n workflow from an automation</p></figcaption></figure>

#### Authorization

The authorization field is optional, and represents the Authorization HTTP header.

Consider an **n8n** webhook with _Basic_ authentication set as _user_: 'test', _password_: 'admin'.

![](https://files.readme.io/7a6bb16-Screenshot\_2024-02-21\_at\_10.26.12.png)

The automation action step will return with a `401` error if the _Authorization_ field is not set:

<figure><img src="https://files.readme.io/5b80256-Screenshot_2024-02-21_at_10.29.53.png" alt="Authorization needs to be set"><figcaption><p>Authorization needs to be set</p></figcaption></figure>

In the case of _Basic_ auth you need to provide the header in the following format:`Basic BASE64_ENCODED(username:password)`, which would look result in something like this:

<figure><img src="https://files.readme.io/e21782d72607ecd78e8ccd252cdc64dfb751334d8d02b310a48a442caf3fbf79-Screenshot_2024-11-07_at_11.47.13.png" alt=""><figcaption></figcaption></figure>

> ### 🚧Keep credentials safe
>
> Ideally you should not save credentials as plain text in your apps. Use Environment variables to keep these credentials safe!
