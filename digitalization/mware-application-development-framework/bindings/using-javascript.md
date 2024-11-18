# Using JavaScript

Mware Application Development Framework is a low-code tool, which means, you can use code to improve your applications! But it also means in certain situations writing code is necessary. Mware Application Development Framework supports the use of JavaScript in several places to customize your applications.

> ### 🚧Limitations
>
> JavaScript within Mware Application Development Framework is server-side synchronous only. Async interactions (such as API calls, or `await`) aren't supported. There also is no DOM-interaction as the code is executed server-side.

### Using JavaScript code in Bindings

Bindings are extremely powerful tools to customize anything that's displayed in an application without the need to change how it is stored inside the database. But even more powerful is using JavaScript with bindings.

JavaScript inside bindings is executed server-side and synchronous. This means there's no async support, nor is there DOM access. Third-party libraries also cannot be added.

To use JavaScript anywhere in Bindings, you need to press the Lightning icon on any field that supports it, and then click the "JavaScript" tab.

<figure><img src="https://files.readme.io/47e80c6f01eca8acf19b2675e6f9e2120c19685ccb3201596ff04b70904c1b1c-Screenshot_2024-09-18_at_12.23.30.png" alt=""><figcaption></figcaption></figure>

In essence, the JavaScript box is a synchronous function. It expects a value to be returned and can use values from bindings, which are all listed on the left.

In this situation we've got access to a Data Provider that provides us with a list of check-ins from the Remote Fleet Management System Template.

Because we've got a list of items, we can do calculations with them. In our case, we've got a column that lists how much was spent on fuel for each check-in. Perhaps we want to sum up all the fuel values from the results.

JavaScript

```javascript
const checkins = $("Data Provider.Rows");
let totalFuelCost = 0;

for (let i in checkins) {
	totalFuelCost += checkins[i].fuel_price;
}

return "Total Fuel Cost: $ " + totalFuelCost;
```

We've used the binding `Data Provider.Rows` as that is the default binding that returns all the rows from the [Data Provider](https://docs.budibase.com/docs/data-provider).

> ### 👍Autocomplete
>
> When working with JavaScript bindings, typing the **$** key will bring up a suggestion window, which will filter as you type. Use the up/down arrow keys to select a binding, and press **Tab** to complete.
>
> ![](https://files.readme.io/d9ef780eb350f04c3a3a65f3265b06ce66f1c91b7b24a3da16a0c183b7259aeb-Screenshot\_2024-09-18\_at\_12.24.02.png)

After entering the code, you can click "Save" on the top-right, and the binding should work automatically.

![](https://files.readme.io/feadaf6-binding-result.png)

Now of course this is just a number that's being used, but you can do anything JavaScript related here.

#### Using a Date Column

Whenever you use a `Date` Column as a data type you will actually be provided with an ISO string, which is compatible with `new Date(Value)` by default.

You should also always return a value that's parseable by the JavaScript `Date` function.

JavaScript

```javascript
const date = new Date($("Value"));
date.setDate(date.getDate() + 3);
return date;
```

As an example, the code above adds 3 days to the provided Date and then returns the Date Object.

### Using JavaScript in Automation

You can learn about using data in automations in Data in Automations guide.

### Using JavaScript with Transformers

When dealing with external Data Sources you can create custom queries. But with queries comes the ability to transform the data in a way your app can consume it easier. For example, when querying a REST API, you could transform the data using JavaScript to a format that only returns what you want to return.

There's an In Depth Guide for you to dive into how to use transformers with JavaScript.
