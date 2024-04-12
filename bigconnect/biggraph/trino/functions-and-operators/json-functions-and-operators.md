# JSON functions and operators

The SQL standard describes functions and operators to process JSON data. They allow you to access JSON data according to its structure, generate JSON data, and store it persistently in SQL tables.

Importantly, the SQL standard imposes that there is no dedicated data type to represent JSON data in SQL. Instead, JSON data is represented as character or binary strings. Although Trino supports `JSON` type, it is not used or produced by the following functions.

Trino supports three functions for querying JSON data: [json\_exists](https://trino.io/docs/current/functions/json.html#json-exists), [json\_query](https://trino.io/docs/current/functions/json.html#json-query), and [json\_value](https://trino.io/docs/current/functions/json.html#json-value). Each of them is based on the same mechanism of exploring and processing JSON input using JSON path.

Trino also supports two functions for generating JSON data – [json\_array](https://trino.io/docs/current/functions/json.html#json-array), and [json\_object](https://trino.io/docs/current/functions/json.html#json-object).

### JSON path language[#](https://trino.io/docs/current/functions/json.html#json-path-language) <a href="#json-path-language" id="json-path-language"></a>

The JSON path language is a special language, used exclusively by certain SQL operators to specify the query to perform on the JSON input. Although JSON path expressions are embedded in SQL queries, their syntax significantly differs from SQL. The semantics of predicates, operators, etc. in JSON path expressions generally follow the semantics of SQL. The JSON path language is case-sensitive for keywords and identifiers.

#### JSON path syntax and semantics[#](https://trino.io/docs/current/functions/json.html#json-path-syntax-and-semantics) <a href="#json-path-syntax-and-semantics" id="json-path-syntax-and-semantics"></a>

JSON path expressions are recursive structures. Although the name “path” suggests a linear sequence of operations going step by step deeper into the JSON structure, a JSON path expression is in fact a tree. It can access the input JSON item multiple times, in multiple ways, and combine the results. Moreover, the result of a JSON path expression is not a single item, but an ordered sequence of items. Each of the sub-expressions takes one or more input sequences, and returns a sequence as the result.

Note

In the lax mode, most path operations first unnest all JSON arrays in the input sequence. Any divergence from this rule is mentioned in the following listing. Path modes are explained in [JSON path modes](https://trino.io/docs/current/functions/json.html#json-path-modes).

The JSON path language features are divided into: literals, variables, arithmetic binary expressions, arithmetic unary expressions, and a group of operators collectively known as accessors.

**literals**[**#**](https://trino.io/docs/current/functions/json.html#literals)

*   numeric literals

    They include exact and approximate numbers, and are interpreted as if they were SQL values.

```
-1, 1.2e3, NaN
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

*   string literals

    They are enclosed in double quotes.

```
"Some text"
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

* boolean literals

```
true, false
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

*   null literal

    It has the semantics of the JSON null, not of SQL null. See [Comparison rules](https://trino.io/docs/current/functions/json.html#json-comparison-rules).

```
null
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

**variables**[**#**](https://trino.io/docs/current/functions/json.html#variables)

*   context variable

    It refers to the currently processed input of the JSON function.

```
$
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

*   named variable

    It refers to a named parameter by its name.

```
$param
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

*   current item variable

    It is used inside the filter expression to refer to the currently processed item from the input sequence.

```
@
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

*   last subscript variable

    It refers to the last index of the innermost enclosing array. Array indexes in JSON path expressions are zero-based.

```
last
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

**arithmetic binary expressions**[**#**](https://trino.io/docs/current/functions/json.html#arithmetic-binary-expressions)

The JSON path language supports five arithmetic binary operators:

```
<path1> + <path2>
<path1> - <path2>
<path1> * <path2>
<path1> / <path2>
<path1> % <path2>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Both operands, `<path1>` and `<path2>`, are evaluated to sequences of items. For arithmetic binary operators, each input sequence must contain a single numeric item. The arithmetic operation is performed according to SQL semantics, and it returns a sequence containing a single element with the result.

The operators follow the same precedence rules as in SQL arithmetic operations, and parentheses can be used for grouping.

**arithmetic unary expressions**[**#**](https://trino.io/docs/current/functions/json.html#arithmetic-unary-expressions)

```
+ <path>
- <path>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The operand `<path>` is evaluated to a sequence of items. Every item must be a numeric value. The unary plus or minus is applied to every item in the sequence, following SQL semantics, and the results form the returned sequence.

**member accessor**[**#**](https://trino.io/docs/current/functions/json.html#member-accessor)

The member accessor returns the value of the member with the specified key for each JSON object in the input sequence.

```
<path>.key
<path>."key"
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The condition when a JSON object does not have such a member is called a structural error. In the lax mode, it is suppressed, and the faulty object is excluded from the result.

Let `<path>` return a sequence of three JSON objects:

```
{"customer" : 100, "region" : "AFRICA"},
{"region" : "ASIA"},
{"customer" : 300, "region" : "AFRICA", "comment" : null}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

the expression `<path>.customer` succeeds in the first and the third object, but the second object lacks the required member. In strict mode, path evaluation fails. In lax mode, the second object is silently skipped, and the resulting sequence is `100, 300`.

All items in the input sequence must be JSON objects.

Note

Trino does not support JSON objects with duplicate keys.

**wildcard member accessor**[**#**](https://trino.io/docs/current/functions/json.html#wildcard-member-accessor)

Returns values from all key-value pairs for each JSON object in the input sequence. All the partial results are concatenated into the returned sequence.

```
<path>.*
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Let `<path>` return a sequence of three JSON objects:

```
{"customer" : 100, "region" : "AFRICA"},
{"region" : "ASIA"},
{"customer" : 300, "region" : "AFRICA", "comment" : null}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The results is:

```
100, "AFRICA", "ASIA", 300, "AFRICA", null
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

All items in the input sequence must be JSON objects.

The order of values returned from a single JSON object is arbitrary. The sub-sequences from all JSON objects are concatenated in the same order in which the JSON objects appear in the input sequence.

**descendant member accessor**[**#**](https://trino.io/docs/current/functions/json.html#descendant-member-accessor)

Returns the values associated with the specified key in all JSON objects on all levels of nesting in the input sequence.

```
<path>..key
<path>.."key"
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The order of returned values is that of preorder depth first search. First, the enclosing object is visited, and then all child nodes are visited.

This method does not perform array unwrapping in the lax mode. The results are the same in the lax and strict modes. The method traverses into JSON arrays and JSON objects. Non-structural JSON items are skipped.

Let `<path>` be a sequence containing a JSON object:

```
{
    "id" : 1,
    "notes" : [{"type" : 1, "comment" : "foo"}, {"type" : 2, "comment" : null}],
    "comment" : ["bar", "baz"]
}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
<path>..comment --> ["bar", "baz"], "foo", null
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

**array accessor**[**#**](https://trino.io/docs/current/functions/json.html#array-accessor)

Returns the elements at the specified indexes for each JSON array in the input sequence. Indexes are zero-based.

```
<path>[ <subscripts> ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `<subscripts>` list contains one or more subscripts. Each subscript specifies a single index or a range (ends inclusive):

```
<path>[<path1>, <path2> to <path3>, <path4>,...]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

In lax mode, any non-array items resulting from the evaluation of the input sequence are wrapped into single-element arrays. Note that this is an exception to the rule of automatic array wrapping.

Each array in the input sequence is processed in the following way:

* The variable `last` is set to the last index of the array.
* All subscript indexes are computed in order of declaration. For a singleton subscript `<path1>`, the result must be a singleton numeric item. For a range subscript `<path2> to <path3>`, two numeric items are expected.
* The specified array elements are added in order to the output sequence.

Let `<path>` return a sequence of three JSON arrays:

```
[0, 1, 2], ["a", "b", "c", "d"], [null, null]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The following expression returns a sequence containing the last element from every array:

```
<path>[last] --> 2, "d", null
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The following expression returns the third and fourth element from every array:

```
<path>[2 to 3] --> 2, "c", "d"
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note that the first array does not have the fourth element, and the last array does not have the third or fourth element. Accessing non-existent elements is a structural error. In strict mode, it causes the path expression to fail. In lax mode, such errors are suppressed, and only the existing elements are returned.

Another example of a structural error is an improper range specification such as `5 to 3`.

Note that the subscripts may overlap, and they do not need to follow the element order. The order in the returned sequence follows the subscripts:

```
<path>[1, 0, 0] --> 1, 0, 0, "b", "a", "a", null, null, null
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

**wildcard array accessor**[**#**](https://trino.io/docs/current/functions/json.html#wildcard-array-accessor)

Returns all elements of each JSON array in the input sequence.

```
<path>[*]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

In lax mode, any non-array items resulting from the evaluation of the input sequence are wrapped into single-element arrays. Note that this is an exception to the rule of automatic array wrapping.

The output order follows the order of the original JSON arrays. Also, the order of elements within the arrays is preserved.

Let `<path>` return a sequence of three JSON arrays:

```
[0, 1, 2], ["a", "b", "c", "d"], [null, null]
<path>[*] --> 0, 1, 2, "a", "b", "c", "d", null, null
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

**filter**[**#**](https://trino.io/docs/current/functions/json.html#filter)

Retrieves the items from the input sequence which satisfy the predicate.

```
<path>?( <predicate> )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

JSON path predicates are syntactically similar to boolean expressions in SQL. However, the semantics are different in many aspects:

* They operate on sequences of items.
* They have their own error handling (they never fail).
* They behave different depending on the lax or strict mode.

The predicate evaluates to `true`, `false`, or `unknown`. Note that some predicate expressions involve nested JSON path expression. When evaluating the nested path, the variable `@` refers to the currently examined item from the input sequence.

The following predicate expressions are supported:

* Conjunction

```
<predicate1> && <predicate2>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

* Disjunction

```
<predicate1> || <predicate2>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

* Negation

```
! <predicate>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

* `exists` predicate

```
exists( <path> )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Returns `true` if the nested path evaluates to a non-empty sequence, and `false` when the nested path evaluates to an empty sequence. If the path evaluation throws an error, returns `unknown`.

* `starts with` predicate

```
<path> starts with "Some text"
<path> starts with $variable
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The nested `<path>` must evaluate to a sequence of textual items, and the other operand must evaluate to a single textual item. If evaluating of either operand throws an error, the result is `unknown`. All items from the sequence are checked for starting with the right operand. The result is `true` if a match is found, otherwise `false`. However, if any of the comparisons throws an error, the result in the strict mode is `unknown`. The result in the lax mode depends on whether the match or the error was found first.

* `is unknown` predicate

```
( <predicate> ) is unknown
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Returns `true` if the nested predicate evaluates to `unknown`, and `false` otherwise.

* Comparisons

```
<path1> == <path2>
<path1> <> <path2>
<path1> != <path2>
<path1> < <path2>
<path1> > <path2>
<path1> <= <path2>
<path1> >= <path2>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Both operands of a comparison evaluate to sequences of items. If either evaluation throws an error, the result is `unknown`. Items from the left and right sequence are then compared pairwise. Similarly to the `starts with` predicate, the result is `true` if any of the comparisons returns `true`, otherwise `false`. However, if any of the comparisons throws an error, for example because the compared types are not compatible, the result in the strict mode is `unknown`. The result in the lax mode depends on whether the `true` comparison or the error was found first.

**COMPARISON RULES**[**#**](https://trino.io/docs/current/functions/json.html#comparison-rules)

Null values in the context of comparison behave different than SQL null:

* null == null –> `true`
* null != null, null < null, … –> `false`
* null compared to a scalar value –> `false`
* null compared to a JSON array or a JSON object –> `false`

When comparing two scalar values, `true` or `false` is returned if the comparison is successfully performed. The semantics of the comparison is the same as in SQL. In case of an error, e.g. comparing text and number, `unknown` is returned.

Comparing a scalar value with a JSON array or a JSON object, and comparing JSON arrays/objects is an error, so `unknown` is returned.

**EXAMPLES OF FILTER**[**#**](https://trino.io/docs/current/functions/json.html#examples-of-filter)

Let `<path>` return a sequence of three JSON objects:

```
{"customer" : 100, "region" : "AFRICA"},
{"region" : "ASIA"},
{"customer" : 300, "region" : "AFRICA", "comment" : null}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
<path>?(@.region != "ASIA") --> {"customer" : 100, "region" : "AFRICA"},
                                {"customer" : 300, "region" : "AFRICA", "comment" : null}
<path>?(!exists(@.customer)) --> {"region" : "ASIA"}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The following accessors are collectively referred to as **item methods**.

**double()**[**#**](https://trino.io/docs/current/functions/json.html#double)

Converts numeric or text values into double values.

```
<path>.double()
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Let `<path>` return a sequence `-1, 23e4, "5.6"`:

```
<path>.double() --> -1e0, 23e4, 5.6e0
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

**ceiling(), floor(), and abs()**[**#**](https://trino.io/docs/current/functions/json.html#ceiling-floor-and-abs)

Gets the ceiling, the floor or the absolute value for every numeric item in the sequence. The semantics of the operations is the same as in SQL.

Let `<path>` return a sequence `-1.5, -1, 1.3`:

```
<path>.ceiling() --> -1.0, -1, 2.0
<path>.floor() --> -2.0, -1, 1.0
<path>.abs() --> 1.5, 1, 1.3
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

**keyvalue()**[**#**](https://trino.io/docs/current/functions/json.html#keyvalue)

Returns a collection of JSON objects including one object per every member of the original object for every JSON object in the sequence.

```
<path>.keyvalue()
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The returned objects have three members:

* “name”, which is the original key,
* “value”, which is the original bound value,
* “id”, which is the unique number, specific to an input object.

Let `<path>` be a sequence of three JSON objects:

```
{"customer" : 100, "region" : "AFRICA"},
{"region" : "ASIA"},
{"customer" : 300, "region" : "AFRICA", "comment" : null}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
<path>.keyvalue() --> {"name" : "customer", "value" : 100, "id" : 0},
                      {"name" : "region", "value" : "AFRICA", "id" : 0},
                      {"name" : "region", "value" : "ASIA", "id" : 1},
                      {"name" : "customer", "value" : 300, "id" : 2},
                      {"name" : "region", "value" : "AFRICA", "id" : 2},
                      {"name" : "comment", "value" : null, "id" : 2}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

It is required that all items in the input sequence are JSON objects.

The order of the returned values follows the order of the original JSON objects. However, within objects, the order of returned entries is arbitrary.

**type()**[**#**](https://trino.io/docs/current/functions/json.html#type)

Returns a textual value containing the type name for every item in the sequence.

```
<path>.type()
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

This method does not perform array unwrapping in the lax mode.

The returned values are:

* `"null"` for JSON null,
* `"number"` for a numeric item,
* `"string"` for a textual item,
* `"boolean"` for a boolean item,
* `"date"` for an item of type date,
* `"time without time zone"` for an item of type time,
* `"time with time zone"` for an item of type time with time zone,
* `"timestamp without time zone"` for an item of type timestamp,
* `"timestamp with time zone"` for an item of type timestamp with time zone,
* `"array"` for JSON array,
* `"object"` for JSON object,

**size()**[**#**](https://trino.io/docs/current/functions/json.html#size)

Returns a numeric value containing the size for every JSON array in the sequence.

```
<path>.size()
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

This method does not perform array unwrapping in the lax mode. Instead, all non-array items are wrapped in singleton JSON arrays, so their size is `1`.

It is required that all items in the input sequence are JSON arrays.

Let `<path>` return a sequence of three JSON arrays:

```
[0, 1, 2], ["a", "b", "c", "d"], [null, null]
<path>.size() --> 3, 4, 2
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Limitations[#](https://trino.io/docs/current/functions/json.html#limitations) <a href="#limitations" id="limitations"></a>

The SQL standard describes the `datetime()` JSON path item method and the `like_regex()` JSON path predicate. Trino does not support them.

#### JSON path modes[#](https://trino.io/docs/current/functions/json.html#json-path-modes) <a href="#json-path-modes" id="json-path-modes"></a>

The JSON path expression can be evaluated in two modes: strict and lax. In the strict mode, it is required that the input JSON data strictly fits the schema required by the path expression. In the lax mode, the input JSON data can diverge from the expected schema.

The following table shows the differences between the two modes.

| Condition                                                                                                                                                                                                                                                                   | strict mode | lax mode                                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | -------------------------------------------------------------------------------------------------------------- |
| <p>Performing an operation which requires a non-array on an array, e.g.:</p><p><code>$.key</code> requires a JSON object</p><p><code>$.floor()</code> requires a numeric value</p>                                                                                          | ERROR       | The array is automatically unnested, and the operation is performed on each array element.                     |
| <p>Performing an operation which requires an array on an non-array, e.g.:</p><p><code>$[0]</code>, <code>$[*]</code>, <code>$.size()</code></p>                                                                                                                             | ERROR       | The non-array item is automatically wrapped in a singleton array, and the operation is performed on the array. |
| <p>A structural error: accessing a non-existent element of an array or a non-existent member of a JSON object, e.g.:</p><p><code>$[-1]</code> (array index out of bounds)</p><p><code>$.key</code>, where the input JSON object does not have a member <code>key</code></p> | ERROR       | The error is suppressed, and the operation results in an empty sequence.                                       |

**Examples of the lax mode behavior**[**#**](https://trino.io/docs/current/functions/json.html#examples-of-the-lax-mode-behavior)

Let `<path>` return a sequence of three items, a JSON array, a JSON object, and a scalar numeric value:

```
[1, "a", null], {"key1" : 1.0, "key2" : true}, -2e3
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The following example shows the wildcard array accessor in the lax mode. The JSON array returns all its elements, while the JSON object and the number are wrapped in singleton arrays and then unnested, so effectively they appear unchanged in the output sequence:

```
<path>[*] --> 1, "a", null, {"key1" : 1.0, "key2" : true}, -2e3
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

When calling the `size()` method, the JSON object and the number are also wrapped in singleton arrays:

```
<path>.size() --> 3, 1, 1
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

In some cases, the lax mode cannot prevent failure. In the following example, even though the JSON array is unwrapped prior to calling the `floor()` method, the item `"a"` causes type mismatch.

```
<path>.floor() --> ERROR
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### json\_exists[#](https://trino.io/docs/current/functions/json.html#json-exists) <a href="#json-exists" id="json-exists"></a>

The `json_exists` function determines whether a JSON value satisfies a JSON path specification.

```
JSON_EXISTS(
    json_input [ FORMAT JSON [ ENCODING { UTF8 | UTF16 | UTF32 } ] ],
    json_path
    [ PASSING json_argument [, ...] ]
    [ { TRUE | FALSE | UNKNOWN | ERROR } ON ERROR ]
    )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `json_path` is evaluated using the `json_input` as the context variable (`$`), and the passed arguments as the named variables (`$variable_name`). The returned value is `true` if the path returns a non-empty sequence, and `false` if the path returns an empty sequence. If an error occurs, the returned value depends on the `ON ERROR` clause. The default value returned `ON ERROR` is `FALSE`. The `ON ERROR` clause is applied for the following kinds of errors:

* Input conversion errors, such as malformed JSON
* JSON path evaluation errors, e.g. division by zero

`json_input` is a character string or a binary string. It should contain a single JSON item. For a binary string, you can specify encoding.

`json_path` is a string literal, containing the path mode specification, and the path expression, following the syntax rules described in [JSON path syntax and semantics](https://trino.io/docs/current/functions/json.html#json-path-syntax-and-semantics).

```
'strict ($.price + $.tax)?(@ > 99.9)'
'lax $[0 to 1].floor()?(@ > 10)'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

In the `PASSING` clause you can pass arbitrary expressions to be used by the path expression.

```
PASSING orders.totalprice AS O_PRICE,
        orders.tax % 10 AS O_TAX
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The passed parameters can be referenced in the path expression by named variables, prefixed with `$`.

```
'lax $?(@.price > $O_PRICE || @.tax > $O_TAX)'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Additionally to SQL values, you can pass JSON values, specifying the format and optional encoding:

```
PASSING orders.json_desc FORMAT JSON AS o_desc,
        orders.binary_record FORMAT JSON ENCODING UTF16 AS o_rec
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note that the JSON path language is case-sensitive, while the unquoted SQL identifiers are upper-cased. Therefore, it is recommended to use quoted identifiers in the `PASSING` clause:

```
'lax $.$KeyName' PASSING nation.name AS KeyName --> ERROR; no passed value found
'lax $.$KeyName' PASSING nation.name AS "KeyName" --> correct
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Examples[#](https://trino.io/docs/current/functions/json.html#examples) <a href="#examples" id="examples"></a>

Let `customers` be a table containing two columns: `id:bigint`, `description:varchar`.

| id  | description                                          |
| --- | ---------------------------------------------------- |
| 101 | ‘{“comment” : “nice”, “children” : \[10, 13, 16]}’   |
| 102 | ‘{“comment” : “problematic”, “children” : \[8, 11]}’ |
| 103 | ‘{“comment” : “knows best”, “children” : \[2]}’      |

The following query checks which customers have children above the age of 10:

```
SELECT
      id,
      json_exists(
                  description,
                  'lax $.children[*]?(@ > 10)'
                 ) AS children_above_ten
FROM customers
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| id  | children\_above\_ten |
| --- | -------------------- |
| 101 | true                 |
| 102 | true                 |
| 103 | false                |

In the following query, the path mode is strict. We check the third child for each customer. This should cause a structural error for the customers who do not have three or more children. This error is handled according to the `ON ERROR` clause.

```
SELECT
      id,
      json_exists(
                  description,
                  'strict $.children[2]?(@ > 10)'
                  UNKNOWN ON ERROR
                 ) AS child_3_above_ten
FROM customers
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| id  | child\_3\_above\_ten |
| --- | -------------------- |
| 101 | true                 |
| 102 | NULL                 |
| 103 | NULL                 |

### json\_query[#](https://trino.io/docs/current/functions/json.html#json-query) <a href="#json-query" id="json-query"></a>

The `json_query` function extracts a JSON value from a JSON value.

```
JSON_QUERY(
    json_input [ FORMAT JSON [ ENCODING { UTF8 | UTF16 | UTF32 } ] ],
    json_path
    [ PASSING json_argument [, ...] ]
    [ RETURNING type [ FORMAT JSON [ ENCODING { UTF8 | UTF16 | UTF32 } ] ] ]
    [ WITHOUT [ ARRAY ] WRAPPER |
      WITH [ { CONDITIONAL | UNCONDITIONAL } ] [ ARRAY ] WRAPPER ]
    [ { KEEP | OMIT } QUOTES [ ON SCALAR STRING ] ]
    [ { ERROR | NULL | EMPTY ARRAY | EMPTY OBJECT } ON EMPTY ]
    [ { ERROR | NULL | EMPTY ARRAY | EMPTY OBJECT } ON ERROR ]
    )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The constant string `json_path` is evaluated using the `json_input` as the context variable (`$`), and the passed arguments as the named variables (`$variable_name`).

The returned value is a JSON item returned by the path. By default, it is represented as a character string (`varchar`). In the `RETURNING` clause, you can specify other character string type or `varbinary`. With `varbinary`, you can also specify the desired encoding.

`json_input` is a character string or a binary string. It should contain a single JSON item. For a binary string, you can specify encoding.

`json_path` is a string literal, containing the path mode specification, and the path expression, following the syntax rules described in [JSON path syntax and semantics](https://trino.io/docs/current/functions/json.html#json-path-syntax-and-semantics).

```
'strict $.keyvalue()?(@.name == $cust_id)'
'lax $[5 to last]'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

In the `PASSING` clause you can pass arbitrary expressions to be used by the path expression.

```
PASSING orders.custkey AS CUST_ID
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The passed parameters can be referenced in the path expression by named variables, prefixed with `$`.

```
'strict $.keyvalue()?(@.value == $CUST_ID)'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Additionally to SQL values, you can pass JSON values, specifying the format and optional encoding:

```
PASSING orders.json_desc FORMAT JSON AS o_desc,
        orders.binary_record FORMAT JSON ENCODING UTF16 AS o_rec
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note that the JSON path language is case-sensitive, while the unquoted SQL identifiers are upper-cased. Therefore, it is recommended to use quoted identifiers in the `PASSING` clause:

```
'lax $.$KeyName' PASSING nation.name AS KeyName --> ERROR; no passed value found
'lax $.$KeyName' PASSING nation.name AS "KeyName" --> correct
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `ARRAY WRAPPER` clause lets you modify the output by wrapping the results in a JSON array. `WITHOUT ARRAY WRAPPER` is the default option. `WITH CONDITIONAL ARRAY WRAPPER` wraps every result which is not a singleton JSON array or JSON object. `WITH UNCONDITIONAL ARRAY WRAPPER` wraps every result.

The `QUOTES` clause lets you modify the result for a scalar string by removing the double quotes being part of the JSON string representation.

#### Examples[#](https://trino.io/docs/current/functions/json.html#id6) <a href="#id6" id="id6"></a>

Let `customers` be a table containing two columns: `id:bigint`, `description:varchar`.

| id  | description                                          |
| --- | ---------------------------------------------------- |
| 101 | ‘{“comment” : “nice”, “children” : \[10, 13, 16]}’   |
| 102 | ‘{“comment” : “problematic”, “children” : \[8, 11]}’ |
| 103 | ‘{“comment” : “knows best”, “children” : \[2]}’      |

The following query gets the `children` array for each customer:

```
SELECT
      id,
      json_query(
                 description,
                 'lax $.children'
                ) AS children
FROM customers
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| id  | children      |
| --- | ------------- |
| 101 | ‘\[10,13,16]’ |
| 102 | ‘\[8,11]’     |
| 103 | ‘\[2]’        |

The following query gets the collection of children for each customer. Note that the `json_query` function can only output a single JSON item. If you don’t use array wrapper, you get an error for every customer with multiple children. The error is handled according to the `ON ERROR` clause.

```
SELECT
      id,
      json_query(
                 description,
                 'lax $.children[*]'
                 WITHOUT ARRAY WRAPPER
                 NULL ON ERROR
                ) AS children
FROM customers
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| id  | children |
| --- | -------- |
| 101 | NULL     |
| 102 | NULL     |
| 103 | ‘2’      |

The following query gets the last child for each customer, wrapped in a JSON array:

```
SELECT
      id,
      json_query(
                 description,
                 'lax $.children[last]'
                 WITH ARRAY WRAPPER
                ) AS last_child
FROM customers
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| id  | last\_child |
| --- | ----------- |
| 101 | ‘\[16]’     |
| 102 | ‘\[11]’     |
| 103 | ‘\[2]’      |

The following query gets all children above the age of 12 for each customer, wrapped in a JSON array. The second and the third customer don’t have children of this age. Such case is handled according to the `ON EMPTY` clause. The default value returned `ON EMPTY` is `NULL`. In the following example, `EMPTY ARRAY ON EMPTY` is specified.

```
SELECT
      id,
      json_query(
                 description,
                 'strict $.children[*]?(@ > 12)'
                 WITH ARRAY WRAPPER
                 EMPTY ARRAY ON EMPTY
                ) AS children
FROM customers
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| id  | children   |
| --- | ---------- |
| 101 | ‘\[13,16]’ |
| 102 | ‘\[]’      |
| 103 | ‘\[]’      |

The following query shows the result of the `QUOTES` clause. Note that `KEEP QUOTES` is the default.

```
SELECT
      id,
      json_query(description, 'strict $.comment' KEEP QUOTES) AS quoted_comment,
      json_query(description, 'strict $.comment' OMIT QUOTES) AS unquoted_comment
FROM customers
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| id  | quoted\_comment | unquoted\_comment |
| --- | --------------- | ----------------- |
| 101 | ‘“nice”’        | ‘nice’            |
| 102 | ‘“problematic”’ | ‘problematic’     |
| 103 | ‘“knows best”’  | ‘knows best’      |

If an error occurs, the returned value depends on the `ON ERROR` clause. The default value returned `ON ERROR` is `NULL`. One example of error is multiple items returned by the path. Other errors caught and handled according to the `ON ERROR` clause are:

* Input conversion errors, such as malformed JSON
* JSON path evaluation errors, e.g. division by zero
* Output conversion errors

### json\_value[#](https://trino.io/docs/current/functions/json.html#json-value) <a href="#json-value" id="json-value"></a>

The `json_value` function extracts a scalar SQL value from a JSON value.

```
JSON_VALUE(
    json_input [ FORMAT JSON [ ENCODING { UTF8 | UTF16 | UTF32 } ] ],
    json_path
    [ PASSING json_argument [, ...] ]
    [ RETURNING type ]
    [ { ERROR | NULL | DEFAULT expression } ON EMPTY ]
    [ { ERROR | NULL | DEFAULT expression } ON ERROR ]
    )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `json_path` is evaluated using the `json_input` as the context variable (`$`), and the passed arguments as the named variables (`$variable_name`).

The returned value is the SQL scalar returned by the path. By default, it is converted to string (`varchar`). In the `RETURNING` clause, you can specify other desired type: a character string type, numeric, boolean or datetime type.

`json_input` is a character string or a binary string. It should contain a single JSON item. For a binary string, you can specify encoding.

`json_path` is a string literal, containing the path mode specification, and the path expression, following the syntax rules described in [JSON path syntax and semantics](https://trino.io/docs/current/functions/json.html#json-path-syntax-and-semantics).

```
'strict $.price + $tax'
'lax $[last].abs().floor()'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

In the `PASSING` clause you can pass arbitrary expressions to be used by the path expression.

```
PASSING orders.tax AS O_TAX
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The passed parameters can be referenced in the path expression by named variables, prefixed with `$`.

```
'strict $[last].price + $O_TAX'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Additionally to SQL values, you can pass JSON values, specifying the format and optional encoding:

```
PASSING orders.json_desc FORMAT JSON AS o_desc,
        orders.binary_record FORMAT JSON ENCODING UTF16 AS o_rec
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note that the JSON path language is case-sensitive, while the unquoted SQL identifiers are upper-cased. Therefore, it is recommended to use quoted identifiers in the `PASSING` clause:

```
'lax $.$KeyName' PASSING nation.name AS KeyName --> ERROR; no passed value found
'lax $.$KeyName' PASSING nation.name AS "KeyName" --> correct
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If the path returns an empty sequence, the `ON EMPTY` clause is applied. The default value returned `ON EMPTY` is `NULL`. You can also specify the default value:

```
DEFAULT -1 ON EMPTY
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If an error occurs, the returned value depends on the `ON ERROR` clause. The default value returned `ON ERROR` is `NULL`. One example of error is multiple items returned by the path. Other errors caught and handled according to the `ON ERROR` clause are:

* Input conversion errors, such as malformed JSON
* JSON path evaluation errors, e.g. division by zero
* Returned scalar not convertible to the desired type

#### Examples[#](https://trino.io/docs/current/functions/json.html#id8) <a href="#id8" id="id8"></a>

Let `customers` be a table containing two columns: `id:bigint`, `description:varchar`.

| id  | description                                          |
| --- | ---------------------------------------------------- |
| 101 | ‘{“comment” : “nice”, “children” : \[10, 13, 16]}’   |
| 102 | ‘{“comment” : “problematic”, “children” : \[8, 11]}’ |
| 103 | ‘{“comment” : “knows best”, “children” : \[2]}’      |

The following query gets the `comment` for each customer as `char(12)`:

```
SELECT id, json_value(
                      description,
                      'lax $.comment'
                      RETURNING char(12)
                     ) AS comment
FROM customers
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| id  | comment        |
| --- | -------------- |
| 101 | ‘nice ‘        |
| 102 | ‘problematic ‘ |
| 103 | ‘knows best ‘  |

The following query gets the first child’s age for each customer as `tinyint`:

```
SELECT id, json_value(
                      description,
                      'lax $.children[0]'
                      RETURNING tinyint
                     ) AS child
FROM customers
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| id  | child |
| --- | ----- |
| 101 | 10    |
| 102 | 8     |
| 103 | 2     |

The following query gets the third child’s age for each customer. In the strict mode, this should cause a structural error for the customers who do not have the third child. This error is handled according to the `ON ERROR` clause.

```
SELECT id, json_value(
                      description,
                      'strict $.children[2]'
                      DEFAULT 'err' ON ERROR
                     ) AS child
FROM customers
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| id  | child |
| --- | ----- |
| 101 | ‘16’  |
| 102 | ‘err’ |
| 103 | ‘err’ |

After changing the mode to lax, the structural error is suppressed, and the customers without a third child produce empty sequence. This case is handled according to the `ON EMPTY` clause.

```
SELECT id, json_value(
                      description,
                      'lax $.children[2]'
                      DEFAULT 'missing' ON EMPTY
                     ) AS child
FROM customers
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| id  | child     |
| --- | --------- |
| 101 | ‘16’      |
| 102 | ‘missing’ |
| 103 | ‘missing’ |

### json\_array[#](https://trino.io/docs/current/functions/json.html#json-array) <a href="#json-array" id="json-array"></a>

The `json_array` function creates a JSON array containing given elements.

```
JSON_ARRAY(
    [ array_element [, ...]
      [ { NULL ON NULL | ABSENT ON NULL } ] ],
    [ RETURNING type [ FORMAT JSON [ ENCODING { UTF8 | UTF16 | UTF32 } ] ] ]
    )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Argument types[#](https://trino.io/docs/current/functions/json.html#argument-types) <a href="#argument-types" id="argument-types"></a>

The array elements can be arbitrary expressions. Each passed value is converted into a JSON item according to its type, and optional `FORMAT` and `ENCODING` specification.

You can pass SQL values of types boolean, numeric, and character string. They are converted to corresponding JSON literals:

```
SELECT json_array(true, 12e-1, 'text')
--> '[true,1.2,"text"]'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Additionally to SQL values, you can pass JSON values. They are character or binary strings with a specified format and optional encoding:

```
SELECT json_array(
                  '[  "text"  ] ' FORMAT JSON,
                  X'5B0035005D00' FORMAT JSON ENCODING UTF16
                 )
--> '[["text"],[5]]'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can also nest other JSON-returning functions. In that case, the `FORMAT` option is implicit:

```
SELECT json_array(
                  json_query('{"key" : [  "value"  ]}', 'lax $.key')
                 )
--> '[["value"]]'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Other passed values are cast to varchar, and they become JSON text literals:

```
SELECT json_array(
                  DATE '2001-01-31',
                  UUID '12151fd2-7586-11e9-8f9e-2a86e4085a59'
                 )
--> '["2001-01-31","12151fd2-7586-11e9-8f9e-2a86e4085a59"]'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can omit the arguments altogether to get an empty array:

```
SELECT json_array() --> '[]'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Null handling[#](https://trino.io/docs/current/functions/json.html#null-handling) <a href="#null-handling" id="null-handling"></a>

If a value passed for an array element is `null`, it is treated according to the specified null treatment option. If `ABSENT ON NULL` is specified, the null element is omitted in the result. If `NULL ON NULL` is specified, JSON `null` is added to the result. `ABSENT ON NULL` is the default configuration:

```
SELECT json_array(true, null, 1)
--> '[true,1]'

SELECT json_array(true, null, 1 ABSENT ON NULL)
--> '[true,1]'

SELECT json_array(true, null, 1 NULL ON NULL)
--> '[true,null,1]'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Returned type[#](https://trino.io/docs/current/functions/json.html#returned-type) <a href="#returned-type" id="returned-type"></a>

The SQL standard imposes that there is no dedicated data type to represent JSON data in SQL. Instead, JSON data is represented as character or binary strings. By default, the `json_array` function returns varchar containing the textual representation of the JSON array. With the `RETURNING` clause, you can specify other character string type:

```
SELECT json_array(true, 1 RETURNING VARCHAR(100))
--> '[true,1]'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can also specify to use varbinary and the required encoding as return type. The default encoding is UTF8:

```
SELECT json_array(true, 1 RETURNING VARBINARY)
--> X'5b 74 72 75 65 2c 31 5d'

SELECT json_array(true, 1 RETURNING VARBINARY FORMAT JSON ENCODING UTF8)
--> X'5b 74 72 75 65 2c 31 5d'

SELECT json_array(true, 1 RETURNING VARBINARY FORMAT JSON ENCODING UTF16)
--> X'5b 00 74 00 72 00 75 00 65 00 2c 00 31 00 5d 00'

SELECT json_array(true, 1 RETURNING VARBINARY FORMAT JSON ENCODING UTF32)
--> X'5b 00 00 00 74 00 00 00 72 00 00 00 75 00 00 00 65 00 00 00 2c 00 00 00 31 00 00 00 5d 00 00 00'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### json\_object[#](https://trino.io/docs/current/functions/json.html#json-object) <a href="#json-object" id="json-object"></a>

The `json_object` function creates a JSON object containing given key-value pairs.

```
JSON_OBJECT(
    [ key_value [, ...]
      [ { NULL ON NULL | ABSENT ON NULL } ] ],
      [ { WITH UNIQUE [ KEYS ] | WITHOUT UNIQUE [ KEYS ] } ]
    [ RETURNING type [ FORMAT JSON [ ENCODING { UTF8 | UTF16 | UTF32 } ] ] ]
    )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Argument passing conventions[#](https://trino.io/docs/current/functions/json.html#argument-passing-conventions) <a href="#argument-passing-conventions" id="argument-passing-conventions"></a>

There are two conventions for passing keys and values:

```
SELECT json_object('key1' : 1, 'key2' : true)
--> '{"key1":1,"key2":true}'

SELECT json_object(KEY 'key1' VALUE 1, KEY 'key2' VALUE true)
--> '{"key1":1,"key2":true}'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

In the second convention, you can omit the `KEY` keyword:

```
SELECT json_object('key1' VALUE 1, 'key2' VALUE true)
--> '{"key1":1,"key2":true}'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Argument types[#](https://trino.io/docs/current/functions/json.html#id11) <a href="#id11" id="id11"></a>

The keys can be arbitrary expressions. They must be of character string type. Each key is converted into a JSON text item, and it becomes a key in the created JSON object. Keys must not be null.

The values can be arbitrary expressions. Each passed value is converted into a JSON item according to its type, and optional `FORMAT` and `ENCODING` specification.

You can pass SQL values of types boolean, numeric, and character string. They are converted to corresponding JSON literals:

```
SELECT json_object('x' : true, 'y' : 12e-1, 'z' : 'text')
--> '{"x":true,"y":1.2,"z":"text"}'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Additionally to SQL values, you can pass JSON values. They are character or binary strings with a specified format and optional encoding:

```
SELECT json_object(
                   'x' : '[  "text"  ] ' FORMAT JSON,
                   'y' : X'5B0035005D00' FORMAT JSON ENCODING UTF16
                  )
--> '{"x":["text"],"y":[5]}'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can also nest other JSON-returning functions. In that case, the `FORMAT` option is implicit:

```
SELECT json_object(
                   'x' : json_query('{"key" : [  "value"  ]}', 'lax $.key')
                  )
--> '{"x":["value"]}'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Other passed values are cast to varchar, and they become JSON text literals:

```
SELECT json_object(
                   'x' : DATE '2001-01-31',
                   'y' : UUID '12151fd2-7586-11e9-8f9e-2a86e4085a59'
                  )
--> '{"x":"2001-01-31","y":"12151fd2-7586-11e9-8f9e-2a86e4085a59"}'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can omit the arguments altogether to get an empty object:

```
SELECT json_object() --> '{}'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Null handling[#](https://trino.io/docs/current/functions/json.html#id12) <a href="#id12" id="id12"></a>

The values passed for JSON object keys must not be null. It is allowed to pass `null` for JSON object values. A null value is treated according to the specified null treatment option. If `NULL ON NULL` is specified, a JSON object entry with `null` value is added to the result. If `ABSENT ON NULL` is specified, the entry is omitted in the result. `NULL ON NULL` is the default configuration.:

```
SELECT json_object('x' : null, 'y' : 1)
--> '{"x":null,"y":1}'

SELECT json_object('x' : null, 'y' : 1 NULL ON NULL)
--> '{"x":null,"y":1}'

SELECT json_object('x' : null, 'y' : 1 ABSENT ON NULL)
--> '{"y":1}'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Key uniqueness[#](https://trino.io/docs/current/functions/json.html#key-uniqueness) <a href="#key-uniqueness" id="key-uniqueness"></a>

If a duplicate key is encountered, it is handled according to the specified key uniqueness constraint.

If `WITH UNIQUE KEYS` is specified, a duplicate key results in a query failure:

```
SELECT json_object('x' : null, 'x' : 1 WITH UNIQUE KEYS)
--> failure: "duplicate key passed to JSON_OBJECT function"
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note that this option is not supported if any of the arguments has a `FORMAT` specification.

If `WITHOUT UNIQUE KEYS` is specified, duplicate keys are not supported due to implementation limitation. `WITHOUT UNIQUE KEYS` is the default configuration.

#### Returned type[#](https://trino.io/docs/current/functions/json.html#id13) <a href="#id13" id="id13"></a>

The SQL standard imposes that there is no dedicated data type to represent JSON data in SQL. Instead, JSON data is represented as character or binary strings. By default, the `json_object` function returns varchar containing the textual representation of the JSON object. With the `RETURNING` clause, you can specify other character string type:

```
SELECT json_object('x' : 1 RETURNING VARCHAR(100))
--> '{"x":1}'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can also specify to use varbinary and the required encoding as return type. The default encoding is UTF8:

```
SELECT json_object('x' : 1 RETURNING VARBINARY)
--> X'7b 22 78 22 3a 31 7d'

SELECT json_object('x' : 1 RETURNING VARBINARY FORMAT JSON ENCODING UTF8)
--> X'7b 22 78 22 3a 31 7d'

SELECT json_object('x' : 1 RETURNING VARBINARY FORMAT JSON ENCODING UTF16)
--> X'7b 00 22 00 78 00 22 00 3a 00 31 00 7d 00'

SELECT json_object('x' : 1 RETURNING VARBINARY FORMAT JSON ENCODING UTF32)
--> X'7b 00 00 00 22 00 00 00 78 00 00 00 22 00 00 00 3a 00 00 00 31 00 00 00 7d 00 00 00'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Warning

The following functions and operators are not compliant with the SQL standard, and should be considered deprecated. According to the SQL standard, there shall be no `JSON` data type. Instead, JSON values should be represented as string values. The remaining functionality of the following functions is covered by the functions described previously.

### Cast to JSON[#](https://trino.io/docs/current/functions/json.html#cast-to-json) <a href="#cast-to-json" id="cast-to-json"></a>

The following types can be cast to JSON:

* `BOOLEAN`
* `TINYINT`
* `SMALLINT`
* `INTEGER`
* `BIGINT`
* `REAL`
* `DOUBLE`
* `VARCHAR`

Additionally, `ARRAY`, `MAP`, and `ROW` types can be cast to JSON when the following requirements are met:

* `ARRAY` types can be cast when the element type of the array is one of the supported types.
* `MAP` types can be cast when the key type of the map is `VARCHAR` and the value type of the map is a supported type,
* `ROW` types can be cast when every field type of the row is a supported type.

Note

Cast operations with supported [character string types](https://trino.io/docs/current/language/types.html#string-data-types) treat the input as a string, not validated as JSON. This means that a cast operation with a string-type input of invalid JSON results in a succesful cast to invalid JSON.

Instead, consider using the [`json_parse()`](https://trino.io/docs/current/functions/json.html#json\_parse) function to create validated JSON from a string.

The following examples show the behavior of casting to JSON with these types:

```
SELECT CAST(NULL AS JSON);
-- NULL

SELECT CAST(1 AS JSON);
-- JSON '1'

SELECT CAST(9223372036854775807 AS JSON);
-- JSON '9223372036854775807'

SELECT CAST('abc' AS JSON);
-- JSON '"abc"'

SELECT CAST(true AS JSON);
-- JSON 'true'

SELECT CAST(1.234 AS JSON);
-- JSON '1.234'

SELECT CAST(ARRAY[1, 23, 456] AS JSON);
-- JSON '[1,23,456]'

SELECT CAST(ARRAY[1, NULL, 456] AS JSON);
-- JSON '[1,null,456]'

SELECT CAST(ARRAY[ARRAY[1, 23], ARRAY[456]] AS JSON);
-- JSON '[[1,23],[456]]'

SELECT CAST(MAP(ARRAY['k1', 'k2', 'k3'], ARRAY[1, 23, 456]) AS JSON);
-- JSON '{"k1":1,"k2":23,"k3":456}'

SELECT CAST(CAST(ROW(123, 'abc', true) AS
            ROW(v1 BIGINT, v2 VARCHAR, v3 BOOLEAN)) AS JSON);
-- JSON '{"v1":123,"v2":"abc","v3":true}'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Casting from NULL to `JSON` is not straightforward. Casting from a standalone `NULL` will produce SQL `NULL` instead of `JSON 'null'`. However, when casting from arrays or map containing `NULL`s, the produced `JSON` will have `null`s in it.

### Cast from JSON[#](https://trino.io/docs/current/functions/json.html#cast-from-json) <a href="#cast-from-json" id="cast-from-json"></a>

Casting to `BOOLEAN`, `TINYINT`, `SMALLINT`, `INTEGER`, `BIGINT`, `REAL`, `DOUBLE` or `VARCHAR` is supported. Casting to `ARRAY` and `MAP` is supported when the element type of the array is one of the supported types, or when the key type of the map is `VARCHAR` and value type of the map is one of the supported types. Behaviors of the casts are shown with the examples below:

```
SELECT CAST(JSON 'null' AS VARCHAR);
-- NULL

SELECT CAST(JSON '1' AS INTEGER);
-- 1

SELECT CAST(JSON '9223372036854775807' AS BIGINT);
-- 9223372036854775807

SELECT CAST(JSON '"abc"' AS VARCHAR);
-- abc

SELECT CAST(JSON 'true' AS BOOLEAN);
-- true

SELECT CAST(JSON '1.234' AS DOUBLE);
-- 1.234

SELECT CAST(JSON '[1,23,456]' AS ARRAY(INTEGER));
-- [1, 23, 456]

SELECT CAST(JSON '[1,null,456]' AS ARRAY(INTEGER));
-- [1, NULL, 456]

SELECT CAST(JSON '[[1,23],[456]]' AS ARRAY(ARRAY(INTEGER)));
-- [[1, 23], [456]]

SELECT CAST(JSON '{"k1":1,"k2":23,"k3":456}' AS MAP(VARCHAR, INTEGER));
-- {k1=1, k2=23, k3=456}

SELECT CAST(JSON '{"v1":123,"v2":"abc","v3":true}' AS
            ROW(v1 BIGINT, v2 VARCHAR, v3 BOOLEAN));
-- {v1=123, v2=abc, v3=true}

SELECT CAST(JSON '[123,"abc",true]' AS
            ROW(v1 BIGINT, v2 VARCHAR, v3 BOOLEAN));
-- {v1=123, v2=abc, v3=true}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

JSON arrays can have mixed element types and JSON maps can have mixed value types. This makes it impossible to cast them to SQL arrays and maps in some cases. To address this, Trino supports partial casting of arrays and maps:

```
SELECT CAST(JSON '[[1, 23], 456]' AS ARRAY(JSON));
-- [JSON '[1,23]', JSON '456']

SELECT CAST(JSON '{"k1": [1, 23], "k2": 456}' AS MAP(VARCHAR, JSON));
-- {k1 = JSON '[1,23]', k2 = JSON '456'}

SELECT CAST(JSON '[null]' AS ARRAY(JSON));
-- [JSON 'null']
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

When casting from `JSON` to `ROW`, both JSON array and JSON object are supported.

### Other JSON functions[#](https://trino.io/docs/current/functions/json.html#other-json-functions) <a href="#other-json-functions" id="other-json-functions"></a>

In addition to the functions explained in more details in the preceding sections, the following functions are available:

is\_json\_scalar(_json_) → boolean[#](https://trino.io/docs/current/functions/json.html#is\_json\_scalar)

Determine if `json` is a scalar (i.e. a JSON number, a JSON string, `true`, `false` or `null`):

```
SELECT is_json_scalar('1');         -- true
SELECT is_json_scalar('[1, 2, 3]'); -- false
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)json\_array\_contains(_json_, _value_) → boolean[#](https://trino.io/docs/current/functions/json.html#json\_array\_contains)

Determine if `value` exists in `json` (a string containing a JSON array):

```
SELECT json_array_contains('[1, 2, 3]', 2); -- true
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)json\_array\_get(_json\_array_, _index_) → json[#](https://trino.io/docs/current/functions/json.html#json\_array\_get)

Warning

The semantics of this function are broken. If the extracted element is a string, it will be converted into an invalid `JSON` value that is not properly quoted (the value will not be surrounded by quotes and any interior quotes will not be escaped).

We recommend against using this function. It cannot be fixed without impacting existing usages and may be removed in a future release.

Returns the element at the specified index into the `json_array`. The index is zero-based:

```
SELECT json_array_get('["a", [3, 9], "c"]', 0); -- JSON 'a' (invalid JSON)
SELECT json_array_get('["a", [3, 9], "c"]', 1); -- JSON '[3,9]'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

This function also supports negative indexes for fetching element indexed from the end of an array:

```
SELECT json_array_get('["c", [3, 9], "a"]', -1); -- JSON 'a' (invalid JSON)
SELECT json_array_get('["c", [3, 9], "a"]', -2); -- JSON '[3,9]'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If the element at the specified index doesn’t exist, the function returns null:

```
SELECT json_array_get('[]', 0);                -- NULL
SELECT json_array_get('["a", "b", "c"]', 10);  -- NULL
SELECT json_array_get('["c", "b", "a"]', -10); -- NULL
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)json\_array\_length(_json_) → bigint[#](https://trino.io/docs/current/functions/json.html#json\_array\_length)

Returns the array length of `json` (a string containing a JSON array):

```
SELECT json_array_length('[1, 2, 3]'); -- 3
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)json\_extract(_json_, _json\_path_) → json[#](https://trino.io/docs/current/functions/json.html#json\_extract)

Evaluates the [JSONPath](http://goessner.net/articles/JsonPath/)-like expression `json_path` on `json` (a string containing JSON) and returns the result as a JSON string:

```
SELECT json_extract(json, '$.store.book');
SELECT json_extract(json, '$.store[book]');
SELECT json_extract(json, '$.store["book name"]');
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The [json\_query function](https://trino.io/docs/current/functions/json.html#json-query) provides a more powerful and feature-rich alternative to parse and extract JSON data.

json\_extract\_scalar(_json_, _json\_path_) → varchar[#](https://trino.io/docs/current/functions/json.html#json\_extract\_scalar)

Like [`json_extract()`](https://trino.io/docs/current/functions/json.html#json\_extract), but returns the result value as a string (as opposed to being encoded as JSON). The value referenced by `json_path` must be a scalar (boolean, number or string).

```
SELECT json_extract_scalar('[1, 2, 3]', '$[2]');
SELECT json_extract_scalar(json, '$.store.book[0].author');
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)json\_format(_json_) → varchar[#](https://trino.io/docs/current/functions/json.html#json\_format)

Returns the JSON text serialized from the input JSON value. This is inverse function to [`json_parse()`](https://trino.io/docs/current/functions/json.html#json\_parse).

```
SELECT json_format(JSON '[1, 2, 3]'); -- '[1,2,3]'
SELECT json_format(JSON '"a"');       -- '"a"'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note

[`json_format()`](https://trino.io/docs/current/functions/json.html#json\_format) and `CAST(json AS VARCHAR)` have completely different semantics.

[`json_format()`](https://trino.io/docs/current/functions/json.html#json\_format) serializes the input JSON value to JSON text conforming to [**RFC 7159**](https://datatracker.ietf.org/doc/html/rfc7159.html). The JSON value can be a JSON object, a JSON array, a JSON string, a JSON number, `true`, `false` or `null`.

```
SELECT json_format(JSON '{"a": 1, "b": 2}'); -- '{"a":1,"b":2}'
SELECT json_format(JSON '[1, 2, 3]');        -- '[1,2,3]'
SELECT json_format(JSON '"abc"');            -- '"abc"'
SELECT json_format(JSON '42');               -- '42'
SELECT json_format(JSON 'true');             -- 'true'
SELECT json_format(JSON 'null');             -- 'null'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

`CAST(json AS VARCHAR)` casts the JSON value to the corresponding SQL VARCHAR value. For JSON string, JSON number, `true`, `false` or `null`, the cast behavior is same as the corresponding SQL type. JSON object and JSON array cannot be cast to VARCHAR.

```
SELECT CAST(JSON '{"a": 1, "b": 2}' AS VARCHAR); -- ERROR!
SELECT CAST(JSON '[1, 2, 3]' AS VARCHAR);        -- ERROR!
SELECT CAST(JSON '"abc"' AS VARCHAR);            -- 'abc' (the double quote is gone)
SELECT CAST(JSON '42' AS VARCHAR);               -- '42'
SELECT CAST(JSON 'true' AS VARCHAR);             -- 'true'
SELECT CAST(JSON 'null' AS VARCHAR);             -- NULL
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)json\_parse(_string_) → json[#](https://trino.io/docs/current/functions/json.html#json\_parse)

Returns the JSON value deserialized from the input JSON text. This is inverse function to [`json_format()`](https://trino.io/docs/current/functions/json.html#json\_format):

```
SELECT json_parse('[1, 2, 3]');   -- JSON '[1,2,3]'
SELECT json_parse('"abc"');       -- JSON '"abc"'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note

[`json_parse()`](https://trino.io/docs/current/functions/json.html#json\_parse) and `CAST(string AS JSON)` have completely different semantics.

[`json_parse()`](https://trino.io/docs/current/functions/json.html#json\_parse) expects a JSON text conforming to [**RFC 7159**](https://datatracker.ietf.org/doc/html/rfc7159.html), and returns the JSON value deserialized from the JSON text. The JSON value can be a JSON object, a JSON array, a JSON string, a JSON number, `true`, `false` or `null`.

```
SELECT json_parse('not_json');         -- ERROR!
SELECT json_parse('["a": 1, "b": 2]'); -- JSON '["a": 1, "b": 2]'
SELECT json_parse('[1, 2, 3]');        -- JSON '[1,2,3]'
SELECT json_parse('"abc"');            -- JSON '"abc"'
SELECT json_parse('42');               -- JSON '42'
SELECT json_parse('true');             -- JSON 'true'
SELECT json_parse('null');             -- JSON 'null'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

`CAST(string AS JSON)` takes any VARCHAR value as input, and returns a JSON string with its value set to input string.

```
SELECT CAST('not_json' AS JSON);         -- JSON '"not_json"'
SELECT CAST('["a": 1, "b": 2]' AS JSON); -- JSON '"[\"a\": 1, \"b\": 2]"'
SELECT CAST('[1, 2, 3]' AS JSON);        -- JSON '"[1, 2, 3]"'
SELECT CAST('"abc"' AS JSON);            -- JSON '"\"abc\""'
SELECT CAST('42' AS JSON);               -- JSON '"42"'
SELECT CAST('true' AS JSON);             -- JSON '"true"'
SELECT CAST('null' AS JSON);             -- JSON '"null"'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)json\_size(_json_, _json\_path_) → bigint[#](https://trino.io/docs/current/functions/json.html#json\_size)

Like [`json_extract()`](https://trino.io/docs/current/functions/json.html#json\_extract), but returns the size of the value. For objects or arrays, the size is the number of members, and the size of a scalar value is zero.

```
SELECT json_size('{"x": {"a": 1, "b": 2}}', '$.x');   -- 2
SELECT json_size('{"x": [1, 2, 3]}', '$.x');          -- 3
SELECT json_size('{"x": {"a": 1, "b": 2}}', '$.x.a'); -- 0
```
