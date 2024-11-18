# Date/Time

The date/time data type is important when using components or displaying information that relies on dates and times.

The Date picker component, for example, can utilize the date field. Dates within Mware Application Development Framework comply with ISO 8601 formatting, e.g. `2023-04-24T13:12:04.622Z`. Thankfully, the date picker makes data entry easier.

### Creating a Date/Time column

1. First, click the `+` button along the top-right
2. Give your column a name
3. Choose date/time as your data type
4. Enter some optional constraints
5. Click `Save Column`

<figure><img src="https://files.readme.io/1da53ba-Screenshot_2022-08-31_at_12.39.57.png" alt=""><figcaption></figcaption></figure>

### Constraints

You can provide an **earliest** and **latest** date within your date/time column. This allows you to enforce a constraint such that dates added must fall _before the latest_ and _after the earliest_ dates specified.

As with all other fields you can make it **required**.

These constraints can easily be edited after creation by hovering over the date field and clicking the three dots icon.

#### Ignore time zones

When this option is checked, the values selected in date pickers will be saved exactly as they are represented into the target database. Regardless of the timezone the browser is in, you will always see the same exact timestamp when this option is selected.

As an example, here is a snippet of a table where the browsers locale was changed to 3 different timezones and the time 12:00 was picked in each. You can see that the column "BB no timezone" (which has _Ignore time zones_ enabled) will always show the same time, whereas the "BB normal" column (where the flag is toggled off) reflects the browser timezone changes.

<figure><img src="https://files.readme.io/1efd0f2-Screenshot_2022-08-31_at_13.03.11.png" alt=""><figcaption></figcaption></figure>

It is possible, but not recommended, to change the _Ignore time zones_ flag on tables that already have dates with timezones. Changing this flag does not change existing data, but it will change how your existing data is presented in the browser.

By default this setting is toggled off.

> ### 📘
>
> This setting is also available for PostgreSQL and MySQL tables.

### Working with dates

As mentioned above, dates in Mware Application Development Framework are given as standard ISO 8601 date/time strings. You can make use of bindings to transform that date string into whatever you need to display.

#### Tutorial: Calculating the duration of an event

Say we have a table _Events_ with two date columns: _Start Time_ and _End Time_. To calculate the duration of each event, we can create a Formula column and then use Bindings to calculate the difference between the two dates.

It's not possible to perform mathematical operations on a date string, so the first step is to convert the date into milliseconds. The value is a count of how many milliseconds the date is before or after the UNIX epoch in 1970. We can do this using the **date helper** where bindings are available. The date helper takes the date value, the desired output format and an optional timezone as arguments.

To format _Start Time_ in milliseconds using the date helper, pass the Start Time value and 'x' as the date format. You can see the other available output formats in the Day.js format documentation, which is what the date helper uses behind the scenes. Our Handlebars expression is `{{ date Start Time 'x' }}` and the output is **1685100000000**.

The next step is to do the same for _End Time_ and then using the **subtract helper**, subtract the ms value for _Start Time_ from it. This will return the length of the event in milliseconds. Here's how it looks with the helpers chained up:

`{{ subtract (date End Time 'x' ) ( date Start Time 'x' ) }}`

Now we have the duration of the event in milliseconds, the final step is to convert this value into something more useful. We can use the **duration helper** which takes our time value and the unit to calculate a human readable duration. By default this helper uses the Day.js Humanize Duration function.

Here's the final binding and the result:

`{{ duration (subtract (date End Time 'x' ) ( date Start Time 'x' )) "milliseconds" }}`

<figure><img src="https://files.readme.io/0b7e610-image.png" alt=""><figcaption></figcaption></figure>

Of course, you can also use JavaScript bindings for more complex time calculations. All of the helpers that make use of Day.js are also available there.

#### Tutorial: Calculating the time until an event

The same principle can be used for calculating the time since a past date or the time until a future date. In this case you would only use one date value and compare it to the current time instead of another date. To get the current time in milliseconds, use `{{ date now 'x' }}`

In all cases, subtract the earliest date from the latest date. For working out how long ago past events were, I used this binding:

`{{ duration (subtract (date now 'x' )( date Start Time 'x' )) "milliseconds" }} ago`

<figure><img src="https://files.readme.io/c6ef418-image.png" alt=""><figcaption></figcaption></figure>
