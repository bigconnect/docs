# Object

### Extend

The "extend" helper is used to extend the context with the properties of other objects. It takes one argument, which is an array of objects to be merged into the context. The objects are shallow merged with the context, meaning that the properties of the objects are added to the context without mutating the original context.

Here's an example usage of the "extend" helper:

handleBars

```Text
{{extend [obj1, obj2, obj3]}}
```

In this example, obj1, obj2, and obj3 are objects containing properties that you want to add to the context. The "extend" helper will merge these objects into the context, making the properties of the objects accessible in your templates.

It's important to note that the merge is shallow, which means that if there are properties with the same name in both the context and the objects being merged, the properties from the objects will overwrite the properties in the context.

The "extend" helper is useful when you need to combine multiple objects and make their properties available in your Handlebars templates without modifying the original context.

### Forin

The "forIn" helper is a block helper that allows you to iterate over the properties of an object in Handlebars. It takes two arguments: the "context" and "options".

Here's the syntax for using the "forIn" helper:

handleBars

```Text
{{#forIn user}}
  {{!-- Content to render for each property --}}
{{/forIn}}
```

In this example, assuming the "user" object has properties like "name", "age", and "email", the "forIn" helper will iterate over each property and render the key-value pairs. The output would be something like:

Object Breakdown

```Text
Key: name, Value: John
Key: age, Value: 30
Key: email, Value: john@example.com
```

The "forIn" helper is particularly useful when you need to dynamically iterate over the properties of an object in your Handlebars templates.

### Forown

The "forOwn" helper is a block helper in Handlebars that allows you to iterate over the own properties of an object. It takes two arguments: "obj" and "options".

Here's the syntax for using the "forOwn" helper:

handleBars

```Text
{{#forOwn obj}}
  {{!-- Content to render for each property --}}
{{/forOwn}}
```

The "obj" argument is the object whose own properties you want to iterate over. Inside the block, you can access the current key and value of each property using the @key and @value variables, respectively. These variables are dynamically updated with each iteration.

Here's an example usage of the "forOwn" helper:

handleBars

```Text
{{ #forIn New Data Provider.Rows }}
  {{  name  }}
  {{  age   }}
  {{ email  }}
{{/forIn}}
```

See the previous object schema in "forIn" to see what is being used for the above example.

The "forOwn" helper is useful when you specifically want to iterate over the own properties of an object, excluding any inherited properties.

### Topath

The "toPath" helper in Handlebars is used to convert a string or number argument into a dot-delimited object property path. It takes a single argument, "prop".

Here's the syntax for using the "toPath" helper:

handleBars

```Text
{{toPath prop}}
```

The "prop" argument can be a string or a number. If it is a string or a number, the "toPath" helper will convert it into a dot-delimited property path.

Here's an example usage of the "toPath" helper:

handleBars

```Text
{{ toPath  New Data Provider.Rows.[0].email }}
```

In this example, the "toPath" helper converts the string "user.name" into the property path "user.name".

The "toPath" helper is useful when you need to dynamically construct property paths based on string or number arguments. It allows you to transform a simple value into a more complex path format that can be used to access nested properties on an object.

Note that the "toPath" helper does not actually access or retrieve values from an object. It simply converts the argument into a property path format. To access the value of the property path, you would need to use the dot notation or other appropriate helpers or expressions.

### Get

The "get" helper in Handlebars is used to retrieve a value or nested value from the context using property paths. It takes three arguments: "prop", "context", and "options".

Here's the syntax for using the "get" helper as a regular helper:

handlebars

```Text
{{get fieldName New Data Provider.Rows.[0] }}
```

The "get" helper is useful when you need to access nested values in your context using property paths. It provides a convenient way to retrieve values deep within nested objects without having to write multiple helper invocations or complex JavaScript expressions.

Note that if any part of the property path is not found in the context, the "get" helper will return undefined.

### Getobject

The "getObject" helper in Handlebars is used to retrieve an object from the context using property paths. It takes two arguments: "prop" and "context".

Here's the syntax for using the "getObject" helper:

handleBars

```Text
{{getObject prop context}}
```

The "prop" argument is the property path from which you want to retrieve the object. It can be a dot-delimited string representing nested properties (e.g., "user.name") or an array of property names (e.g., \["user", "name"]).

The "context" argument is the object or context from which the object will be retrieved. It is the starting point for resolving the property path.

Here's an example usage of the "getObject" helper:

handleBars

```Text
{{getObject fieldName New Data Provider.Rows.[0] }}
```

In this example, the "getObject" helper retrieves the object at the property path "user" from the "context" object.

The "getObject" helper differs from the "get" helper in two ways:

It returns the actual object, including the given property key. For example, if you use the "getObject" helper to retrieve the object at the property path "user" from the context, it will return the object with the key "user" intact, rather than just the value of the "user" property.

It does not work as a block helper. Unlike the "get" helper, the "getObject" helper cannot be used as a block helper to render content based on the retrieved object.

The "getObject" helper is useful when you need to retrieve an object from the context and retain the original property key. It can be used to access nested objects within the context using property paths and retrieve the entire object for further manipulation or processing.

Note that if any part of the property path is not found in the context, the "getObject" helper will return undefined.

### Hasown

The "hasOwn" helper in Handlebars is used to check if a given key is an own, enumerable property of the specified context object. It takes two arguments: "key" and "context".

Here's the syntax for using the "hasOwn" helper:

handleBars

```Text
{{ hasOwn New Data Provider.Rows.[0] "email" }}
```

The "context" argument is the object or context in which you want to check for the property.

The "key" argument is the property key that you want to check for in the context object.

### Isobject

