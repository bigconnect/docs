# Make

To get started, follow the Make guide on setting up a [Webhook](https://www.make.com/en/help/tools/webhooks).

If you have correctly determined your data structure in Make, you should be able to see the available bindings that your Webhook offers to subsequent steps:

<figure><img src="https://files.readme.io/465ec63-small-Screenshot_2023-05-11_at_16.52.41.png" alt="Available Make Webhook bindings"><figcaption><p>Available Make Webhook bindings</p></figcaption></figure>

### Pushing data to your Make scenario

1. Create a new automation with Trigger
2. Click the `+` to add a step. Select `Make`.

<figure><img src="https://files.readme.io/8607c60-make.gif" alt=""><figcaption></figcaption></figure>

3. Enter the Make webhook URL and JSON payload.

> ### 👍
>
> Click on the webhook step in your Make scenario and click `Copy address to clipboard` to get the URL
>
> ![](https://files.readme.io/4972274-small-Screenshot\_2023-05-11\_at\_17.04.17.png)

When entering the payload data, your JSON payload should match the values of your Make webhook data structure. In this example it would appear as follows:

JSON

```json
{
  "value1": "ONE",
  "value2": "TWO",
  "value3": "THREE",
  "value4": "FOUR",
  "value5": "FIVE"
}
```

***

### Using bindings

Most likely you will want to send dynamic values, such as those from your automation trigger, rather than static values previously shown.

You can do this through Bindings.

If you are using an App action trigger, then the fields can be accessed like so:

<figure><img src="https://files.readme.io/ae3f35d-small-Screenshot_2023-05-11_at_18.02.41.png" alt="App action with &#x27;test&#x27; field"><figcaption><p>App action with 'test' field</p></figcaption></figure>

<figure><img src="https://files.readme.io/150e507-small-Screenshot_2023-05-11_at_18.05.20.png" alt="Using the app action &#x27;test&#x27; field value"><figcaption><p>Using the app action 'test' field value</p></figcaption></figure>

> ### 📘
>
> **Note** the binding expression in this case is surrounded in double quotes because _appValue_ is of type **Text**.
>
> For type **Number**, the quotes should not be included: `{{ trigger.fields.appValue }}`
