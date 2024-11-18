# Date

### Date

The "date" helper in Handlebars is used to format a date using the Moment.js library. It takes two required arguments and an optional third argument for the timezone.

Here's the syntax for using the "date" helper:

handleBars

```Text
{{date datetime format timezone}}
```

The "datetime" argument represents the date and time value that you want to format. It can be a JavaScript Date object, a string that can be parsed into a Date object, or a number representing the number of milliseconds since the Unix epoch.

The "format" argument specifies the desired format for the date output. It follows the formatting tokens defined by Moment.js. For example, "DD-MM-YYYY" represents the day, month, and year in the format "21-01-2021".

The "timezone" argument is optional and allows you to specify the timezone for the date. It uses the tz database (also known as the Olson database) to determine the timezone. If not provided, the date will be formatted in the local timezone of the user's environment.

Here's an example usage of the "date" helper:

handleBars

```Text
{{date now "DD-MM-YYYY" "America/New_York"}}
```

In this example, the "now" variable represents the current date and time. The date will be formatted using the "DD-MM-YYYY" format and converted to the "America/New\_York" timezone.

The "date" helper is particularly useful when you need to display formatted dates in your templates, such as showing the creation date of a post or the last modified date of a document.

Note that the "date" helper relies on the Moment.js library, so you need to include the Moment.js library in your project for the helper to work.

### duration

The "duration" helper in Handlebars is used to produce a humanized representation of a duration or time interval. It takes two required arguments.

Here's the syntax for using the "duration" helper:

handleBars

```Text
{{duration time durationType}}
```

The "time" argument represents the duration or time interval that you want to humanize. It can be a number representing a duration in milliseconds, seconds, minutes, hours, days, weeks, months, or years.

The "durationType" argument specifies the type of time measurement used for the duration. It can be one of the following values: "milliseconds", "seconds", "minutes", "hours", "days", "weeks", "months", or "years".

Here's an example usage of the "duration" helper:

handleBars

```Text
{{duration timeLeft "seconds"}}
```

In this example, the "timeLeft" variable represents the amount of time left until a certain event. The duration will be humanized using seconds as the measurement. The output might be something like "a few seconds", "1 minute", "2 hours", etc.

The "duration" helper is useful when you need to display a user-friendly representation of a duration or time interval in your templates. It can be handy for displaying countdowns, time remaining, or elapsed time.