The "isObject" helper in Handlebars is used to check if a given value is an object. It takes one argument: "value".

Here's the syntax for using the "isObject" helper:

handleBars

```Text
{{isObject objectField}}
```

The "value" argument is the value that you want to check if it is an object.

Here's an example usage of the "isObject" helper:

handlebars

```Text
{{#if (isObject objectField)}}
  The user is an object.
{{else}}
  The user is not an object.
{{/if}}
```

In this example, the "isObject" helper is used within an `if` block to check if the "user" variable is an object. If the value is an object, the first block of the `if` statement is rendered; otherwise, the `else` block is rendered.

The "isObject" helper is useful when you need to determine if a value is of type "object". It can be used to conditionally render content based on the type of the value.

Please note that the "isObject" helper checks if the value is of type "object" in the JavaScript sense, which includes plain objects, arrays, and other objects. It does not consider other types such as strings, numbers, booleans, etc. as objects.

### Jsonparse

The "JSONparse" helper in Handlebars is used to parse a JSON string into a JavaScript object. It takes one argument: "string".

Here's the syntax for using the "JSONparse" helper:

handleBars

```Text
{{JSONparse stringObject}}
```

The "string" argument is the JSON string that you want to parse into an object.

The "JSONparse" helper is useful when you need to convert a JSON string into a JavaScript object within your Handlebars template.

Please note that the "JSONparse" helper relies on the JavaScript built-in function JSON.parse to parse the JSON string. It is important to ensure that the JSON string is valid, otherwise an error may occur during parsing.

### Jsonstringify

The "JSONstringify" helper in Handlebars is used to convert a JavaScript object into a JSON string. It takes one argument: "obj".

Here's the syntax for using the "JSONstringify" helper:

handleBars

```Text
{{JSONstringify obj}}
```

The "obj" argument is the JavaScript object that you want to convert into a JSON string.

Here's an example usage of the "JSONstringify" helper:

handleBars

```Text
{{JSONstringify New Data Provider.Rows}}
```

In this example, the "JSONstringify" helper is used to convert the "user" object into a JSON string, which is then rendered within a paragraph element.

The "JSONstringify" helper is useful when you need to convert a JavaScript object into a JSON string within your Handlebars template.

Please note that the "JSONstringify" helper relies on the JavaScript built-in function JSON.stringify to perform the conversion. It is important to ensure that the object you pass as an argument is serializable to JSON, as certain types (such as functions or circular references) cannot be properly converted to a JSON string.

### Merge

The "merge" helper in Handlebars is used to deeply merge the properties of multiple objects with the context object. It takes two arguments: "object" and "objects".

Here's the syntax for using the "merge" helper:

handleBars

```Text
{{merge object objects}}
```

The "object" argument represents the context object, which is the target object that will be merged with the other objects.

The "objects" argument is an array of objects that contain the properties to be merged into the context object.

Please note that the "merge" helper performs a deep merge, meaning that nested objects within the provided objects will also be merged recursively. It ensures that the original context object is not mutated during the merge operation.

### Parsejson

The "parseJSON" helper in Handlebars is used to parse a JSON string using the JavaScript function "JSON.parse". It takes one argument: "string".

Here's the syntax for using the "parseJSON" helper:

handleBars

```Text
{{parseJSON string}}
```

The "string" argument represents the JSON string that you want to parse into a JavaScript object.

The "parseJSON" helper is useful when you need to convert a JSON string into a JavaScript object within your Handlebars template. It allows you to access and manipulate the data contained in the JSON string.

### Pick

The "pick" helper in Handlebars is used to extract specific properties from an object. It takes three arguments: "properties", "context", and "options".

Here's the syntax for using the "pick" helper:

handleBars

```Text
{{pick properties context options}}
```

The "properties" argument represents the properties you want to extract from the "context" object. It can be a single property name or an array of property names.

The "context" argument is the object from which you want to pick the properties.

The "options" argument is an optional parameter that allows you to specify additional configuration options for the helper.

Here's an example usage of the "pick" helper:

handleBars

```Text
{{pick 'name'  New Data Provider.Rows.[0] }}
```

In this example, the "pick" helper is used to extract the "name" property from the "person" object. The result will be the value of the "name" property.

You can also specify multiple properties to pick by passing an array of property names:

handleBars

```Text
{{pick ['name', 'age']  New Data Provider.Rows.[0] }}
```

In this case, the "pick" helper will extract both the "name" and "age" properties from the "person" object.

The "pick" helper is useful when you only need to work with a subset of properties from an object and want to avoid cluttering your template with unnecessary data.

Note that the "pick" helper does not modify the original object. It returns a new object containing only the picked properties.

### Stringify

The "stringify" helper in Handlebars is used to convert an object into a JSON string representation using the JSON.stringify function. It takes a single argument, which is the object to be serialized.

Here's the syntax for using the "stringify" helper:

handleBars

```Text
{{stringify obj}}
```

The "obj" argument represents the object that you want to convert into a JSON string.

The "stringify" helper is particularly useful when you want to pass complex data structures as a string to other parts of your application, such as storing it in a database or sending it over a network.

Note that the "stringify" helper relies on the JSON.stringify function provided by JavaScript, so it follows the same rules and limitations. Circular references and certain types of values, such as functions and undefined, may not be preserved during the serialization process.

### uuid

The uuid function generates a universally unique identifier (UUID) using the V4 method. This function is useful for creating unique keys or identifiers in applications.

**Example**

handleBars

```Text
{{ uuid }}
```

**Output**

```
f34ebc66-93bd-4f7c-b79b-92b5569138bc
```

In this example, the function generates a random UUID each time it is called.
