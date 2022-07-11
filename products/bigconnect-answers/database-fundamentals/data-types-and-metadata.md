# Data types and metadata

Data types tell your database what kind of data it should expect in each column. Think of data types as a field’s classification—each field can only have one data type, and that data type might be a kind of number, text, boolean, or timestamp.

Different databases support different sets of data types — this guide covers some of the most common.

### Examples of data types <a href="#examples-of-data-types" id="examples-of-data-types"></a>

* **String Types** (TEXT, CHAR, VCHAR, etc.) - In the world of technology, snippets of text are referred to as “strings.” (You’ve probably heard of a “string of text” before.) Strings can contain numbers and special characters in addition to letters. These fields store things like names, addresses, or anything else that is text.
* **Numerical Types** (Integer, Float, DoubleFloat, Decimal, etc.) - These fields store numbers. Integers are whole numbers; floats and decimals are ways to store numbers with decimals in them. Numerical types store things like ages, bank account balances, costs, latitudes, and longitudes.
* **Temporal Types** (Timestamp, Date, Time etc.) - These fields are a special format used to store dates and times (or both), called “timestamps.” Sometimes timestamps are stored in an integer, called an Epoch UNIX Timestamp.
* **Boolean Types** - A value in these fields can be one of two options, usually `TRUE` or `FALSE`. Not all databases support boolean types.

A field may return `null` if it lacks a value entirely. `Null` doesn’t mean that a value is zero, rather that it is unknown and not listed.

In Answers, you can view the data type of a field by navigating to the Data Browser, selecting the gray **book icon** next to a table to access the Data Reference page, and clicking on **Fields in this table** in the left sidebar. Data types for each field are listed in the third column.

![Fig. 1. Viewing data types in Answers.](https://www.metabase.com/learn/images/data-types-overview/data-reference-page.png)

#### A note about IDs

Your database most likely has one or more ID fields that act as primary or foreign keys linking tables to each other. While these fields are important, “ID” itself is not a data type.

For example, your `PRODUCT_ID` field may be an integer or a string, as it could be made up of numbers or a combination of numbers and letters.

### Metadata

As the name suggests, metadata is data that describes other data. In other words, it’s information that tells you about the data found in your database. For example, we could label a column that looks like just a bunch of numbers with the label “latitude,” which would give that column additional meaning and context.

In Answers, administrators can edit field display names, descriptions, and semantic types (also known as field types) to give their users additional context about the purpose of each field and indicate to Answers how different fields should be intepreted.

#### Semantic types

While data types tell your database what kind of values to expect in a field, **semantic types** indicate the _meaning_ of a field. You may have several fields in your database with the data type `type/text`, but not all text fields have the same meaning or purpose. Semantic types are essential to building relationships between tables.

In Answers, semantic types are known as field types, and play an important role in telling Answers how to interpret each column. Correctly categorizing your field types makes it possible for Answers to determine what chart type to show you, create maps based on location information, or display URLs as links.
