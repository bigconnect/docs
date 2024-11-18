# Transformers

Using Mware Application Development Frameworkquery transformers

In this section, we will cover how to use data transformers, which can be used as part of data source queries. It is often a requirement when retrieving data from various sources to transform to fit your app's use case - from simply extracting properties from deeper JSON objects to enriching your data with more information transformers can be used for a wide variety of applications.

> ### 📘This section requires JavaScript knowledge
>
> We recommend the Modern [JavaScript Tutorial ](https://javascript.info/)to pick up the basics, for transformers you will specifically benefit from knowledge around data types, such as [Arrays](https://javascript.info/array), [Array Methods](https://javascript.info/array-methods) and [Objects](https://javascript.info/keys-values-entries).

To create a transformer first you need to create an external data source and a query, steps for this can be found in the External Data Sources section.

### Accessing bindings

It is possible to access the bindings that your query received when it was called, through the `params` object that is available in the transformer context. For example if I've added a binding of `lastName` then I can access this property in the transformer under `params.lastName`. You can see a full example of this below.

<figure><img src="https://files.readme.io/c553972c1729aee14507380baa987b213dc2d7e799811b3fcc04b1f7b0cbff4d-Screenshot_2024-09-20_at_15.05.01.png" alt=""><figcaption></figcaption></figure>

### Tutorial

In the example below we will be transforming some data from the [Open Brewery Database](https://www.openbrewerydb.org/) - we will be creating an app that has tallies of the number of breweries by US state.

When you first create a query you'll see the transformer code editor in its own section, as seen below.

<figure><img src="https://files.readme.io/d0712b5-transformers.png" alt=""><figcaption></figcaption></figure>

> ### 📘
>
> There are two properties that are accessible by default within the transformer. First, the data which as its name suggests contains the data retrieved by the query. Secondly, params, which contain the query bindings/parameters that were provided when it was called.

This will return the data exactly the way it is retrieved from the data source, it is recommended to get your query up and running correctly first before altering your transformer, this will allow you to look at the initial schema of the data returned. In our example so far we have:

1. Setup a REST data source, with our URL set to

```
https://api.openbrewerydb.org
```

2. Created a query and set the path to _breweries_
3. Ran the query to see the schema

The schema for this query appears as below:

<figure><img src="https://files.readme.io/1fc1cad6d7f93e9a330b40666609627dba3adc9396c56fa9d7f802e156cc8bb1-Screenshot_2024-09-24_at_11.12.29.png" alt=""><figcaption></figcaption></figure>

Using this information we can now write the transformer function that will be used. Initially we just want to write a function which will:

1. Work through the array of breweries that are returned by the API, in the format shown above
2. Extract the state and add it to a total count of states that have been seen
3. Return a new structure that contains only the state name and the count of breweries within

You can see the basic transformer we have written to do this below.

<figure><img src="https://files.readme.io/15ab6883c0a5c7d353c24068211646910acc404ab827092e5350a3dd35e7f59b-Screenshot_2024-09-24_at_12.56.43.png" alt=""><figcaption></figcaption></figure>

Here we have taken the data, written a for loop that iterates through a fills up an object with counts for each state (using the state's name as the key into the object), and finally, we've mapped these totals to our output structure, an object with a state and count property. You can see from this that we can drastically change the format of the data; using JavaScript you can change the data in a multitude of ways.

For the last part, we want to add data that simply isn't a part of the query, enriching the application logic that we've provided as part of the transformer. We are going to add a URL that points to an image of the states flag for each of the state entries, to do this we will create a URL dynamically to [http://flags.ox3.in/](http://flags.ox3.in/) - a repository of SVG flags. You can see the final function which does this below.

<figure><img src="https://files.readme.io/a0650eb543897b411fcb8fec2705cd8d4bd5f4c11af2b11f631f4dcd606ff0da-Screenshot_2024-09-24_at_13.07.27.png" alt=""><figcaption></figcaption></figure>

This may look a little complicated, but all we have done is added a map of the state names to state codes, then at the end when we produce the final data we lookup the state name in the map and build a URL that has the code in it. You can see in our results section we now have a "flag" property, which can be used in the design section with something like an image or card component. We've thrown together a quick example of how this can all come together in your design, using only a data provider, repeater and card component bound to the "state", "count" and "flag" properties our transformer produced.

![](https://files.readme.io/522188e-image\_7.png)

Hopefully, this has helped to demonstrate how transformers can be used to get the data you need for your application, happy coding!

### Response

A query must always return an array as that is how Mware Application Development Framework handles data, it will always attempt to fit your data into a column/row type table structure. If you have an endpoint which returns a single object then the following conversion with occur:

JSON

```json
{ 
  "a": 1,
  "b": 2,
}

// will convert this to:
[
  { "a": 1, "b": 2 } 
]
```

This functionality is important to understand when building transformers as if you return an array it will be broken up into individual rows. An example of this would be the use of the `Object.keys` or `Object.values` functions, breaking an object up into an array of keys/values. Below we can see an example of how Mware Application Development Framework would treat a transformer which returns the direct result of these functions:

JSON

```json
// outputs [ "a", "b" ]
return Object.keys({ "a": 1, "b": 2 })

// will break this array into the following response:
[
  { "value": "a" },
  { "value": "b" },
]
```

If we were to take the above example and add it to a table within Mware Application Development Framework it would appear as seen below.

![](https://files.readme.io/fadc6e2-image.png)

If you instead would like to return all of these elements as an array you can do this, by wrapping the transformer response in an object, like `return { letters: Object.keys({ "a": 1, "b": 2 }) }` - however the query will still return an array, so that it can be used by a data provider + table/repeater.
