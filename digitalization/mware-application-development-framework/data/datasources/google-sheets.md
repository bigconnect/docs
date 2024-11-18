# Google Sheets

Google Sheets is a spreadsheet program included as part of the free, web-based Google Docs Editors suite.

### Setup

Navigate to the spreadsheet you want to integrate into Mware Application Development Framework . Copy the URL.

<figure><img src="https://files.readme.io/5feaf25-Screenshot_2022-09-05_at_11.21.27.png" alt=""><figcaption></figcaption></figure>

### Connect

Click the `Add source` button to add a datasource, and then select 'Google Sheets'.

<figure><img src="https://files.readme.io/1123c56-Screenshot_2023-06-27_at_12.10.36.png" alt=""><figcaption></figcaption></figure>

You will then be prompted to sign-in with a Google account. This account should have permissions to view the spreadsheet you want to work with.

<figure><img src="https://files.readme.io/096a6ca-Screenshot_2022-09-05_at_11.27.31.png" alt=""><figcaption></figcaption></figure>

Allow Mware Application Development Framework to access your Google account.

<figure><img src="https://files.readme.io/b32a6e1-Screenshot_2022-09-05_at_11.29.01.png" alt=""><figcaption></figcaption></figure>

> ### ❗️Self-hosting
>
> If you are self-hosted, you will need to setup [Google SSO](https://docs.budibase.com/docs/sso-with-google)

Once you have authenticated with Google, you will be asked to provide your Google Sheet URL

<figure><img src="https://files.readme.io/e03a8b1-Screenshot_2023-06-27_at_12.26.03.png" alt=""><figcaption></figcaption></figure>

Once it successfully connects to your Google Sheet, you can select which worksheets to import (or skip):

<figure><img src="https://files.readme.io/fb85e77-Screenshot_2023-06-27_at_12.26.10.png" alt=""><figcaption></figcaption></figure>

> ### 🚧Header row needed
>
> Your worksheets will need to have a header row to successfully import into Mware Application Development Framework .

### Data view

Each sheet can be viewed and edited as a grid within the builder.

<figure><img src="https://files.readme.io/311a261-Screenshot_2023-06-27_at_12.33.45.png" alt=""><figcaption></figcaption></figure>

You can edit columns, add rows, edit rows and delete rows via the Grid interface. Changes made in Mware Application Development Framework will be reflected in that sheet of your Google spreadsheet.

<figure><img src="https://files.readme.io/16e9bdc-Screenshot_2023-06-27_at_12.34.04.png" alt=""><figcaption></figcaption></figure>

In addition, Google Sheets data tables can utilise the row actions, such as Save Row and be used as a Form schema

### Query

The Google Sheets connector supports adding queries for when you might need to transform data or perform CRUD operations with specific bindings.

To add a query, select your Google spreadsheet datasource, and click `Add query` at the bottom of the configuration page.

<figure><img src="https://files.readme.io/a9adb33-Screenshot_2022-09-05_at_12.18.47.png" alt=""><figcaption></figcaption></figure>

#### Create

Allows you to insert a row. You must provide the name of the sheet as well as a JSON payload to specify the column names and value.

<figure><img src="https://files.readme.io/4427cd3-Screenshot_2022-09-05_at_13.04.36.png" alt=""><figcaption></figcaption></figure>

The row number of the created entry will be returned on success.

<figure><img src="https://files.readme.io/1109724-Screenshot_2022-09-05_at_13.08.11.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/b80505f-Screenshot_2022-09-05_at_13.09.01.png" alt=""><figcaption></figcaption></figure>

#### Read

Simply returns all the rows of a specified sheet. The transformer allows you to filter and map the data into the schema you desire.

#### Update

Similar to _Create_ in that you must provide a payload of data, however you must also provide a row index to be overwritten.

> ### 📘Row index
>
> Row indexing starts from zero - the first row below the header row (usually the second row).

<figure><img src="https://files.readme.io/ed9e641-Screenshot_2022-09-05_at_17.04.58.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/052333b-Screenshot_2022-09-05_at_17.05.38.png" alt=""><figcaption></figcaption></figure>

#### Delete

The same as _Update_, a sheet name and row index must be provided. That row will be deleted.

<figure><img src="https://files.readme.io/d755faf-Screenshot_2022-09-05_at_17.07.56.png" alt=""><figcaption></figcaption></figure>

Rows will be shifted up after deletion.

<figure><img src="https://files.readme.io/07371b1-Screenshot_2022-09-05_at_17.09.23.png" alt=""><figcaption></figcaption></figure>
