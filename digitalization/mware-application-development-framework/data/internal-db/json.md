# JSON

The JSON data type is available for internal tables. JSON fields have a schema, which can be configured when creating or editing the column. Having the correct schema is important as the builder and apps use this to know what bindings are available and what form fields are available. Extra data that is not in the schema can be stored, but no bindings will exist for it.

The JSON field schema can be edited with either a form or by providing an example JSON payload. If you are using deep JSON structures (e.g. multiple levels of objects or arrays) then you will need to use the JSON schema editor, as the form editor only goes one level deep.

Schema editor button when creating or editing a JSON column:

<figure><img src="https://files.readme.io/1ef75ae-145810969-46b3262f-03b1-4750-b490-fb51ba48538a_1.png" alt=""><figcaption></figcaption></figure>

### Editing JSON (Form)

Using the JSON form editor, you can update your JSON without having to write JSON.

<figure><img src="https://files.readme.io/c1fee5f-145811398-fccb00cf-ae4d-4758-b777-19ea98f93e7d_1.png" alt=""><figcaption></figcaption></figure>

### Editing JSON (raw)

Using the JSON schema editor, you can write and edit raw JSON.

<figure><img src="https://files.readme.io/d019a49-145811736-9ad06ea6-15c7-4a06-b3a3-936927e09720_1.png" alt=""><figcaption></figcaption></figure>

### Displaying data within a JSON column

JSON field schema is merged with table schema in client apps, and basically treated as new top level columns. This means you can filter on nested values, or display them inside tables, as if they were normal fields in your table. Here's an example of a table showing all columns inside the table containing the JSON field above:

<figure><img src="https://files.readme.io/27347e2-145813246-1ed77cd1-4613-4ee8-8f85-a1b1d8a39da5_1.png" alt=""><figcaption></figcaption></figure>

### Displaying JSON arrays

In that screenshot, you can see that it has pulled out fields from within the JSON field and displayed them automatically. It also is showing the JSON fields as a whole (as it shows all columns by default) - but you can easily control what gets displayed by using the table setting:

<figure><img src="https://files.readme.io/e4d3026-145813415-de271d75-99e2-47f5-964c-d71e33476225_1.png" alt=""><figcaption></figcaption></figure>

***

### Filtering

You can filter using fields inside your JSON. Here's an example of filtering using the nested value Car.Make:

<figure><img src="https://files.readme.io/665a9f1-145813879-c03d502f-29e7-4a38-84f6-da03bc950d85_1.png" alt=""><figcaption></figcaption></figure>

***

### Data bindings

Data bindings will be automatically generated for all available fields inside your JSON, including nested fields (e.g. _Car.Make_). Data bindings will be generated until an array is hit, at which point no further bindings can be generated. You can read about how arrays are handled down below. Bindings are also provided for the JSON field as a whole and any array fields, both of which integrate very nicely with JS bindings.

Here's an example of all the data bindings provided for the JSON field above. My table is called **People** and my JSON column is called **Data**.

<figure><img src="https://files.readme.io/2b3ab0c-145812656-2bd83f85-524e-46bc-a83c-5dcf9d321428_1.png" alt=""><figcaption></figcaption></figure>

***

### Arrays

Arrays inside JSON fields can be used as data sources. For this example, I've added a new property called **Friends** inside my JSON field, which is an array.

JSON

```json
"Friends": [
    {
      "Name": "Bill",
      "Address": {
        "Number": 1,
        "Street": "High Street",
        "City": "New York"
      }
    },
    {
      "Name": "Ben",
      "Address": {
        "Number": 18,
        "Street": "Shore Road",
        "City": "Carrickfergus"
      }
    },
    {
      "Name": "Bert",
      "Address": {
        "Number": 249,
        "Street": "Mountain Cresent",
        "City": "Lima"
      }
    }
  ]
```

Here's an example using a table block to display data of _Friends_. The structure is:

* a Data Provider with Repeater (to get the rows that contain the JSON fields),
* then a table block (to show the data inside the array inside the JSON field).

<figure><img src="https://files.readme.io/32eed66-Screenshot_2022-07-06_at_12.51.16.png" alt=""><figcaption></figcaption></figure>

You can see from this screenshot that the schema correctly determines all available fields inside the array objects and is listing them in the table. If you have an array of primitives (e.g. an array of numbers, or strings) then a fake schema entry **value** is generated. You can then display data inside primitive arrays using _Field.something.value_.

You can nest arrays as deep as you like. If your JSON structure looked something like this:

JSON

```json
{
  "countries": [{
    "name": "England",
    "cities": [{
      "name": "London",
      "boroughs": ["Barnet", "Brent", "Camden", "Croydon", "Harrow", "Sutton"]
    }]
  }]
}
```

Then you can use a repeater on _Column.countries_, then put a repeater block inside that and target _countries.cities_, then put another repeater block inside that and target _cities.boroughs_, to finally get out the borough values. You can chain this as long as you like and the schema will always be available.

***

### JSON form field

