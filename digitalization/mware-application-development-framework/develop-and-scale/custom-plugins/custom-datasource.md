# Custom datasource

### Purpose

Datasource plugins are simple in their nature, allowing you to write an extension  that defines external [CRUD](https://en.wikipedia.org/wiki/Create,\_read,\_update\_and\_delete) operations that are useful to your business. Datasource plugins are data centric, focused on getting the data you need into the system so that you can build apps quickly and easily.

### Getting started

Make sure you have the latest CLI installed. Even if you have previously installed the CLI, you may want to install it again to be sure you have the latest version.

Also make sure that you have **node v16+** installed. You can run `node --version` to see if you need to upgrade.

Please note that datasource plugins can only be used in self host installations - they cannot be imported to the Cloud.

Once that's done, we need to initialise a template for our datasource.

Navigate to the folder location you want to create your plugin, and execute the following command:

Shell

```shell
plugins --init datasource
```

Next you will receive a series of prompts to enter details about your datasource. You can press enter without providing a value if you are happy to use the placeholder.

<figure><img src="https://files.readme.io/a8176c5-Screenshot_2022-09-16_at_09.34.23.png" alt=""><figcaption></figcaption></figure>

Within the newly created custom datasource project you should see a tree structure as follows:

YAML

```yaml
src:
  datasource.ts: The code for your custom datasource. Do not rename.

test:
  index.spec.ts: A Jest test file that includes template unit tests for each CRUD method.
  
package.json: The version, license and dependencies for your datasource.

schema.json: The meta data and available builder settings for your datasource. 
```

\


### Building your datasource

After you have created your new datasource directory, execute the following:

Shel

```shell
cd my-datasource
yarn build
```

Assuming you have updated the server env variable you should now be able to see your datasource template:

<figure><img src="https://files.readme.io/23a1d4c72cb27cabcba529cd55577ae4d2aff8c66cfd06c3a010efb24f5fb6a8-customdatasource_2.png" alt=""><figcaption></figcaption></figure>

#### datasource.ts

This is where your code will live for your datasource. It is recommended that it is developed in TypeScript. The default implementation is a basic HTTP connector, but you can make use of a wide range of npm packages to build the desired integration.

To make sure your saved changes are passed through to your server, you must execute a `yarn watch` within your custom datasource project. This will allow your local app builder to see those changes in real-time, which is very handy for getting your datasource to behave just right.

As the file is TypeScript, it is recommended that you use the **import** keyword over **require**. E.g

> ### 👍
>
> JavaScript
>
> ```javascript
> import Surreal from "surrealdb.js"
> ```

> ### ❗
>
> JavaScript
>
> ```javascript
> const Surreal = require("surrealdb.js");
> ```

**Methods**

The `datasource.ts` file contains four CRUD methods which must be implemented.

The signature of each method should match the definition provided by the [Schema](https://docs.budibase.com/docs/custom-datasource-schema).

You will also notice a `request` method in the created template. This is not mandatory, however it can be useful to have a method that executes shared code between your CRUD methods.

\


#### Test

The `index.spec.ts` test file includes template unit test methods for each CRUD method in your datasource. You can implement these to meet your own specification, however failing tests wont prevent the datasource from being built and used within the builder.

\


#### Schema

On the next page we will discuss the intricacies of the datasource schema.json file - which defines the inputs and outputs of your datasource and its queries.

You can jump to the datasource schema page [here](https://docs.budibase.com/docs/custom-datasource-schema).

\


#### Adding an icon

By default your custom datasource will present with the following icon:

<figure><img src="https://files.readme.io/b5a4d7b3efee43a1b71ec49a7f53fc1a1b1d10df603dc7e7fafbf9b4edea1d3d-Screenshot_2024-10-23_at_14.08.03.png" alt="Default icon"><figcaption><p>Default icon</p></figcaption></figure>

It is nice to add an icon to your custom datasource so that it is distinguishable from other custom datasources. Simply add an **svg** file to the root of your project. It should be called `icon.svg`.

<figure><img src="https://files.readme.io/f365c8f06eafdf55fb54080bd4b517afb88ffa9cf5838ff0de85a78a2007e4a5-icons.png" alt="Custom icon examples"><figcaption><p>Custom icon examples</p></figcaption></figure>

\


### Example: n8n workflow connector

This tutorial will show you how to build an n8n connector that can perform CRUD operations on workflows.

This example will assume you have configured the server [plugins directory env var](https://docs.budibase.com/docs/custom-plugin#hot-reloading-developers-only)

#### Step 1 - Initialise

Initialise your custom datasource as follows:

<figure><img src="https://files.readme.io/8a6bc30-cli-create-n8n.png" alt=""><figcaption></figcaption></figure>

Next make sure the dev environment is running.

Open the newly created custom datasource directory in your IDE, for example VSCode, and run `yarn watch`. Verify your plugin is available in the plugins list in the portal.\


<figure><img src="https://files.readme.io/07581d15c38d1b3b8cef0387e10dcb2d27ee90b76414f39fd9bd986807567c2b-customdatasource3.png" alt=""><figcaption></figcaption></figure>

#### Step 2 - Schema updates

Update the `schema.json` to include the **host** and **apiKey** settings.

<figure><img src="https://files.readme.io/a93cb46-schema-change.png" alt=""><figcaption></figcaption></figure>

You can also update the **docs** link and **friendlyName**.

Next update the **read** property of the schema. This will determine the available fields available for reading our n8n workflows. It should look as follows:

<figure><img src="https://files.readme.io/39754ab-schema-read.png" alt="" width="188"><figcaption></figcaption></figure>

You will need to refresh your builder webpage to pull in schema changes. To test things so far, if you add a new datasource it should look as follows:

![](https://files.readme.io/fa7cbd4-add\_datasource.png)

![](https://files.readme.io/2e35118-Screenshot\_2022-09-16\_at\_20.03.58.png)

And the **Read** query:

![](https://files.readme.io/38e3962-Screenshot\_2022-09-16\_at\_20.05.22.png)

\


#### Step 3- Read method implementation

Whilst we have provided the interface to let the system know which fields to expect for the custom datasource read method, we still have to write the implementation.

First lets replace the existing config properties with the new ones: **host** and **apiKey**. Replace all references to the old properties with the new ones throughout the file.

![](https://files.readme.io/3ae534b-constructor.png)

Next update the `read` method as follows:

JavaScript

```javascript
async read(query: { id: number, active: string, tags: string, limit: number, cursor: string }) {
    const opts = {
      method: "GET",
    }
    if (query.id > -1) {
      return this.request(`${this.host}/api/v1/workflows/${query.id}`, opts)
    }
    let queryParams = ""
    if (query.active) {
      queryParams += `&active=${query.active}`
    }
    if (query.tags) {
      queryParams += `&tags=${query.tags}`
    }
    if (query.limit) {
      queryParams += `&limit=${query.limit}`
    }
    if (query.cursor) {
      queryParams += `&cursor=${query.cursor}`
    }
    return this.request(`${this.host}/api/v1/workflows?${queryParams}`, opts)
  }
```

If an ID is provided then get the data for that particular workflow, otherwise grab all workflows using the optional query params.

Finally, the n8n API expects the **apiKey** in the header, so we can add that in the `request` method as all calls will need authenticated.

![](https://files.readme.io/83ff3cb-Screenshot\_2022-09-16\_at\_21.21.34.png)

The official n8n API workflow docs can be found [here](https://docs.n8n.io/api/api-reference/#tag/Workflow).

**Get all workflows**

![](https://files.readme.io/722fa7b-Screenshot\_2022-09-16\_at\_21.23.51.png)

A similar process can be followed to implement the Create, Update and Delete methods. Remember to update the schema as needed.

\


#### Step 4 - Adding an additional method

So far we have demonstrated how to implement CRUD methods for a custom datasource, but what if we want to add additional methods? For example, the n8n workflows API includes CRUD operations, but it also has endpoints for activating/deactivating workflows.

First add the additional method properties to the schema like so:

![](https://files.readme.io/18a2be4-Screenshot\_2022-09-17\_at\_15.40.57.png)

Upon refreshing your local builder webpage (you may also need to navigate out of your app and back in), then you should see the changes:

![](https://files.readme.io/e1de7f8-Screenshot\_2022-09-17\_at\_15.46.08.png)

Next add another method to your `CustomIntegration` class as follows:

JavaScript

```javascript
async activate(query: { id: string, active: string }) {
    const opts = {
      method: "POST",
    }
    if (query.active === "true") {
      return this.request(`${this.host}/api/v1/workflows/${query.id}/activate`, opts)
    }
    if (query.active === "false") {
      return this.request(`${this.host}/api/v1/workflows/${query.id}/deactivate`, opts)
    }
    throw new Error("Active must be 'true' or 'false'")
  }
```

Depending on the state of the active flag, the `activate` or `deactivate` endpoint is called. An error is thrown if a boolean is not provided.\


### Example: SQL query

The previous example shows you how to include functions with _field_ type. You may want your custom datasource to support SQL queries with basic syntax highlighting.

> ### 📘
>
> This example is taken from the [SurrealDB custom datasource integration](https://github.com/melohagan/budibase-datasource-surrealdb)

1. First update your schema to include a function that takes a _sql_ type function.

![](https://files.readme.io/614c131-Screenshot\_2022-09-20\_at\_13.09.00.png)

2. Next include the `SqlQuery` type

![](https://files.readme.io/8eaf7d2-Screenshot\_2022-09-20\_at\_13.15.28.png)

3. Finally update your method signature to include the _query_ parameter. You can access the SQL string via `query.sql`

![](https://files.readme.io/1efa0b7-Screenshot\_2022-09-20\_at\_13.16.56.png)

**Result**

![](https://files.readme.io/9686598-Screenshot\_2022-09-20\_at\_13.17.32.png)



### Example: Extra fields for handling modes

So far we have looked at examples where we perform CRUD operations within a single domain/mode. In this example we will look at handling different modes for when you want to mostly share CRUD functionality, but make slightly different calls depending on the selected mode.

Update the schema to include the `extra` property. In this case it was placed _after_ the `query` property:

json

```json
"extra": {
      "type": {
        "displayName": "Type",
        "type": "list",
        "required": true,
        "data": {
          "create": [
            "Subscriptions",
            "Subscription Items",
            "Subscription Schedules"
          ],
          "read": [
            "Subscriptions",
            "Subscription Items",
            "Subscription Schedules"
          ],
          "update": [
            "Subscriptions",
            "Subscription Items",
            "Subscription Schedules"
          ],
          "delete": [
            "Subscriptions",
            "Subscription Items",
            "Subscription Schedules"
          ],
          "list": [
            "Subscriptions",
            "Subscription Items",
            "Subscription Schedules"
          ],
          "searchSubscriptions": [
            "Subscriptions"
          ],
          "releaseSchedule": [
            "Subscription Schedules"
          ]
        }
      }
    }
```

The lists indicate which options are available depending on which function has been selected. For example:

<figure><img src="https://files.readme.io/3ae1a76-Screenshot_2022-09-20_at_17.44.50.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/3c1c52d-Screenshot_2022-09-20_at_17.45.27.png" alt=""><figcaption></figcaption></figure>

Next we can use the _extra_ information to make a decision about which endpoint to hit. For example, the **create** method can be changed to the following:

JavaScript

```javascript
async create(query: { json: object; extra: { [key: string]: string } }) {
    if (query.extra.type === "Subscriptions") {
      return await this.stripe.subscriptions.create(query.json as Stripe.SubscriptionCreateParams)
    }
    if (query.extra.type === "Subscription Items") {
      return await this.stripe.subscriptionItems.create(query.json as Stripe.SubscriptionItemCreateParams)
    }
    if (query.extra.type === "Subscription Schedules") {
      return await this.stripe.subscriptionSchedules.create(query.json as Stripe.SubscriptionScheduleCreateParams)
    }
    throw new Error("You must provide a type!")
  }
```
