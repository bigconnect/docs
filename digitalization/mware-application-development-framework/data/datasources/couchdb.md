# CouchDB

Apache CouchDB is an open-source NoSQL database.

CouchDB as the underlying technology of our internal Mware Application Development Framework DB.

### Connect

To start, click on the `Add source` button of the **Data** section, select _CouchDB_, and click `Continue`.

<figure><img src="https://files.readme.io/3c00104-Screenshot_2023-02-03_at_13.32.37.png" alt="CouchDB datasource"><figcaption><p>CouchDB datasource</p></figcaption></figure>

Next enter a connection URL and database to connect to. Click `Save and continue to query`.

<figure><img src="https://files.readme.io/8d8b14b-Screenshot_2023-02-03_at_13.33.28.png" alt=""><figcaption></figcaption></figure>

#### Providing a username and password

If you need to authenticate with a username and password, you can that by providing them in the URL as so:

<figure><img src="https://files.readme.io/0f121ca-Screenshot_2023-02-03_at_13.52.26.png" alt="Providing a username and password"><figcaption><p>Providing a username and password</p></figcaption></figure>

### Adding a query

You can add a query by clicking `Add query` on the CouchDB configuration page.

<figure><img src="https://files.readme.io/31cc125-Screenshot_2023-02-03_at_14.09.51.png" alt=""><figcaption></figcaption></figure>

> ### 🚧Connecting to a DB
>
> You can use this connector to connect to your internal tables, including the Users table.
>
> We recommend that you do not do this, because a bad write/delete could break your apps.
>
> If you are determined, you will need to port forward.

There are a number of query types available, which are chosen from the _Function_ dropdown.

<figure><img src="https://files.readme.io/93e9756-Screenshot_2023-02-03_at_14.15.18.png" alt=""><figcaption></figcaption></figure>

For more information on query syntax for this connector, you can look at the [PouchDB API docs](https://pouchdb.com/api.html).

#### Read

Performs an **allDocs** query against your configured database. If you choose to provide no query body, you still need to add `{}` into the box.

<figure><img src="https://files.readme.io/313251b-Screenshot_2023-02-03_at_14.22.41.png" alt=""><figcaption></figcaption></figure>

#### Get / Delete

Allows you to retrieve or remove a single document by **id**.

In this example a Binding called _doc\_id_ is being used, so that the query can be used to obtain any document specified in your app. See more here: Custom queries

<figure><img src="https://files.readme.io/8101bbd-Screenshot_2023-02-03_at_14.24.26.png" alt=""><figcaption></figcaption></figure>

#### Create / Update

More here: [https://pouchdb.com/api.html#create\_document](https://pouchdb.com/api.html#create\_document).

Provide a JSON payload. In the case of _Update_ you will need to specify a document **\_id**.

When creating a document you do not need to provide an id. An automatically generated id will be returned in the response.

<figure><img src="https://files.readme.io/66c17e8-Screenshot_2023-02-03_at_14.37.46.png" alt=""><figcaption></figcaption></figure>
