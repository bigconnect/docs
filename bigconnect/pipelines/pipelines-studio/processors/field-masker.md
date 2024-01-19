# Field Masker

The Field Masker masks string values based on the selected mask type. You can use variable-length, fixed-length, custom, or regular expression masks. Custom masks can reveal part of the string value.

Use the Field Masker to mask sensitive string data. For example, you might use a custom mask to mask the last four digits of a phone number.

Tip: To mask non-string data, you might use the Field Type Converter processor to convert non-string data to String, then pass the data to the Field Masker.

### Mask Types <a href="#ariaid-title2" id="ariaid-title2"></a>

You can use the following mask types to mask data:

Fixed-lengthReplaces values with a fixed-length mask. Use when you want to mask variations in the length of the data.The following example uses a fixed-length mask to hide passwords:

| Original Password                                                                    | Fixed-Length Mask                                                                       |
| ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------- |
| <p><code>1234</code></p><p><code>donKey</code></p><p><code>022367snowfall</code></p> | <p><code>asd302kd0</code></p><p><code>2v03msO3d</code></p><p><code>L92m1sN3q</code></p> |

Variable-lengthReplaces values with a variable-length mask. Use when you want to reveal the lengths of the original data.The following example uses a variable length mask to hide the same passwords:

| Original Password                                                                    | Variable-Length Mask                                                                      |
| ------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------- |
| <p><code>1234</code></p><p><code>donKey</code></p><p><code>022367snowfall</code></p> | <p><code>asd3</code></p><p><code>2v03ms</code></p><p><code>L92m1sN3q0jaOmE67Ws</code></p> |

CustomReplaces values with a mask based on a user-defined pattern. When you define the pattern for the mask, you can use a hash mark (#) to reveal the character in that location. All other characters are used as constants in the mask.The following example uses `###-xxx-xxxx` as the mask pattern to reveal the area code of a phone number while masking the rest of the number:

| Original Phone Number                                                                            | Custom Mask (###-xxx-xxxx)                                                                       |
| ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| <p><code>415-333-3434</code></p><p><code>301-999-0987</code></p><p><code>617-567-8888</code></p> | <p><code>415-xxx-xxxx</code></p><p><code>301-xxx-xxxx</code></p><p><code>617-xxx-xxxx</code></p> |

Tip: To avoid confusing masking characters for real data, use one masking character instead of a mix of masking characters.The length of a custom mask is either the original data length or the mask pattern length, whichever is smaller. For example, you use `###xx` as the mask pattern to reveal the three digit zip code range while masking the rest of the zip code. The mask pattern length is five characters. When the Field Masker applies the mask to an original zip code with ten characters, it uses the minimum length of five characters, removing the last five characters of the original zip code. When the processor applies the mask to an original zip code with three characters, it uses the minimum length of three characters, revealing those three characters and then not masking any characters, as follows:

| Original Zip Code                                                                                       | Custom Mask (###xx)                                                                                |
| ------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| <p><code>94105</code></p><p><code>94086-6161</code></p><p><code>80123</code></p><p><code>703</code></p> | <p><code>941xx</code></p><p><code>940xx</code></p><p><code>801xx</code></p><p><code>703</code></p> |

Regular ExpressionReplaces groups of values with a variable-length mask. You define the data structure with a regular expression, using parentheses to define groups of values. You can optionally specify any groups of data that you do not want to mask. If you do not specify groups, Field Masker masks all values.For example, you use the following regular expression to describe data that appends a five-digit code to a social security number:

```
([0-9]{5}) - ([0-9]{3}-[0-9]{2}-[0-9]{4}) 
```

The parentheses creates two groups of data. If you configure the stage to reveal the first group, then the results of the mask might look as follows:

```
Regex Mask
30529-xxx-xx-xxxx
10384-xxx-xx-xxxx
95833-xxx-xx-xxxx
```

### Configuring a Field Masker <a href="#ariaid-title3" id="ariaid-title3"></a>

Configure a Field Masker to mask sensitive data.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Mask tab, configure the following properties:

    | Field Masker Property | Description                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Fields to Mask        | <p>One or more String fields to mask with the same mask type.</p><p>You can use the asterisk wildcard to represent array indices and map elements.</p><p>You can specify individual fields or use a field path expression to specify a set of fields.</p>                                                                                                                                                                                       |
    | Mask Type             | <p>Mask type to hide field values. Select one of the following options:</p><ul><li>Fixed-length - Replaces values with a fixed-length mask.</li><li>Variable-length - Replaces values with a mask the length of the original value.</li><li>Custom - Replaces values with a user-defined mask.</li><li>Regular Expression - Replaces groups of values based on the groups defined by the regular expression and the groups to reveal.</li></ul> |
    | Custom Mask           | <p>Mask pattern for a custom mask. Enter the pattern that you want to use.</p><p>Use the hash mark (#) to display characters in the specified location. Use any other character as a masking character.</p>                                                                                                                                                                                                                                     |
    | Regular Expression    | <p>Regular expression that describes the data in the masked fields.</p><p>If you want to display a group of data, use parentheses to define groups within the pattern. For example, ([0-9]{5}) - ([0-9]{3}-[0-9]{2}-[0-9]{4}) .</p>                                                                                                                                                                                                             |
    | Groups to Show        | Optional comma-separated list of groups to show. Use 1 to represent the first group.                                                                                                                                                                                                                                                                                                                                                            |
3. To mask another field, click the Add icon, and then repeat the previous step. You can use simple or bulk edit mode to mask another field.
