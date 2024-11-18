# Using Handlebars

Mware Application Development Frameworkis focused on building data-centric applications - to be able to do this you need a way to manipulate data. To do this it has the ability to bind data into any design or automation element through the use of Handlebars syntax or JavaScript

There are a number of places that bindings can be utilized throughout the Builder such as:

1. External data source queries - when building a query it is possible to utilize Handlebars syntax to specify where parameters should be inserted, e.g. `SELECT * FROM users WHERE username = {{ value }}`
2. The value of components in design - the values of most components in the design can be populated with bindings; anywhere that a value in the components setting has a lightning bolt next to it a binding can be inserted. Clicking the binding button (the bolt) will open the binding drawer.
3. Automation steps - automations have the ability to make use of data from previous steps, this can be found in the binding panel; opened in the same manner as with the design bindings.\
   When making use of bindings it is possible to manipulate the data using the Handlebars helpers, these allow performing calculations, carrying out some basic logical comparisons, and some array/string functions. The full list of options available is as follows:

* Arrays - the ability to sort, slice, get an item at a specific index, etc.
* Comparison - simple logic such as greater than, equal to, less than, contains, etc.
* Dates - the ability to format dates as you desire, using syntax like `"DD-MM-YYYY"` - there is only one helper for this and it can be used as such: `{{ date tableName.dateProperty "DD-MM-YYYY" }}` you can also use now instead of a table/data source data property if desired.
* Math - math functions like adding, subtracting, averaging, etc.
* Number - these helpers allow formatting numbers, for example setting precision, setting to an exponential, adding commas, etc.
* String - the ability to manipulate strings, such as splitting them (useful with the array helpers), appending, capitalization, etc.
* URL - the ability to encode/decode URI and escape URLs.

For more information, the full catalogue of helpers is available [here](https://github.com/helpers/handlebars-helpers#helpers)

> ### 🚧Handlebars helpers and queries
>
> Handlebars helpers can be used in automations, the design, and data table formula columns, but they cannot be used as part of data source queries - as part of a query you can only use singular Handlebars statements like `{{ value }}`.

Below is a video showing some of the capabilities of bindings in the design panel - showing how to add a repeater to make use of table data and then displaying some date values as well as carrying out some complex string manipulation.

### Tips and tricks

It is important to remember that Handlebars does allow quite complex statements, which can make use of comparative logic (especially with our comparison helpers). Below we've detailed a few tips which should help you get started with bindings and Handlebars syntax.

#### Chaining helpers

Handlebars allows chaining helpers together to create quite complex statements - for example, if I had the string "Username: Michael" and I wanted to just get the name I could do the following:

Handlebars

```html
{{ last ( split "Username: Joe" ": " ) }}
```

In the example we split the string by the colon and space in the middle, then get the last element of the array produced by the split helper. Chaining helpers simply requires wrapping each layer of helpers with parentheses, although it is important to remember parameter order, for example, if we had the string "My name is Joe Bloggs" and we wanted to get the string "Joe Bloggs" we would do the following:

Handlebars

```html
{{ join ( after ( split "My name is Joe Bloggs" " " ) 3 ) " " }}
```

Take note with this statement of where the "join" and "after" last parameters are positioned, here we are splitting the string into an array by the spaces, then selecting the elements in the array after index 3, then finally re-joining the string with a space again.

#### Iteration

A powerful feature that can be utilized with Handlebars is the ability to iterate over an object or array. This can be extremely useful when paired up with data sources, for example, if you had a DynamoDB table that contains JSON you might want to do the following:

Handlebars

```html
{{#forIn Query 1.object.json}}
  property: {{@key}} - value: {{.}}
{{/forIn}}
```

In the example above it is assumed that the `Query 1.array` property is an array consisting of objects that contain `username` property.

#### Conditionals and comparisons

Handlebars is capable of carrying out logic when running a block, this is a core component that can be used to produce more conditional outputs. An example of this might be pulling information from an external data source, which has information about a customer's total spending, we might do something like:

Handlebars

```html
{{#if ( gte Query 1.Customer.spending 1000 )}}
  Thanks {{Query 1.Customer.name}}! We value your patronage.
{{else}}
  Hi {{Query 1.Customer.name}}!
{{/if}}
```

As you can see from this example we can wrap a statement in an #if block which allows us to produce a specific output when a condition is met. We have also made use of the gte helper which checks if the first property is greater than or equal to the second property. Chaining together a few different helpers and then finishing with comparison can allow us to perform some very specific logic. We can also make use of regex to perform conditional logic, as such:

Handlebars

```html
{{#if ( test "hello there" (toRegex "there"))}}
  Over there!
{{/if}}
```

We can use the `test` helper to run a regex expression against a string, this regex can come from the `toRegex` helper or it can be directly input as a string; or even a property of a table, query or automation step!

#### JavaScript Bindings

When using JavaScript instead of the handlebars, the syntax will be a little different, using dollar sign parenthesis. For example:

JavaScript

```javascript
return $("Query 1.Customer.name")
```

To read more about JavaScript bindings, head over to the Using JavaScript guide.
