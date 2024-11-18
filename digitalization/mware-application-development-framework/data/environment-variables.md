# Environment variables

Configuration for apps which switches per environment

Environment variables provide a mechanism for easily re-using secrets/datasource configuration across different apps, as well as allowing for different values to be used in development apps versus production apps. The environment variable configuration can be found under Settings, then Environment.

This feature is most useful when working with data that is already used in production or updating an app that is already being used. For example lets say you have two Postgres databases, one used locally for testing and one used in production for the applications real data. Environment variables can be utilised in this scenario to allow the connection details for the two different databases to be parameterised and changed depending on whether the app is in development, or in production.

### Unlocking environment variables

If you are using the free tier you will see the following under the environment variables section:

<figure><img src="https://files.readme.io/f7c0223-image.png" alt=""><figcaption></figcaption></figure>

You can click Upgrade to unlock environment variables or click `View Plans` to navigate to the pricing page to see the full range of options in more detail.

### Managing environment variables

Once you have navigated to the environment variable section of the Mware Application Development Framework portal, you will see an option to add a new variable as shown below, but you can also edit and delete your environment variables too.

<figure><img src="https://files.readme.io/140b9f5-image.png" alt=""><figcaption></figcaption></figure>

Once the modal opens you'll be able to set the environment variable name and its value in production and development. Note that once these values have been set they will be encrypted (uses **AES-256** encryption), stored and you will no longer be able to retrieve it. By default this will use the same value for development as production, incase you have not yet setup two separate datasources, this way you can build your apps utilising environment variables, then later update them if you want to separate the development and production values.

![](https://files.readme.io/543d5d3-image.png)

> ### 🚧Environment variable naming
>
> It is important to pick recognisable names for your environment variables, as these will be how you pick which variables to use within your apps. Your variables also cannot contain spaces, we recommend the use of underscores instead.

At this point you should have created your required environment variables and they should have appeared within the variable table list. From here you can also edit or delete an environment variable, it is important to note the change will be immediate, if you change a value or remove an environment variable, production apps will be affected.

<figure><img src="https://files.readme.io/07c2455-image.png" alt=""><figcaption></figcaption></figure>

To edit or delete an existing environment variables, click the `Edit` button.

<figure><img src="https://files.readme.io/31a150a-Screenshot_2023-08-29_at_10.04.42.png" alt=""><figcaption></figcaption></figure>

### Using environment variables in datasources

Once you've configured some environment variables we can progress to creating or updating an app to use these new variables. Enter an app and create or update a datasource from the data section, in this example we will be creating a Postgres connection. Below you can see the default Postgres connection details, you will also notice that the small key icon on the end of the input field.

<figure><img src="https://files.readme.io/013c423-image.png" alt=""><figcaption></figcaption></figure>

Clicking on this icon will open the environment variable menu, which allows you to select from a list of variables that have been created. You will also have the option to create a new environment variable.

<figure><img src="https://files.readme.io/a98e757-image.png" alt=""><figcaption></figcaption></figure>

Once configured the input fields will be disabled, with the environment variable contained within, as shown below.

<figure><img src="https://files.readme.io/9dd0d24-image.png" alt=""><figcaption></figcaption></figure>

It is important to note that while they appear to look like bindings, they cannot be used as such. The environment variable values are only available in a few key locations and cannot be used in the same way that bindings function. Once this is complete you can save the datasource and it will now utilise the variable.

At this point your app is now utilising these environment variables, within the app builder you will be using the development values, once you publish the app it will utilise the production values. It is important to note that these environments are entirely independent, you can be using the builder, with the development values for your datasources while your users are utilising the currently released version of your app and the production values.

### Using environment variables in queries

Another useful feature of environment variables is their ability to be used in [queries](https://docs.budibase.com/docs/data-sources#custom-queries) as well as datasource configuration. If you open the binding drawer you will see a new environment section has been added, as seen below.

![](https://files.readme.io/8ee9866-image.png)

Expanding the environment section will list all of the variables which are available to the query, unlike datasource configuration, these can be used as normal bindings in queries. In the example below we have configured a header in [REST query](https://docs.budibase.com/docs/rest) to use an environment variable.

<figure><img src="https://files.readme.io/0398294-image.png" alt=""><figcaption></figcaption></figure>

These can also be used in an SQL query, in the same format as the binding drawer, like `{{ env.env_variable }}` this is shown below.

<figure><img src="https://files.readme.io/d36d61a-image.png" alt=""><figcaption></figcaption></figure>

### Using environment variables in automations

The last place that variables can be accessed is as part of an automation run. This can be very useful if connecting to an external service, or triggering a notification e.g. Slack or Discord. Like queries environment variables can be used as bindings within automation steps. Below we have configured a Discord step in an automation, which utilises an environment variable for the webhook URL. Not only is this more re-usable and secure, but it also allows using different URLs for testing and for production.

<figure><img src="https://files.readme.io/2d3fc10b86748f3a3a4097218d3218735f093d87bbaf457ba325727ab9e799d9-Screenshot_2024-09-20_at_15.00.48.png" alt=""><figcaption></figcaption></figure>
