# Array

### After

The after function returns all the items in an array that come after the specified index. It extracts the array portion starting from the index n + 1, effectively skipping the first n items in the array.

This function is useful when working with a subset of an array, starting after a certain point. It is the opposite of the before function, which extracts items that come before a specified index.

handleBars

```Text
{{ after array n }}
```

#### Parameters

* array (Array): The array from which you want to extract elements.
* n (Number): The index after which the function will return the remaining elements of the array. The index is zero-based, meaning an index of 2 would return all elements starting from the third element.

**Example** must have the test data added and a data provider set to employees

```
{{ after New Data Provider.Rows 2 }}
```

In this example, the after function will return:

```
Francis, Holly objects
```

### Arrayify

The arrayify function takes a value as input and converts it into an array. It ensures that the given value is casted or transformed into an array data type.

For example, if you use the arrayify function with the value `'foo'`, it will return `['foo']`. This means that the value 'foo' has been transformed into an array with a single element `['foo']`. The arrayify function allows you to ensure that a value is treated as an array, even if it is a single value or a non-array type.

<figure><img src="https://files.readme.io/09fe8a5-Screenshot_2023-06-14_at_13.09.21.png" alt=""><figcaption></figcaption></figure>

### Before

The before function takes an array and a count "n" as input. It returns a new array containing all the elements in the original array that come before the specified count.

For example, if you use the before function with the array `[1, 2, 3]` and count `2`, it will return `[1, 2]`. This means that it retrieves all the items in the array before the specified count of 2. In this case, the elements `1` and `2` are returned. The before function allows you to extract the elements of an array up to a specific count and get all the elements that come before that count.

### Each index

The eachIndex function is used for iterating over an array. It takes two arguments: the array to iterate over and an optional set of options.

Within the iteration block, you can access two variables: "item" and "index". "item" represents the current element of the array being iterated, while "index" represents the index of that element.

Here's an example of using the eachIndex function:

handleBars

```Text
{{#eachIndex [1, 2, 3] }}  
  {{item}} is {{index}}  
{{/eachIndex}}
```

In this example, the eachIndex function is applied to the array `[1, 2, 3]`. It will iterate over each element of the array. For each iteration, it will output the value of the "item" variable (the current element) followed by "is" and the value of the "index" variable (the index of the current element).

The output of the above example would be:

Output

```Text
1 is 0  
2 is 1  
3 is 2
```

This demonstrates how the eachIndex function allows you to iterate over an array and access both the item value and its corresponding index during each iteration.

### Filter

The filter function is a block helper used for filtering an array based on a specific value or condition. It takes three arguments: the array to filter, the value or condition to filter by, and an optional set of options.

Within the block, you can check if each element of the array matches the specified value or condition. If an element matches, the block inside the filter function is rendered. If none of the elements match, the inverse block (specified by the "else" keyword) is rendered.

Here's an example of using the filter function:

handleBars

```Text
{{#filter [1, 2, 3] 2 }}
  2 Found
{{else}}
  2 not found
{{/filter}}
```

In this example, the filter function is applied to the array `[1, 2, 3]` with the value `2`. It will iterate over each element of the array and check if the element is equal to `2`. Since the element `2` is present in the array, the block inside the filter function is rendered, resulting in the output "2 Found".

If we modified the example to use a value that is not present in the array, like `4,` the output would be "2 not found".

The filter function allows you to selectively render different blocks based on whether or not elements in the array match a specified value or condition.

### First

The first function returns the first item or the first n items of an array. It allows you to retrieve a subset of an array starting from the beginning, either selecting just the first element or a specified number of elements.

This function is particularly useful when you need to access the initial portion of an array.

handleBars

```Text
{{ first array n }}
```

**Example** using the sample data and a data provider for the below example

handleBars

```Text
{{ first New Data Provider.Rows 2 }}
```

json

```Text
Brayden,Donald
```

Here, the function retrieves the first 2 items of the array: Brayden and Donald.

### forEach

The forEach function iterates over each item in an array and exposes the current item as context within the inner block. This allows you to access and display each element of the array within a template, along with additional helpful variables like the index and whether the current item is the first or last in the array.

This function is commonly used for looping over arrays in templates, making it easy to render lists of items dynamically.

handleBars

```Text
{{#forEach array}}
  {{context}}
{{/forEach}}
```

#### Parameters

* array (Array): The array you want to iterate over.
* options (Block Options): Defines the block of code where each array element is exposed as the current context.

**Example**

handleBars

```Text
{{#forEach New Data Provider.Rows }}
  {{Email}}
{{/forEach}}
```

