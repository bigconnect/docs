# Database Fundamentals

## A short overview of databases

#### How tables, rows, columns, and keys work together to make a database.

If you’re going to get started using Answers, it’s helpful to know a few key database terms.

### Tables

Fundamentally, databases are made up of one or more tables. Tables contain one or more columns and one or more row. A row is made up of _cells_, and each cell has a _value_ that corresponds to the column it falls under.

Here’s an example of a table:

| Name  | Age |
| ----- | --- |
| John  | 25  |
| Jenny | 31  |

Here, the columns are `Name` and `Age`. The first row contains two cells, one with `John` and one with `25`, corresponding to the Name and Age columns, respectively.

### Columns

All the cells in a column contain the same type of information. For example, in the sample table above, the `Name` column contains names in each cell, while the `Age` column lists ages.

Each field has a type that describes what kind of data is stored in the field.

#### Columns vs fields

A note about columns and fields, as these terms can be used interchangeably:

* A **field** is an element for storing data (e.g., the `PRODUCT_ID` field stores identification codes for products).
* A **column** is a list of values, and most often a list of values from a single field (e.g., the `PRODUCT_ID` column stores values from the `PRODUCT_ID` field). A column can also, however, be a list of values from multiple fields. For example, a column might contain values from an expression that computes the difference of values from two different fields: a `TOTAL_WITH_DISCOUNT` column, for example, could take values from the `DISCOUNT` field and subtract them from values in the `SUBTOTAL` field, and list the difference.

In Answers (and elsewhere) you’ll often see these two terms used interchangeably, as in most cases a column refers to data from a single field.

### Keys

A primary key (also known as an entity key) is a field in a table that uniquely identifies each row. For example, imagine a car reservation app where you can book a car in advance. The ID of the reservation could be the reservation number, and no two reservations would share the same reservation number, allowing each reservation to be uniquely identified by its reservation number.

**Example**

_Reservations Table_

| Reservation ID | Name  | Age |
| -------------- | ----- | --- |
| 11             | John  | 25  |
| 12             | Jenny | 31  |

In the above table, the `Reservation ID` field is the ID (primary key).

### Relationships between tables

Tables can contain references to other tables, which establishes a relationship between them.

Let’s expand the database of our hypothetical car booking app. We could have two tables: one for reservations (let’s call it **Reservations**) and one for customers, (we’ll call this one **Customers**).

To connect reservation data to the corresponding customer data, we’ll use a foreign key. A foreign key is a special kind of field in a table that references a column with matching values in a different table. Almost always, the field that the foreign key points to is the _ID_ or _primary key_ in the other table.

In this case, we’ll connect each record in the **Reservations** table to the record of the corresponding customer who made that reservation. To do this, we’ll create a `Customer` column in the **Reservations** table whose values match those of the `ID` column in the **Customers** table.

**Reservations**

| Customer | Date       | Car          |
| -------- | ---------- | ------------ |
| 11       | 12/20/2015 | Toyota Camry |
| 12       | 1/2/2016   | Range Rover  |

**Customers**

| ID | Name  | Age |
| -- | ----- | --- |
| 11 | John  | 25  |
| 12 | Jenny | 31  |

While the built-in relationship between primary and foreign keys make things easier for database users, most databases will allow you to make a join between two tables even if there is no key relationship.

Now if we wanted to analyze our hypothetical app’s database with Answers, we could ask a question, like: _What’s the average age of all customers who made reservations in December of 2015?_

To do this, we’d open up the Reservation table, add a filter to only look at reservations between December 1 and December 31, 2015, and select `Average of…`. To select the average of Age specifically, we’d put our foreign key to use and select Age from the _Customers_ table that our Reservations table references.
