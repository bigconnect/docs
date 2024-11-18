# Number

A number column is a good choice if you want to store data like age, scores, IDs, etc. The number column type is important when you need to do calculations within your data. Within the Number data type, you can provide min and max values. This allows you to add additional validation to your data type.

### Creating a number column

The process for creating a number column takes a few seconds.

1. First, click the `+` button along the top-right
2. Give your column a name
3. Choose Number as your data type
4. Give it a min of 1, and a max of 5
5. Click Save Column

### Using the number data type

In the Design section, you can pull in information from your number column, using the Repeater component and bindings

### Important considerations

* A number column can only include numeric values and cannot include letters and symbols.
* Telephone numbers need to be stored as a text/string data type because they often begin with a 0 and if they were stored as an integer then the leading zero would be discounted.
