# Languages and localization

The **Localization** settings allow you to set global defaults for your Answers instance. You can find **Localization** under **Admin settings** > **Settings**.

## Default language

Here you can set the default language (also called the “instance language”) across your Answers UI, system emails, dashboard subscriptions, and alerts. People can pick a different language from their own account settings.

## Supported languages

Thanks to our amazing user community, Answers has been translated into many different languages. Due to the way we collect translations, languages may be added or removed during major releases depending on translation coverage.

The languages you can currently pick from are:

* English (default)
* Albanian
* Arabic
* Bulgarian
* Catalan
* Chinese (simplified)
* Chinese (traditional)
* Czech
* Dutch
* Farsi/Persian
* Finnish
* French
* German
* Indonesian
* Italian
* Japanese
* Korean
* Latvian
* Norwegian Bokmål
* Polish
* Portuguese
* Russian
* Romanian
* Serbian
* Slovak
* Spanish
* Swedish
* Turkish
* Ukrainian
* Vietnamese

> While Answers can support languages that read right to left, the Answers UI is designed around languages that read left to right.

## Report timezone

Use **report timezone** to set a default display time zone for dates and times in Answers. The report timezone setting is a display setting only, so changing the report timezone won’t affect the time zone of any data in your database.

Report timezone doesn’t apply to `timestamp without time zone` data types, including the output of `convertTimezone` expressions. For example:

| Raw timestamp in your database           | Data type                     | Report time zone | Displayed as           |
| ---------------------------------------- | ----------------------------- | ---------------- | ---------------------- |
| `2022-12-28T12:00:00 AT TIME ZONE 'CST'` | `timestamp with time zone`    | ‘Canada/Eastern’ | Dec 28, 2022, 7:00 AM  |
| `2022-12-28T12:00:00-06:00`              | `timestamp with offset`       | ‘Canada/Eastern’ | Dec 28, 2022, 7:00 AM  |
| `2022-12-28T12:00:00`                    | `timestamp without time zone` | ‘Canada/Eastern’ | Dec 28, 2022, 12:00 AM |

Report timezone is only supported for the following databases:

* BigQuery
* Druid
* MySQL
* Oracle
* PostgreSQL
* Presto
* Redshift
* Vertica

## First day of the week

If you need to, you can change the first day of the week for your instance (the default is Sunday). Setting the first day of the week affects things like grouping by week and filtering in questions built using the query builder. This setting doesn’t affect SQL queries.

## Localization options

**Localization options** allow you to set global default display formats for dates, times, numbers, and currencies.

You can also override these localization options for specific fields or questions. For more info, see Formatting.

## Dates and times

* **Date style:** the way dates should be displayed in tables, axis labels, and tooltips.
* **Date separators:** you can choose between slashes (`2022/12/14`), dashes (`2022-12-14`), and dots (`2022.12.14.`).
* **Abbreviate names of days and months:** whenever a date is displayed with the day of the week and/or the month written out, turning this setting on will display e.g. “January” as “Jan” or “Monday” as “Mon”.
* **Time style:** choose to display the time using either a 12 or 24-hour clock (e.g., 3:00 PM or 15:00).

## Numbers

* **Separator style:** some people use commas to separate thousands places, and others use periods. Here’s where you can indicate which camp you belong to.

## Currency

* **Unit of currency:** if you do most of your business in a particular currency, you can specify that here.
* **Currency label style:** whether you want to have your currencies labeled with a symbol, a code (like “USD”), or its full name.
* **Where to display the unit of currency:** this pertains specifically to tables, and lets you choose if you want the currency labels to appear only in the column heading, or next to each value in the column.
