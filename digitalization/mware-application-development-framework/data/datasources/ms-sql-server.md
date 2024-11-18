# MS SQL Server

Connecting Mware Application Development Framework to an existing Microsoft SQL Server

> ### 👍SQL Databases
>
> Mware Application Development Framework is designed to integrate with any core SQL database.
>
> Read more about SQL Datasources.

## Connecting to SQL Server

After clicking on the SQL Server datasource you will be prompted to enter connection details:

![](https://files.readme.io/4cad850-sqlserver.png)

### Advanced authorisation

If you are using an MS SQL Server protected via Azure Active Directory, you can setup the `Advance auth` field to `Active Directory`, providing the required secrets.

<figure><img src="https://files.readme.io/a7b40f6-ad.png" alt=""><figcaption></figcaption></figure>

The Azure documentation can be found [here](https://learn.microsoft.com/en-us/sql/relational-databases/security/authentication-access/azure-ad-authentication-sql-server-setup-tutorial?view=sql-server-ver16).



***

## Tutorial: Building a Bike Store app

In this tutorial we will look at building a Mware Application Development Framework app from scratch using an existing Microsoft SQL Server database as our foundation. Our data is split between two schemas.

One schema is called **Production** which contains our products, details and stocks. This data will be available to the public for customer use.

The second schema is called **Sales** which contains internal data to be seen and managed by employees only.

The full dataset can be found on [sqlservertutorial.net](https://www.sqlservertutorial.net/sql-server-sample-database/).

### Setting up the datasources

Given you have an MS SQL Server instance running, with the dataset committed, we can begin pulling that data into Mware Application Development Framework .

First create an app - no sample data needed.

<figure><img src="https://files.readme.io/ef353f8-create_app.png" alt=""><figcaption></figcaption></figure>

As we have two schemas, we will want to create two [Datasources](https://docs.budibase.com/docs/data-sources); one for each schema.

Click `Add source` and select `MS SQL Server`. Fill out the connection details and press `Save and fetch tables`.

<figure><img src="https://files.readme.io/ec7639b-create-ds-sales.png" alt=""><figcaption></figcaption></figure>

Repeat the same process, but change the _Schema_ to **production**.

By default the datasource names will be _SQL Server_ and _SQL Server-2_. It is best to rename these to match the corresponding schemas.

For example, select the datasource for the **production** tables, change the name under configuration, and click `Save`. Do the same for **sales**.

<figure><img src="https://files.readme.io/1fa4ca6-rename-ds-prod.png" alt=""><figcaption></figcaption></figure>

Your final _Sources_ tree should look as follows:

<figure><img src="https://files.readme.io/0672e31-Screenshot_2022-12-19_at_15.27.11.png" alt=""><figcaption></figcaption></figure>

### Define existing relationships

Now that we have pulled in our tables, we need to inform Mware Application Development Framework how the tables relate to each other.

* One Brand -> Many Products
* One Category -> Many Products
* One Store -> Many Staff
* One Store -> Many Orders
* One Staff -> Many Orders
* One Customer -> Many Orders

To get started, click on the `brands` table. Next click on the `Define existing relationship` button along the top of the table.

<figure><img src="https://files.readme.io/282c97d-Screenshot_2022-12-19_at_15.53.58.png" alt=""><figcaption></figcaption></figure>

This will pop up a dialog for you to select the `products` table as the _to table_, and select the _brand\_id_ as the foreign key.

<figure><img src="https://files.readme.io/838d21f-define_relationship.png" alt=""><figcaption></figcaption></figure>

Click `Save` and you will see the relationship links successfully added in both tables.

<figure><img src="https://files.readme.io/4973af1-Screenshot_2022-12-19_at_15.58.58.png" alt="Product links in the brands table"><figcaption><p>Product links in the brands table</p></figcaption></figure>

<figure><img src="https://files.readme.io/5cad2d4-Screenshot_2022-12-19_at_16.00.16.png" alt="Brand links in the products table"><figcaption><p>Brand links in the products table</p></figcaption></figure>

The same process can be followed for the other desired relationships mentioned. Alternatively you can [define relationships](https://docs.budibase.com/docs/sql-datasource#defining-relationships) from the datasource configuration page.

### Adding a public products catalogue screen

In this tutorial we want to allow customers to browse the available products of the bike store. First we must give public users permissions to view the data. For each table under the **Production** datasource, click on `Manage access`.

<figure><img src="https://files.readme.io/0d0bb56-Screenshot_2022-12-19_at_16.13.28.png" alt=""><figcaption></figcaption></figure>

This will display a modal for setting the read and write access levels. As we only want the public to be able to read the data, select the **Public** role for _Read_ and click `Done`.

<figure><img src="https://files.readme.io/367bf5e-Screenshot_2022-12-19_at_16.12.01.png" alt="Anyone can read the products, but write require authorization"><figcaption><p>Anyone can read the products, but write require authorization</p></figcaption></figure>

> ### 🚧Manage access for relationships
>
> When making data public, you must also give the related data public access.\
> In this case, `products`, `brands` and `categories` must all have public read access.

With the data permissions now set, we can add our screen into the [Design](https://docs.budibase.com/docs/working-with-data) section. Click on the **Design** tab, and select the `/home` screen. Under access, select **Public** so that anyone can view the [Screen](https://docs.budibase.com/docs/screens).

<figure><img src="https://files.readme.io/fe00aa4-Screenshot_2022-12-19_at_16.19.45.png" alt="Public screen access"><figcaption><p>Public screen access</p></figcaption></figure>

Next click on `Add component` and add a [Cards block](https://docs.budibase.com/docs/blocks#cards-block). Select `products` as the datasource, and fill out the cards title, subtitle and description. Use the Binding from the dropdowns to pull data from the SQL datasource.

<figure><img src="https://files.readme.io/bae8846-Screenshot_2022-12-19_at_16.24.49.png" alt="Selecting the product name as the card title"><figcaption><p>Selecting the product name as the card title</p></figcaption></figure>

Bindings also allow you to combine static text with dynamic data pulled from your SQL table. For example, we can prefix the price with a dollar sign using the following handlebars expression:

```
${{ Products Cards block.products.list_price }}
```

<figure><img src="https://files.readme.io/fef6581-Screenshot_2022-12-19_at_16.29.21.png" alt=""><figcaption></figcaption></figure>

You can tick _paginate_ and adjust the _limit_ setting to determine how many products should be displayed per page.

Of course given a catalogue of products, users would like to be able to filter. You can easily add a _Search column_ via the dropdown in the cards block.

<figure><img src="https://files.readme.io/966d7e4-Screenshot_2022-12-19_at_17.23.51.png" alt="Adding search columns"><figcaption><p>Adding search columns</p></figcaption></figure>

For example, you may want to allow users to search by product name, brand name, or model year.

![](https://files.readme.io/7531e12-Screenshot\_2022-12-19\_at\_17.25.11.png)

It would also be nice to add in some tags for the brand and category of the product. To do this let's eject the block. This will give us a more granular component tree like so:

<figure><img src="https://files.readme.io/a1304b1-Screenshot_2022-12-19_at_17.26.11.png" alt="Ejected cards block"><figcaption><p>Ejected cards block</p></figcaption></figure>

With access to the Repeater, we can now add a couple of [Tag](https://docs.budibase.com/docs/tag) components below the _Spectrumcard_; one each for the brand and category.

Use the following binding for the tag text: `{{ Repeater.products.brands.0.primaryDisplay }}`

To explain, _brands_ is the name of the relationship field. The `.0` is required as relationship fields are always considered to be an array, however in this case we know that it will only ever be one element. Finally the `primaryDisplay` corresponds to the table display column.

Using Containers and a bit of styling, you can create something like this:

<figure><img src="https://files.readme.io/6dce82d-Screenshot_2022-12-19_at_17.29.15.png" alt=""><figcaption></figcaption></figure>

Finally click the `Preview` button in the top-right corner of the screen to test the app so far.

Select `View as public user` to make sure unauthenticated users can view the data.

<figure><img src="https://files.readme.io/d51c1c2-Screenshot_2023-02-03_at_11.57.14.png" alt=""><figcaption></figcaption></figure>

### Managing sales data

Whilst we want the public to be able to view the available products, the sales data should be managed by authenticated users.

After setting up your relationships, as outlined earlier in the tutorial, go to the **Design** section and click `Add screen`. Next click on `Autogenerated screens`.

This will prompt you to select the table to create the [Autogenerated screen](https://docs.budibase.com/docs/table-block#autogenerated-screen) for. Select the `orders` table and `Confirm`.

<figure><img src="https://files.readme.io/214415d-7dc103f-Screenshot_2022-12-19_at_17.44.01.png" alt=""><figcaption></figcaption></figure>

Next you will be prompted to select an access level. By default **Basic** will be selected, which requires an authenticated user of the app. Click `Done`.

This will add full CRUD functionality for the `orders` table. If you want to customise this screen, look at the [Table block](https://docs.budibase.com/docs/table-block) page for more info.

<figure><img src="https://files.readme.io/1b862bb-Screenshot_2022-12-19_at_17.51.42.png" alt="Basic logged in user can view and edit orders"><figcaption><p>Basic logged in user can view and edit orders</p></figcaption></figure>

Whilst public users will be denied access to the _Orders_ screen, it would be better to not show them the Navigation link so as to avoid confusion.

Click on the navigation panel, and click `Configure links`. Next beside `/orders` select a _Minimum role_ - in this case **Basic**. Click `Save`.

<figure><img src="https://files.readme.io/11c6448-Screenshot_2022-12-19_at_18.04.07.png" alt=""><figcaption></figcaption></figure>

This will ensure that public users do not see the _Owners_ link.

<figure><img src="https://files.readme.io/932dad0-Screenshot_2022-12-19_at_18.05.35.png" alt="Unauthenticated user only sees the Home link"><figcaption><p>Unauthenticated user only sees the Home link</p></figcaption></figure>

Auto-generated screens can be created for the other **sales** tables, but different [access levels](https://docs.budibase.com/docs/user-roles#app-specific-roles) could be used.