In this example, the forEach function will iterate through the array and return:

```
brayden.carpenter@example.com
donald.lynch@example.com
francis.castro@example.com
holly.carroll@example.com
...
```

The inner block exposes each item in the array, allowing you to access the property name and render it.

### inArray

The inArray function is a block helper that checks whether a given value exists within an array. If the value is found, the block is rendered. Optionally, you can specify an inverse block to be rendered if the value is not found.

handleBars

```Text
{{#inArray array value}}
  <!-- Block content -->
{{else}}
  <!-- Inverse block content -->
{{/inArray}}
```

#### Parameters

* array (Array): The array in which to check for the value.
* value (Any): The value to search for within the array.
* options (Block Options): The content to render if the value exists, with an optional else block if the value does not exist.

**Example**

```
{{#inArray New Data Provider.Rows 2}}
  2 exists
{{else}}
  2 does not exist
{{/inArray}}
```

**Output**

```
2 does not exist
```

In this case, the value 2 isn't present in the array and therefore `2 does not exist` is outputted.

### isArray

The isArray function is used to check if a given value is an ES5 array. It takes one argument, which is the value to be checked.

When the isArray function is applied to a value, it checks if the value is an array. If it is, the function returns true. Otherwise, it returns false.

Here's an example of using the isArray function:

handleBars

```Text
{{ isArray [1, 2] }}
```

In this example, the isArray function is applied to the array `[1, 2]`. Since `[1, 2]` is indeed an array, the function will return true.

If we applied the isArray function to a non-array value, such as a string or number, it would return false.

The isArray function is useful when you need to determine if a value is an array before performing any array-specific operations or logic. It provides a simple way to check the type of a value and make decisions based on the result.

### itemAt

The itemAt function returns the item located at the specified index (idx) in an array. It provides an easy way to access a specific element by its index from an array.

handleBars

```Text
{{ itemAt array idx }}
```

#### Parameters

* array (Array): The array from which you want to retrieve an element.
* idx (Number): The zero-based index of the item you want to access.

**Example**

handleBars

```Text
{{ itemAt New Data Provider.Rows 2 }}
```

**Output**

```
Francis object
```

In this example, the function retrieves the item at index 2 from the array New Data Provider, which is the Francis object.

### Join

The join function is used to concatenate all the elements of an array into a single string. It takes two arguments: the array to be joined and an optional separator.

When the join function is applied to an array, it combines all the elements of the array into a string. By default, it uses a comma `(",")` as the separator between the elements. However, you can specify a custom separator as the second argument to the function.

Here's an example of using the join function:

handleBars

```Text
{{ join [1, 2, 3] }}
```

In this example, the join function is applied to the array `[1, 2, 3]`. Since no custom separator is provided, it uses the default separator of a comma. The resulting string is `"1,2,3"`.

If we wanted to use a different separator, such as a hyphen `("-")`, we can modify the example like this:

handleBars

```Text
{{ join [1, 2, 3] "-" }}
```

In this modified example, the join function is still applied to the array `[1, 2, 3]`, but we specify a custom separator of a hyphen `("-")`. The resulting string will be `"1-2-3".`

The join function is useful when you need to convert an array into a string representation, where the elements are concatenated with a specific separator. It allows you to control the formatting and structure of the resulting string.

### Last

The last function returns the last item or the last n items from an array or string. It provides a way to access the final elements, either as a single item or a subset from the end of the collection. This function is the opposite of the first function.

handleBars

```Text
{{ last value n }}
```

This helper does not need to supply the n value. You can include the array, and it will get the last object within it.

**Example**

handleBars

```Text
{{ last New Data Provider.Rows }}
```

**Output**

```
Suzy object
```

If you want to retrieve more than one item from the end, you can specify the n parameter:

handleBars

```Text
{{ last New Data Provider.Rows 2}}
```

This would output suzy and richard's objects from the New Data provider if using the sample data.

```
Suzy and Richard objects
```

### Equals length

The equalsLength function is used to compare the length of a value (such as an array or string) with a specified length. It takes three arguments: the value to be compared, the desired length, and optional options.

When the equalsLength function is applied, it checks if the length of the given value is equal to the specified length. If the lengths match, the function returns true. Otherwise, it returns false.

Here's an example of using the equalsLength function:

handleBars

```Text
{{ equalsLength '[1,2,3]' 3 }}
```

In this example, the equalsLength function is applied to the value `'[1,2,3]'` (which is an array) and the desired length of `3`. Since the length of the array is indeed 3, the function will return true.

