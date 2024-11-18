# Firestore

Cloud Firestore is a flexible, scalable database for mobile, web, and server development from Firebase and Google Cloud.

### Setup

To get started, add a project through the Firebase console and create a Firestore database:\
[https://firebase.google.com/docs/firestore/quickstart](https://firebase.google.com/docs/firestore/quickstart)

### Connect

You will need to use a **Service Account** in order to be able to access your Firestore via the Budibase connector.

To get the required credentials, within the Firebase console, click on the cog icon beside _Project Overview_ and then select the _Service accounts_ tab.

<figure><img src="https://files.readme.io/b2b801a-Screenshot_2022-06-10_at_15.15.39.png" alt=""><figcaption></figcaption></figure>

Next, click **Generate new private key** button. This will download a json file that will contain the credentials you need to connect.

In particular:

* client\_email
* private\_key
* project\_id

Whenever you add a new Firestore Data Source you will be prompted to enter these fields:

<figure><img src="https://files.readme.io/e7f5de2-Screenshot_2022-06-10_at_15.29.52.png" alt=""><figcaption></figcaption></figure>

With your Firestore config saved, you will now be able to add a query.

### Create query

1. Select the 'Create' function
2. Enter a collection to save the new document to
3. Enter the JSON payload into the query box
4. The newly created document will be returned with a randomly generated unique id

<figure><img src="https://files.readme.io/ba27a2d-Screenshot_2022-06-10_at_16.49.14.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/9528acc-Screenshot_2022-06-10_at_16.50.01.png" alt=""><figcaption></figcaption></figure>

**Bindings**

You can also use [bindings](https://docs.budibase.com/docs/custom-queries#using-query-bindings) like so:

<figure><img src="https://files.readme.io/3ef13c8-Screenshot_2022-06-10_at_16.52.48.png" alt=""><figcaption></figcaption></figure>

### Read query

For these examples, there is a collection named _test_ with five documents representing various fruit and veg.\
Select the 'Read' function.

**Get all documents from a collection**

1. Enter the name of the collection into the collection field
2. Leave the filter fields blank

<figure><img src="https://files.readme.io/fa78832-Screenshot_2022-06-10_at_16.05.14.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/264e439-Screenshot_2022-06-10_at_16.09.23.png" alt=""><figcaption></figcaption></figure>

**Using filter fields**

These map to the parameters of the where clause

1. Enter the field name: 'color'
2. Select the 'equals' filter comparison
3. Enter the filter value: 'yellow'

<figure><img src="https://files.readme.io/3ff4303-Screenshot_2022-06-10_at_17.11.17.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/c3a3ef4-Screenshot_2022-06-10_at_16.15.51.png" alt=""><figcaption></figcaption></figure>

The full list of supported filter comparisons can be found [here](https://firebase.google.com/docs/reference/js/firestore\_#wherefilterop)

**Bindings**

You can also use bindings like so:

<figure><img src="https://files.readme.io/9dda221-Screenshot_2022-06-10_at_16.27.03.png" alt=""><figcaption></figcaption></figure>

### Update query

Select the 'Update' function.

**Update by id**

1. Enter the name of the collection into the collection field
2. Leave the filter fields blank
3. In the query box, enter the **id** to match on, and the field values to update to

<figure><img src="https://files.readme.io/90dd6e1-Screenshot_2022-06-10_at_16.59.04.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/805e8bd-Screenshot_2022-06-10_at_16.59.30.png" alt=""><figcaption></figcaption></figure>

### Delete query

Select the 'Delete' function.

**Delete by id**

1. Enter the name of the collection into the collection field
2. Leave the filter fields blank
3. In the query box, enter the **id** to match on

<figure><img src="https://files.readme.io/0f746de-Screenshot_2022-06-10_at_17.04.58.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/7c72644-Screenshot_2022-06-10_at_17.05.15.png" alt=""><figcaption></figcaption></figure>

As with other data sources, you can Transform any query results.

For example, you may want to access the latitude and longitude from a GeoPoint

### Sub-collection

To query a sub-collection simply provide the path within the _Collection_ field

<figure><img src="https://files.readme.io/d646220-Screenshot_2022-08-16_at_14.39.43.png" alt=""><figcaption></figcaption></figure>

![](https://files.readme.io/98222f8-Screenshot\_2022-08-16\_at\_14.41.17.png)

You can also use bindings within the collection path like so:

<figure><img src="https://files.readme.io/89e4c5c-Screenshot_2022-08-17_at_08.54.33.png" alt=""><figcaption></figcaption></figure>
