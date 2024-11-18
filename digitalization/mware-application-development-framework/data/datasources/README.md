# Datasources

Connecting Budibase to databases and APIs

Suggest Edits

Budibase can store your data inside the native internal database (CouchDB). This is the default datasource unless you opt to connect another datasource to Budibase.

<figure><img src="https://files.readme.io/ab0673d-Screenshot_2024-06-07_at_14.02.15.png" alt="List of app datasources"><figcaption><p>List of app datasources</p></figcaption></figure>

Budibase supports connection to a range of remote datasources. More information about connecting Budibase to each of these datasources is nested in this section of the documentation.

Furthermore, if a datasource you want to use is missing, it is possible to build a [custom datasource](https://docs.budibase.com/docs/custom-datasource).

### Schema

When Fetching tables from external SQL databases, Budibase will attempt to match the external data types to the internal Budibase DB types. These default types can be changed by clicking the three dots icon beside the table column, and selecting `Edit column`:

<figure><img src="https://files.readme.io/1f936db-edit_col.png" alt="Edit column"><figcaption><p>Edit column</p></figcaption></figure>

<figure><img src="https://files.readme.io/b7df37a-Screenshot_2023-08-11_at_09.05.40.png" alt=""><figcaption></figcaption></figure>

For other basic datasources and Custom queries, the schema will be determined by selecting types in the query output.

<figure><img src="https://files.readme.io/95963a3-Screenshot_2022-11-25_at_11.40.20.png" alt=""><figcaption></figcaption></figure>

The schema is important, as it will affect how sorting and filtering is applied in the design section.

### Sample data

Budibase provides the option of adding some sample data to allow new users to explore the various Data types available, and quickly build an app for learning purposes. While it is an internal Budibase DB source, it will be kept separate to the other internal DB tables that you create.

<figure><img src="https://files.readme.io/059e435-Screenshot_2024-01-16_at_10.29.58.png" alt="Use sample data"><figcaption><p>Use sample data</p></figcaption></figure>

If you do not need this data, it can easily be removed in the same way as other additional datasources. Simply click on the ellipsis to the right of the sample data source and click _Delete_.

<figure><img src="https://files.readme.io/fbec395-Screenshot_2024-01-16_at_10.30.56.png" alt="Sample data"><figcaption><p>Deleting sample data</p></figcaption></figure>

### Security

When connecting to an external datasource you must allow Budibase access. Normally datasources will be protected by a firewall and you will need to open firewall access to your datasource from the Budibase cloud or from your self-hosted Budibase. Port numbers and user management differ across each type of datasource so if you have difficulty connecting you should speak to the Database Administrator for your organisation.

\


***

\


## Custom queries

Budibase supports the ability to write custom queries. These queries can be really useful if you want to perform advanced joins or server-side filtering.

To write custom queries, head over to your Datasource, and scroll down until you find the queries section. Click the `Add Query` button, which will guide you to a form to configure your query.

> ### 📘Budibase DB
>
> The internal Budibase DB is a unique datasource that does not support custom queries.\
> It does however support Views

An example would be to query all pet names from a specific owner. For this demonstration we've used a custom binding to insert the `ownerID` into the query.

<figure><img src="https://files.readme.io/3ffa1e1-4a73ef1-query-owners-pets.png" alt=""><figcaption></figcaption></figure>

> ### 📘Read or Write
>
> If you want to use a custom query with a [Data provider](https://docs.budibase.com/docs/data-provider), you must select **Read** as the function of your query.
>
> Other query types, such as Create, can be used by the [Execute Query action](https://docs.budibase.com/docs/data-sources#tutorial-posting-form-data-to-a-custom-query).

### Using query bindings

You can also override the default binding values.

When you use the query as a data source for your provider, you can click on the cog icon.

<figure><img src="https://files.readme.io/f4c82ad-Screenshot_2022-04-04_at_14.52.37.png" alt=""><figcaption></figcaption></figure>

This will show a query bindings drawer where you can add a dynamic value to override the default.

<figure><img src="https://files.readme.io/42caffe-Screenshot_2022-04-04_at_14.59.01.png" alt=""><figcaption></figcaption></figure>

### Editing custom queries

A list of your custom queries can be found underneath the associated data sources. Click on the query to be able to edit the config.

It is also possible to **delete** queries that are no longer needed, or **duplicate** queries, which can useful in cases where an additional query only requires minor changes.

To access these options, hover over the query and click the **…**

<figure><img src="https://files.readme.io/4f2e995-Screenshot_2022-05-09_at_08.59.35.png" alt=""><figcaption></figcaption></figure>

### Tutorial: Posting form data to a custom query

1. Add your query to a datasource, in this example we'll use PostgreSQL. Add your bindings with some optional defaults, and then reference them within your query.

<figure><img src="https://files.readme.io/c3c5a07-Screenshot_2022-11-15_at_11.15.59.png" alt=""><figcaption></figcaption></figure>

2. Add a Form with fields for each of the bindings, except for the _id_ which will come from the [URL ID](https://docs.budibase.com/docs/url-parameters) binding.

<figure><img src="https://files.readme.io/7ebc1ef-Screenshot_2022-11-15_at_12.47.12.png" alt=""><figcaption></figcaption></figure>

3. **Define actions** for the 'Submit' button. Choose the **Execute Query** action and pick _PostgreSQL_ as the datasource. You should now be able to see and select your custom query.

<figure><img src="https://files.readme.io/b2e874e-Screenshot_2022-11-15_at_12.49.34.png" alt=""><figcaption></figcaption></figure>

4. Finally map your form field bindings to your custom query bindings like so:

<figure><img src="https://files.readme.io/382af3b-Screenshot_2022-11-15_at_12.51.51.png" alt=""><figcaption></figcaption></figure>

Click **Save** to finish.