The equalsLength function can be used to validate the length of arrays, strings, or other types of collections. By comparing the length with an expected value, you can determine if the length meets your criteria.

Note that the equalsLength function can be used both as a block helper and an inline helper. As a block helper, you can specify alternative content to be rendered. As an inline helper, the function simply returns the boolean result.

Overall, the equalsLength function provides a convenient way to compare the length of a value with a specified length in a handlebars template.

### Length

The length function returns the number of elements in a string or array. It takes one argument, which can be a string or an array. It returns the length of the string or the number of elements in the array.

Example:

handleBars

```Text
{{ length '[1, 2, 3]' }} -> 3
{{ length 'hello' }} -> 5
```

The length function is useful when you need to determine the size or number of elements in a string or array.

### Length equals

The lengthEqual function checks if the length of a given value (string or array) is equal to a specified length. It takes three arguments: the value, the expected length, and optional options. It returns true if the length of the value is equal to the specified length.

Example:

handleBars

```Text
{{ lengthEqual '[1, 2, 3]' 3 }} -> true
{{ lengthEqual 'hello' 5 }} -> true
{{ lengthEqual [1, 2, 3] 4 }} -> false
```

The lengthEqual function is useful when you need to compare the length of a string or array with a specific value to check for equality.

### Map

The map function applies a given function to each element of an array and returns a new array containing the results. It takes two arguments: the array to be mapped and the function to apply to each element.

Example:

handleBars

```Text
{{ map [1, 2, 3] double }} -> [2, 4, 6]
```

In the above example, the "double" function is applied to each element of the array `[1, 2, 3]`, resulting in a new array `[2, 4, 6]`. The map function is useful when you need to transform each element of an array based on a specific operation or function.

### Pluck

The pluck function allows you to extract a specific property from objects in a collection or array and returns an array of the extracted values. It takes two arguments: the collection or array of objects and the property to extract.

Example:

handleBars

```Text
{{ pluck [{ 'name': 'Bob' }] 'name' }} -> ['Bob']
```

In the above example, the pluck function is used to extract the 'name' property from the object `{ 'name': 'Bob' }`. It returns an array containing the extracted value `['Bob']`.

### Reverse

The reverse function allows you to reverse the elements in an array or the characters in a string. It takes one argument, which can be an array or a string.

Example:

handleBars

```Text
{{ reverse [1, 2, 3] }} -> [3, 2, 1]
```

In the above example, the reverse function is used to reverse the elements in the array `[1, 2, 3]`. It returns a new array `[3, 2, 1]` with the elements in reverse order.

You can also use the reverse function with a string to reverse the characters:

handleBars

```Text
{{ reverse "hello" }} -> "olleh"
```

In this case, the reverse function reverses the characters in the string "hello" and returns the string "olleh".

Note that the reverse function does not modify the original array or string. It creates a new array or string with the reversed elements or characters.

### Some

The "some" function is a block helper that checks if the callback function returns true for some value in the given array. It takes three arguments: the array to iterate over, the callback function, and an optional "provided" value.

Example:

handleBars

```Text
{{#some [1, 'b', 3] isString}} string found {{else}} No string found {{/some}} -> string found
```

In the above example, the "some" function is used to check if there is any string value in the array \[1, 'b', 3]. The "isString" callback function is applied to each element of the array. If the callback function returns true for any string value in the array, the block "string found" is rendered. Otherwise, the "No string found" block is rendered.

### Sort

The "sort" function is used to sort the elements of an array. It takes two arguments: the array to be sorted and an optional "key" or sorting function.

Example:

handleBars

```Text
{{ sort ['b', 'a', 'c'] }} -> ['a', 'b', 'c']
```

In the above example, the "sort" function is applied to the array `['b', 'a', 'c']`, and it returns a new array `['a', 'b', 'c']` with the elements sorted in ascending order.

### Sort by

The "sortBy" function is used to sort an array based on a specific property of the objects within the array. It takes two arguments: the array to be sorted and the property (or properties) to sort on.

Example:

handleBars

```Text
{{ sortBy [{a: 'zzz'}, {a: 'aaa'}] 'a' }} -> [{'a':'aaa'}, {'a':'zzz'}]
```

In the above example, the "sortBy" function is applied to the array `[{a: 'zzz'}, {a: 'aaa'}]` and the property 'a'. It returns a new array with the objects sorted in ascending order based on the values of the 'a' property. In this case, the resulting array is `[{'a':'aaa'}, {'a':'zzz'}]`.

Output

```Text
1, 2
3, 4
```

### withAfter

