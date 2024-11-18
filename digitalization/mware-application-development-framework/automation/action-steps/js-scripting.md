# JS Scripting

While Bindings in automations support JavaScript, you can also add a step to execute a JavaScript code block.

Commonly this action is used to compare and aggregate data from multiple Datasources.\


### Adding a JavaScript code block

1. After adding an automation Trigger, click the `+` icon and select `JS Scripting`. Click `Save`.

<figure><img src="https://files.readme.io/a398627-Screenshot_2023-03-15_at_09.55.41.png" alt=""><figcaption></figcaption></figure>

2. Click the `Edit Code` button to open the code editor modal.

<figure><img src="https://files.readme.io/aed29130953a88acc142b00a2966c2f54c300bdefdaf445ea983151bd8e86777-Screenshot_2024-11-07_at_10.49.20.png" alt=""><figcaption></figcaption></figure>

> ### 📘
>
> Make sure to **return** a value at the end of your script!

3. Available bindings can be seen on the far right of your modal. These can be clicked to references within your JS scripting area.

<figure><img src="https://files.readme.io/4a4bd351786c1fec588459a4705f37d3c3f206bc00a4900d341a4b13664d63a2-Screenshot_2024-11-07_at_10.49.20.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/80053c4-Screenshot_2023-03-15_at_10.33.25.png" alt="Example output"><figcaption><p>Example output</p></figcaption></figure>

\


### Tutorial: Linking an API response to table rows

In this tutorial we will look at using a JS Scripting block to link people from an API response to people with the same name in a table.

1. Add a REST Query. This example will pull back the Top 100 Birders for a given date. Be sure to `Save` the query after `Send`.

<figure><img src="https://files.readme.io/59f5809-Screenshot_2023-03-15_at_11.20.42.png" alt=""><figcaption></figcaption></figure>

2. We will also add a DB table for the members of our mock bird club, which will include some additional information not provided by the API, such as _Twitter Username_.

<figure><img src="https://files.readme.io/505aa94-Screenshot_2023-03-15_at_11.23.58.png" alt=""><figcaption></figcaption></figure>

3. In the **Automate** tab, add an automation. This automation will fetch data from both sources and send a summary Email. I will use an App action trigger, but any other trigger could be used.

<figure><img src="https://files.readme.io/929b26e-Screenshot_2023-03-15_at_11.31.35.png" alt="Fetch the Top 100 birders from the API, and then all our club members"><figcaption><p>Fetch the Top 100 birders from the API, and then all our club members</p></figcaption></figure>

4. Add a `JS Scripting` block, and `Edit Code`. Enter the following:

JavaScript

```javascript
/*
* Compare and combine the local club members that match, by name, with the Top 100 birders from the API.
*/
const top100 = steps[1].response;
const clubMembers = steps[2].rows;

let topClubMembers = top100.filter(row => clubMembers.map(tableRow => tableRow.Name).includes(row.userDisplayName));

return topClubMembers.map(member => ({
	...member,
	...clubMembers.find(tableRow => tableRow.Name === member.userDisplayName)
}));
```

5. Finally add the `Send Email` step. Fill in the details and format the _HTML Contents_ as desired.

HTML Contents

<figure><img src="https://files.readme.io/fd1acd17b6a2a6ed542bc402b8293d6cbe4f484bf10b38e5c11d2ce6dec64391-Screenshot_2024-11-07_at_11.18.50.png" alt=""><figcaption></figcaption></figure>

```javascript
let listItems = ""
for (let member of $("steps.3.value")) {
	listItems += `<li>${member.Name} <b>@${member["Twitter Username"]}</b> 

<br />Spotted ${member.numSpecies} species!</li><br/>`
}

return `Congrats to our top bird watchers - give them a shout out on Twitter!

<p><ol>${listItems}</ol></p>`
```

6. Click `Run test` at the top of your automation to see the result!

<figure><img src="https://files.readme.io/3cc556b-Screenshot_2023-03-15_at_12.50.23.png" alt=""><figcaption></figcaption></figure>
