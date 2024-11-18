# Zapier

### &#x20;Setup

To begin, create a Zap and select `Webhooks by Zapier` as your first step, and click `Continue`.

Click `Copy` to copy the Webhook URL to the clipboard.

Send a JSON payload to the webhook URL to setup the request body. This should match the JSON that you will be sending from the system.

With the request body defined, you can use those values in your subsequent steps by selecting the `Raw Body` binding, and typing the path to the desired property.

For example:

<figure><img src="https://files.readme.io/df56509-small-Screenshot_2023-05-11_at_17.37.01.png" alt="Selecting the &#x27;Raw Body&#x27; binding"><figcaption><p>Selecting the 'Raw Body' binding</p></figcaption></figure>

<figure><img src="https://files.readme.io/0a972e7-small-Screenshot_2023-05-11_at_17.37.45.png" alt="Typing out the desired body property, i.e. &#x27;value5&#x27;"><figcaption><p>Typing out the desired body property, i.e. 'value5'</p></figcaption></figure>

***

### Pushing data to Zapier&#x20;

With your Zap configured, you can now interact with it.

1. Create a new automation with a Trigger.
2. Click the `+` icon, and select the `Zapier` app action.
3. Paste in the webhook URL and fill in the JSON payload as previously defined in Zapier.

<figure><img src="https://files.readme.io/6cce345-zapier.gif" alt=""><figcaption></figcaption></figure>

***

### Using bindings

Most likely you will want to send dynamic values, such as those from your automation trigger, rather than static values previously shown.

You can do this through Bindings.

If you are using an App action trigger, then the fields can be accessed like so:

<figure><img src="https://files.readme.io/675a270-small-Screenshot_2023-05-11_at_17.54.51.png" alt="App action fields binding"><figcaption><p>App action fields binding</p></figcaption></figure>

> ### 📘
>
> **Note** the binding expression in this case is surrounded in double quotes because _appValue_ is of type **Text**.
>
> For type **Number**, the quotes should not be included: `{{ trigger.fields.appValue }}`