There is a new form field component for JSON fields. It will render a text area that pretty prints the content as JSON. It also has forced validation to ensure that the input is valid JSON.

The new JSON field component:

<figure><img src="https://files.readme.io/1d34354-145822205-4eaf0f40-4f3f-455b-85c4-e2aa1d07373f_1.png" alt=""><figcaption></figcaption></figure>

Validation enforcing JSON syntax:

<figure><img src="https://files.readme.io/498d8bc-145822308-9acafdf1-5af8-4165-af0e-f5b50fe1f1ba_1.png" alt=""><figcaption></figcaption></figure>

***

### JSON form integration

Since nested JSON fields are considered normal fields, you can bind a form component to a nested JSON field. In my example, I have the field _Car.Make_. If I want to update this, I can simply add a text field and bind it to _Car.Make_, then save my whole row as normal. This will transparently update the value inside the JSON field with no extra configuration.

Binding a form component to a nested JSON field:

![](https://files.readme.io/d539e3e-145822912-6a5b6fce-8004-4e8e-85a1-67e7087c3bd9\_1.png)

When generating form components automatically (via the `Update form fields` button on [Field groups](https://docs.budibase.com/docs/field-groups)) the builder will always insert an actual JSON field form component for the whole JSON field, rather than individual inputs for every nested property. You can always add/remove fields as you see fit if you want to be able to directly update some nested fields.

If you have a form targeting a subsection of your JSON field, then the best way to save your full row value is to write a small [JavaScript Binding](https://docs.budibase.com/docs/javascript) and make use of the new _Form.Value_ binding (which is an object of the whole value of the form) to insert the value into your JSON field. This is only really needed when you are iterating over JSON arrays and want to update one of the array elements.

#### Example: Questionnaires

1. Add a **Topic** table with a _Name_ column and a JSON type _Questionnaires_ column with the following schema:

<figure><img src="https://files.readme.io/127d302-Screenshot_2023-02-06_at_11.11.53.png" alt=""><figcaption></figcaption></figure>

2. After clicking `Save Column`, insert a row for each of your topics.

<figure><img src="https://files.readme.io/8e09b8f-Screenshot_2023-02-06_at_11.14.42.png" alt=""><figcaption></figcaption></figure>

3. Create a [Screen](https://docs.budibase.com/docs/screens) for the `Topic` table. Insert a Cards block. Assign the _Title_ property to the topic name binding (`{{ New Cards block.Topic.Name }}`, and optionally set the _Subtitle_ to display the number of questionnaires: `Questionnaires: {{ length New Cards block.Topic.Questionnaires.values }}`
4. Tick the `Link card title` setting, and provide the following URL: `/new-questionnaire/:topicId`

<figure><img src="https://files.readme.io/b8f0832-Screenshot_2023-02-06_at_11.23.19.png" alt=""><figcaption></figcaption></figure>

5. Create another screen, and set the route to the link destination:

<figure><img src="https://files.readme.io/058ab59-Screenshot_2023-02-06_at_11.26.55.png" alt=""><figcaption></figcaption></figure>

6. Add a [Repeater block](https://docs.budibase.com/docs/blocks#repeater-block) with a filter on the _topicId_

<figure><img src="https://files.readme.io/341fdf6-Screenshot_2023-02-06_at_11.30.02.png" alt=""><figcaption></figcaption></figure>

7. Add a Headline for the topic name, with the following binding: `Topic: {{ Topics Repeater block.Topic.Name }}`.
8. Nest a Form within the repeater block. Select the _Questionnaires_ column as the schema.

<figure><img src="https://files.readme.io/c7f15d2-Screenshot_2023-02-06_at_11.34.01.png" alt=""><figcaption></figcaption></figure>

9. Now you'll be able to add the form fields as defined by the JSON schema earlier.

<figure><img src="https://files.readme.io/9d4c897-Screenshot_2023-02-06_at_11.40.49.png" alt=""><figcaption></figcaption></figure>

10. Finally add a submit Button. `Define actions` and select the **Save Row** action. Choose the repeater block as the _Datasource_, with _Topic_ as the target table. Click `Add Column` so that the JSON _Questionnaires_ array can be populated correctly.

<figure><img src="https://files.readme.io/ff5595d-Screenshot_2023-02-06_at_11.43.43.png" alt=""><figcaption></figcaption></figure>

Questionnaires.values

```javascript
return [...($("Topics Repeater block.Topic.Questionnaires.values") || []), {
	title: $("Questionnaire Form.Fields.title"),
	rating: $("Questionnaire Form.Fields.rating"),
	comments: $("Questionnaire Form.Fields.comments")
}];
```

**Result**

| Q1 | Q2                                                                            |
| -- | ----------------------------------------------------------------------------- |
|    | ![](https://files.readme.io/adfba1f-Screenshot\_2023-02-06\_at\_11.47.32.png) |

<figure><img src="https://files.readme.io/7f2b69c-Screenshot_2023-02-06_at_11.45.49.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/9b029a8-Screenshot_2023-02-06_at_11.50.25.png" alt=""><figcaption></figcaption></figure>