The withAfter function is a block helper that uses the items in an array after the specified index as context inside a block. This allows you to work with the subset of the array that starts right after the given index. It is the opposite of the withBefore function, which works with items before the specified index.

handleBars

```Text
{{#withAfter array idx}}
  <!-- Block content -->
{{/withAfter}}
```

#### Parameters

* array (Array): The array from which you want to select items.
* idx (Number): The zero-based index after which the items will be used.
* options (Block Options): The content to render for the array items that come after the specified index.

**Example**

handleBars

```Text
{{#withAfter New Data Provider.Rows 9}}
  {{this}}
{{/withAfter}}
```

**Output**

```
Suzy's object
```

In this example, the items after index 9 are used as the context inside the block, rendering object 10.

### withBefore

The withBefore function is a block helper that uses the items in an array before the specified index as context inside a block. This allows you to work with the subset of the array that ends right before the given index. It is the opposite of the withAfter function, which works with items after the specified index.

handleBars

```Text
{{#withBefore array idx}}
  <!-- Block content -->
{{/withBefore}}
```

**Example**

handleBars

```Text
{{#withBefore New Data Provider.Rows 9}}
  {{this}}
{{/withAfter}}
```

**Output**

```
All objects in the array before suzy ~ 9 total objects would be returned.
```

### withFirst

The withFirst function is a block helper that uses the first item in an array or collection as context inside a block. This allows you to access and work with only the first item in the array. It is the opposite of the withLast function, which works with the last item in a collection.

handleBars

```Text
{{#withFirst array}}
  <!-- Block content -->
{{/withFirst}}
```

**Example**

handleBars

```Text
{{#withFirst  New Data Provider.Rows }}
  {{this}}
{{/withFirst}}
```

**Output**

```
Brayden object
```

### withGroup

The withGroup function is a block helper that groups the elements of an array into subarrays of a specified size. Inside the block, each group is treated as an array, allowing you to work with a fixed number of items at a time.

handleBars

```Text
{{#withGroup array size}}
  <!-- Block content -->
{{/withGroup}}
```

#### Parameters

* array (Array): The array to be divided into groups.
* size (Number): The number of elements in each group.
* options (Block Options): The content to render for each group inside the block.

**Example**

handleBars

```Text
{{#withGroup New Data Provider.Rows 2}}
  {{#each this}}{{.}}{{/each}}

{{/withGroup}}
```

This will output the entire array and separate 2 objects with a space between each. This will look like the below.

![](https://files.readme.io/eaa088e13cafb3f928fce7785cc086e6a9ade6d3811ed37002c75003beb522e9-Screenshot\_2024-09-20\_at\_11.25.52.png)

### withLast

The withLast function is a block helper that uses the last item or the last n items in an array as context inside a block. This allows you to access and work with the final element(s) of the array. It is the opposite of the withFirst function, which focuses on the first item.

handleBars

```Text
{{#withLast array}}
  <!-- Block content -->
{{/withLast}}
```

**Example**

```
{{#withLast New Data Provider.Rows}}
  {{this}}
{{/withLast}}
```

This will output the last object in the array. You can use `{{ this }}` to select a specific field within the object. For example `{{ this.Email }}` will output the employees email.

### withSort

The withSort function is a block helper that sorts a collection and exposes the sorted collection as context inside the block. This allows you to work with elements in a defined order, making it easier to render them in a sorted format.

handleBars

```Text
{{#withSort array}}
  <!-- Block content -->
{{/withSort}}
```

#### Parameters

* array (Array): The collection you want to sort.
* prop (Optional String): This parameter can be used to specify a property to sort by, particularly useful for arrays of objects.
* options (Block Options): The content to render using the sorted collection as the context.

**Example**

handleBars

```Text
{{#withSort ['b', 'a', 'c']}}
  {{this}}
{{/withSort}}
```

**Output**

```
abc
```

### Unique

The "unique" function is a block helper that takes an array as input and returns a new array with all duplicate values removed. It is best used along with an "each" helper to iterate over the unique values.

Example:

handleBars

```Text
{{#each (unique ['a', 'a', 'c', 'b', 'e', 'e'])}}
  {{.}}
{{/each}}
```

In the above example, the "unique" function is applied to the array `['a', 'a', 'c', 'b', 'e', 'e']`. It removes the duplicate values from the array, resulting in a new array `['a', 'c', 'b', 'e']`. The "each" helper is then used to iterate over the unique values of the array. Inside the block, the "." refers to each unique value as it is iterated.

The output of the example would be:

Output

```Text
a
c
b
e
```
