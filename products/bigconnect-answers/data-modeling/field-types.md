# Field types

While data types indicate to a database how it should interpret the values in a field, **field types** or **semantic types** describe the _meaning_ of a field. For example, a column’s data type could be `type/text` but the semantic type may be **Email**. Field types are just one example of metadata—information about data—that Admins can change in Answers.

Field types dictate how Answers displays its data, as well as the column’s special functionality, if any. By marking columns in a table as **Latitude** and **Longitude**, Answers can use the columns to create pin and heat maps. Similarly, designating a column as a **URL** allows users to click on the link to visit that URL.

## List of Answers Field Types

Answers recognizes the following field types:

* **Overall Row**
  * **Entity Key** – The field in this table that uniquely identifies each row. Could be a product ID, serial number, etc.
  * **Entity Name** – Different from the entity key, the entity name represents what each row in the table _is_. For example, in a Users table, the User column might be the entity name.
  * **Foreign Key** – The column in this table that (usually) refers to the primary key of another table in order to connect data from different tables that are related. For example, in a Products table, you might have a Customer ID field that points to a Customers table, where Customer ID is the primary key.
* **Common**
  * Category
  * Comment
  * Description
  * Title
* **Location**
  * City
  * Country
  * Latitude
  * Longitude
  * State
  * Zip Code
* **Financial**
  * Cost
  * Currency
  * Discount
  * Gross margin
  * Income
  * Price
* **Numeric**
  * Quantity
  * Score
  * Share
  * Percentage
* **Profile**
  * Birthday
  * Company
  * Email
  * Owner
  * Subscription
  * User
* **Date and Time**
  * Cancelation date
  * Cancelation time
  * Cancelation timestamp
  * Creation date
  * Creation time
  * Creation timestamp
  * Deletion date
  * Deletion time
  * Deletion timestamp
  * Updated date
  * Updated time
  * Updated timestamp
  * Join date
  * Join time
  * Join timestamp
  * UNIX Timestamp (Milliseconds)
  * UNIX Timestamp (Seconds)
* **Categorical**
  * **Enum** - An abbreviation for “enumerated type,” the value of an enum draws on a predefined list of options. An example of an enum would be a field for the months of the year. This list of twelve options is defined in makeup of the column, and no options outside this list would be valid.
  * Product
  * Source
* **URLs**
  * Avatar Image URL
  * Image URL
  * URL
* **Other**
  * Field containing JSON
  * **No semantic type** – Used for fields that don’t fall into any of the above field types.

## Using field types in Answers

### Fields types in models

You can set field types for models, which helps Answers understand how to work with data in models built using SQL. You can also configure text fields in models to surface individual records in search.

### X-rays

When you X-ray a table or model, Answers considers both the data type and the field type to display different charts that summarize that data.

#### Field Filters

Knowing what field types are and how they work is helpful when using field filters, as you can only create field filters for certain field types.

### Editing types in the Table Metadata page

If you’re an administrator, you can edit field types using the Table Metadata page in the Admin Panel.

While data types themselves can’t be edited in Answers, admins can manually cast data types to be read differently, like interpreting a numerical data type as a date format.
