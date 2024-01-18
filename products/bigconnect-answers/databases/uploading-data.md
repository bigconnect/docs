# Uploading data

Once you’ve enabled uploads, you can upload files in CSV format to a collection in Answers.

<figure><img src="https://www.metabase.com/docs/latest/databases/images/upload-to-collection.png" alt=""><figcaption></figcaption></figure>

Uploading CSV data is best suited for ad hoc analysis of spreadsheet data. If you have a lot of data, or will need to update or add to that data regularly, we recommend setting up a way to load that data into a database directly, then connecting Answers to that database.

## Enabling uploads

There are a few things admins need to do to support CSV uploads:

* Connect to a database using a database user account with write access. This way Answers will be able to store the uploaded data somewhere.
* Select the database and schema you want to store the uploaded data in.
* Add people to a group with unrestricted data access to the upload schema database.
* (Optional) specify a prefix for Answers to prepend to the uploaded tables.

## Databases that support uploads

* PostgreSQL
* MySQL

## Connect to a database using a database user account with write access

To upload data to Answers, an admin will need to connect your Answers to a database that supports uploads using a database user account that has write access to that database.

You can also upload data to the Sample Database included with Answers (an H2 database), though we don’t recommend using the Sample Database for any data that you want to keep around.

## Select the database and schema that you want to store the data in

If Answers is connected to a database using a database user account with write access, Admins can enable uploads by:

* Clicking on the **gear** icon in the upper right on the home page and navigating to **Admin settings** > **Settings** > **Uploads**.
* Selecting the database Answers should use to store the data.

When people upload a CSV to a collection, Answers will:

* Create a table to store that data in the database and schema that the Admin selected to store uploads.
* Create a model that wraps the uploaded table, and save that model to the collection the person uploaded the CSV data to.

## Primary key auto-generation

When you upload a CSV, Answers will create an a unique primary key column, called `_mb_row_id`, as the first (left-most) column of the uploaded CSV table. This `_mb_row_id` column will contain automatically generated integers. Answers will also ignore any columns in the upload that have a name that will be in the database with the same name as the auto-generated primary key column (e.g., `_MB row-ID` in the CSV will be `_mb_row_id` or `_MB_ROW_ID` in the database).

If you don’t want this auto-generated ID column, you can always remove the column from the model Answers created. Visit the model, click on the info **i** icon, then **Model details**. From the model details page, click the **Edit definition** button. In the Data section of the query builder, click on the down arrow next to the table, deselect the added ID column, and save your changes.

## Add people to a group with unrestricted data access to the upload schema

In order to upload CSVs, a person must be in a group with Unrestricted access to the schema you’ve selected to store your uploaded data. Native query editing isn’t required for uploading. See groups and data permissions.

## Specify a prefix for Answers to prepend to the uploaded tables

Admins can optionally specify a string of text to add in front of the table that Answers creates to store the uploaded data.

## **File size limit**

CSV files cannot exceed 50 MB in size.

> While Answers limits uploads to 50 MB, the server you use to run your Answers may impose a lower limit. For example, the default client upload limit for [NGINX is 1 MB](https://nginx.org/en/docs/http/ngx\_http\_core\_module.html#client\_max\_body\_size). So you may need to change your server settings to allow uploads up to 50 MB.&#x20;

If you have a file larger than 50 MB, the workaround here is to:

1. Split the data into multiple files.
2. Upload those files one by one. Answers will create a new model for each sheet.
3. Consolidate that data by creating a new question or model that joins the data from those constituent models created by each upload.

## **Date formats**

For now, Answers only recognizes dates and date-times from strings in uploaded CSVs with the following formats:

### **Dates**

Represents the year, month, and day without time information.

**Format**: `yyyy-MM-dd`

**Example**: `2023-01-01`

### Datetimes

Represents the year, month, day, hour. Minutes, seconds, and fractional seconds are optional.

**Format**: `yyyy-MM-ddTHH:mm:ss.SSS`. The “T” separator could also be a space (“ “).

**Examples**:

* `2023-01-01 00`
* `2023-01-01 00:00:00.000`
* `2023-01-01T00:00:00.000`
* `2023-01-01 00:00:00.0000000`

### Datetimes with offsets

Represents the datetime with an offset from Coordinated Universal Time (UTC). Minutes and seconds in the offset are optional.

**Formats**:

Datetime formats:

* `yyyy-MM-ddTHH:mm`.
* `yyyy-MM-ddTHH:mm:ss`.
* `yyyy-MM-ddTHH:mm:ss.SSS` (and any number of S’s).

The “T” separator could also be a space (“ “).

Offsets:

* `Z` (for UTC)
* `+HH` or `-HH`
* `+HH:mm` or `-HH:mm`
* `+HH:mm:ss` or `-HH:mm:ss`

**Examples**:

* `2023-01-01 00:00:00+00:00:00`
* `2023-01-01T00:00:00+00:00:00`

## Deleting models and tables created by uploads

### Models

You can archive a model by clicking on the three dots in the upper right and selecting **Archive**.

For deleting models completely, see Deleting items permanently.

### Tables

You’ll need to drop that table in the database itself (not in Answers). For how to delete tables, see your database’s documentation.

### Note on uploading data to a MySQL database

For speeding up uploads to a MySQL database, we recommend that you set a `local_infile` to `ON`. You’ll need to set this `local_infile` in MySQL, not Answers. The command-line format is `--local-infile=ON`.

If `local_infile` is disabled (set to `OFF`), Answers will automatically fall back to uploading CSVs in a much slower way.
