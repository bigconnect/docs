# Internal DB

The internal Mware Application Development Framework DB is our native datasource. Any data you add here is stored directly on your instance.

### Creating a table

You can create a table when you are in the **Data** section of the application builder.

Click `Add source` and then click the `Create new table` button.

<figure><img src="https://files.readme.io/44d2cff-Screenshot_2024-01-16_at_10.09.49.png" alt="260"><figcaption><p>Create a new table using this button</p></figcaption></figure>

<figure><img src="https://files.readme.io/598062a-create_table.png" alt="All auto columns selected by default"><figcaption><p>Creating a new DB table</p></figcaption></figure>

#### Adding columns

To add new columns to an existing DB table, navigate to the table you want to edit, then click the `+` button along the top-right of the grid view. This will display a popover where you can configure the column you want to add.

<figure><img src="https://files.readme.io/842339a-Screenshot_2023-08-11_at_09.01.15.png" alt="Adding a required text column"><figcaption><p>Adding a long form 'Address' field</p></figcaption></figure>

#### Editing columns

1. Step into the table through the sidebar
2. Hover over the column name you want to edit
3. Click the three dots and select _Edit column_

<figure><img src="https://files.readme.io/7db9965-edit_col.png" alt="Click the pencil icon to edit column"><figcaption><p>Click the pencil icon to edit column</p></figcaption></figure>

#### Selecting the display column

The display column is an important aspect when you're setting up relationships in tables. The column you select as a display column will be the one visually displayed inside a relationship column in another table.

Selecting a column during the table creation is only possible if you import data using a `CSV` file. To select a display column in any other situation, you can set it from the edit column dropdown.

<figure><img src="https://files.readme.io/5dfb8fc-Screenshot_2023-06-28_at_15.13.35.png" alt="Use as display column"><figcaption><p>Use as display column</p></figcaption></figure>

#### Auto-columns

After creating a new DB table you can add any number of built-in (auto types) columns you want to have. Each of these will be automatically updated whenever rows are added or updated in your table.

When adding a column, select _Auto Column_, and then the specific sub-type.

<figure><img src="https://files.readme.io/f94690a-auto_column.png" alt="Selecting the &#x27;Auto Column&#x27; data type"><figcaption><p>Selecting the 'Auto Column' data type</p></figcaption></figure>

<figure><img src="https://files.readme.io/c636bfe-Screenshot_2023-08-11_at_08.53.51.png" alt="Selecting the &#x27;Created By&#x27; auto column sub-type"><figcaption><p>Selecting the 'Created By' auto column sub-type</p></figcaption></figure>

| Name       | Description                            |
| ---------- | -------------------------------------- |
| Auto ID    | An automatically generated ID          |
| Created by | The user who created the row           |
| Created at | When this row was created              |
| Updated by | The user who updated the row last      |
| Updated at | The time when the row was updated last |

> ### 👍Create table from CSV
>
> You can Import a CSV/JSON file as a new table.

***

### Data types

Within Mware Application Development Framework DB there are several data types to choose.

| Name                 | Description                                                                                                                    |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Text                 | Storage of (relatively) short text                                                                                             |
| Long Form Text       | Allows you to store large amounts of texts, also supports markdown                                                             |
| Options              | Predefined list of options of which one can be selected                                                                        |
| Multi-select         | Predefined list of options of which multiple can be selected                                                                   |
| Number               | Storage of number                                                                                                              |
| Boolean (True/False) | Storage of true/false                                                                                                          |
| Date/time            | Storage of a date with a time                                                                                                  |
| Attachments          | Storage of a file. Limited to 20MB per file                                                                                    |
| Relationships        | Creates a link between this table and another table                                                                            |
| Formula              | Allows you to set a formulated column, which will be calculated based on what you define. You can use handlebars or JavaScript |
| JSON                 | Allows you to store JSON within a row, with the option to define a schema                                                      |
| Barcode/QR           | A text representation of a barcode or QR code                                                                                  |
| Auto Column          | Auto columns can be added post-creation of the database by selecting data type.                                                |

If using an external SQL Datasource, the native SQL data types will be mapped to one of these types for use in your apps.

### Column definitions

Common column options:

| Property Name               | Description                                                                                                               |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Name                        | The name of the column, used for querying and displaying                                                                  |
| Type                        | The type of data you want to store.                                                                                       |
| Use as table display column | Enabling this will make the newly created column the display column. This will also automatically make the field required |
| Primary index               | Enabling this allows you to search in this field                                                                          |
| Secondary index             | When primary index is in use, you can enable a second index for searching                                                 |

\
Data type specific column options:

| Property Name                   | Description                                                                                                                              | Data Types            |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| Enable Rich Text Format Support | Enabling this will allow you to store markdown                                                                                           | Long Form Text        |
| Options                         | The select-able options available for this column, one per line.                                                                         | Options, Multi-select |
| Table                           | Allow a relationship to the specified table                                                                                              | Relationship          |
| Column name in other table      | The relationship created will also be visible in the related table, this will set the new column name on the related table               | Relationship          |
| Formula                         | Allows you to set a formulated column, which will be calculated based on what you define. Allows you to define handlebars and JavaScript | Formula               |

### Constraints

The following column constraints are available:

| Constraint Name | Description                                                                          | Data Types |
| --------------- | ------------------------------------------------------------------------------------ | ---------- |
| Required        | When enabled, a value must be set for this field                                     | All        |
| Maximum Length  | The maximum length that can be stored in this column                                 | Text       |
| Min Value       | When set, any value entered in the database lower than this number will be rejected  | Number     |
| Max Value       | When set, any value entered in the database higher than this number will be rejected | Number     |
| Earliest        | When set, any date before the specified date is rejected for new rows                | Date/Time  |
| Latest          | When set, any date after the specified date is rejected for new rows                 | Date/Time  |

> ### 📘External datasource constraints
>
> You can add constraints to external table columns, however this is entirely for form validation and will not restrict the data that is saved.\
> Instead, you must add data constraints yourself to any external table schemas.

***

### Data sync between environments

When using the internal DB, there are two environments to consider.

The **Development** environment represents the data that you see and interact with when using the app builder, which includes the _Data_ and _Design_ sections, and the App preview.

If in doubt, you can see if you are in the dev environment from the URL, which will show `/app/app_dev_abc...` in the path. In production, the URL will appear without the `_dev` part.

**Production** represents your live environment. This is the data that your End Users will interact with. When in the app builder, production data will continuously sync and update your development data, although a page refresh may be needed to see the new changes.

Whenever you Publish your app, any edits, inserts or deletes made in your dev data will be pushed into production, which may overwrite existing data.

> ### 📘External datasources
>
> Things are a little different when using an external database such as SQL.\
> In this case the data in either environment will read and write to the database that you connected to.
>
> You can use Environment variables to make use of development and production databases.
