# File Functions

Use file functions to return information about a file name or path. For example, you might use a file function to remove a file extension from a file path or to return part of the path.

You can replace any argument with a literal or an expression that evaluates to the argument. String literals must be enclosed in single or double quotation marks.

The expression language provides the following file functions:

### file:fileExtension(\<filepath>)

Returns the file extension from a file path. Uses the following argument:

* filepath - An absolute path to a file.

Return type: String.

For example, the following expression returns `"txt"`:

```
${file:fileExtension('/logs/weblog.txt')}
```

### file:fileName(\<filepath>)

Returns the file name from a file path. Uses the following argument:

* filepath - An absolute path to a file.

Return type: String.

For example, the following expression returns the filename, `"weblog.txt"`:

```
${file:fileName('/logs/weblog.txt')}
```

### file:parentPath(\<filepath>)

When used with a path to a file, returns the path to the file without the final separator, such as `/files` for `/files/file.log`.

When used with a path to a directory, returns the path to the directory without the final separator, such as `/serverA/logs` for `/serverA/logs/2016`.

Uses the following argument:

* filepath - An absolute path to a file.

Return type: String.For example, the following expression, which includes a path to a file, returns `"/serverB/logs"`:

```
${file:parentPath('/serverB/logs/weblog.txt')}
```

Similarly, the following expression, which includes a path to a directory, returns the parent directory, `"/serverB/logs"`:

```
${file:parentPath('/serverB/logs/weblogs')}
```

### file:pathElement(\<filepath>, \<integer>)

Returns the part of a path based on the specified integer. Uses the following arguments:

* filepath - An absolute path to a file.
* integer - The section of a path to return. Can return parts starting from the left or right side of the path:
  * To return a section of a path, counting from the left side of the path, use 0 and positive integers and start with 0.
  * To return a section of a path, counting from the right side of the path, use negative integers and start with -1.

Return type: String.

For example, to return the logs portion of the path, you can use either of the following expressions:

```
${file:pathElement('/logs/weblog.txt',0)}
${file:pathElement('/logs/weblog.txt',-2)}
```

### file:removeExtension(\<filepath>)

Returns the file path without the file extension. Uses the following argument:

* filepath - An absolute path to a file.

Return type: String.

For example, the following expression returns `"/logs/weblog"`:

```
${file:removeExtension('/logs/weblog.txt')}
```
