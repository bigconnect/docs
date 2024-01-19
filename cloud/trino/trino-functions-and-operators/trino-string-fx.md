# String

### String Operators <a href="#string-operators" id="string-operators"></a>

The `||` operator performs concatenation.

The `LIKE` statement can be used for pattern matching and is documented in Pattern Comparison: LIKE.

### String Functions <a href="#string-functions" id="string-functions"></a>

{% hint style="info" %}
These functions assume that the input strings contain valid UTF-8 encoded Unicode code points. There are no explicit checks for valid UTF-8 and the functions may return incorrect results on invalid UTF-8. Invalid UTF-8 data can be corrected with `from_utf8()`.

Additionally, the functions operate on Unicode code points and not user visible _characters_ (or _grapheme clusters_). Some languages combine multiple code points into a single user-perceived _character_, the basic unit of a writing system for a language, but the functions will treat each code point as a separate unit.

The `lower()` and `upper()` functions do not perform locale-sensitive, context-sensitive, or one-to-many mappings required for some languages. Specifically, this will return incorrect results for Lithuanian, Turkish and Azeri.
{% endhint %}

* `chr`(_n_) → varchar

Returns the Unicode code point `n` as a single character string.

* `codepoint`(_string_) → integer

Returns the Unicode code point of the only character of `string`.

* `concat`(_string1_, _..._, _stringN_) → varchar

Returns the concatenation of `string1`, `string2`, `...`, `stringN`. This function provides the same functionality as the SQL-standard concatenation operator (`||`).

* `format`(_format_, _args..._) → varchar

See `format()`.

* `hamming_distance`(_string1_, _string2_) → bigint

Returns the Hamming distance of `string1` and `string2`, i.e. the number of positions at which the corresponding characters are different. Note that the two strings must have the same length.

* `length`(_string_) → bigint

Returns the length of `string` in characters.

* `levenshtein_distance`(_string1_, _string2_) → bigint

Returns the Levenshtein edit distance of `string1` and `string2`, i.e. the minimum number of single-character edits (insertions, deletions or substitutions) needed to change `string1` into `string2`.

* `lower`(_string_) → varchar

Converts `string` to lowercase.

* `lpad`(_string_, _size_, _padstring_) → varchar

Left pads `string` to `size` characters with `padstring`. If `size` is less than the length of `string`, the result is truncated to `size` characters. `size` must not be negative and `padstring` must be non-empty.

* `ltrim`(_string_) → varchar

Removes leading whitespace from `string`.

* `position`(_substring IN string_) → bigint

Returns the starting position of the first instance of `substring` in `string`. Positions start with `1`. If not found, `0` is returned.

{% hint style="info" %}
This SQL-standard function has special syntax and uses the `IN` keyword for the arguments. See also `strpos()`.
{% endhint %}

* `replace`(_string_, _search_) → varchar

Removes all instances of `search` from `string`.

* `replace`(_string_, _search_, _replace_) → varchar

Replaces all instances of `search` with `replace` in `string`.

* `reverse`(_string_) → varchar

Returns `string` with the characters in reverse order.

* `rpad`(_string_, _size_, _padstring_) → varchar

Right pads `string` to `size` characters with `padstring`. If `size` is less than the length of `string`, the result is truncated to `size` characters. `size` must not be negative and `padstring` must be non-empty.

* `rtrim`(_string_) → varchar

Removes trailing whitespace from `string`.

* `split`(_string_, _delimiter) -> array(varchar_)

Splits `string` on `delimiter` and returns an array.

* `split`(_string_, _delimiter_, _limit) -> array(varchar_)

Splits `string` on `delimiter` and returns an array of size at most `limit`. The last element in the array always contain everything left in the `string`. `limit` must be a positive number.

* `split_part`(_string_, _delimiter_, _index_) → varchar

Splits `string` on `delimiter` and returns the field `index`. Field indexes start with `1`. If the index is larger than than the number of fields, then null is returned.

* `split_to_map`(_string_, _entryDelimiter_, _keyValueDelimiter_) → map\<varchar, varchar>

Splits `string` by `entryDelimiter` and `keyValueDelimiter` and returns a map. `entryDelimiter` splits `string` into key-value pairs. `keyValueDelimiter` splits each pair into key and value.

* `split_to_multimap`(_string_, _entryDelimiter_, _keyValueDelimiter) -> map(varchar_, _array(varchar)_)

Splits `string` by `entryDelimiter` and `keyValueDelimiter` and returns a map containing an array of values for each unique key. `entryDelimiter` splits `string` into key-value pairs. `keyValueDelimiter` splits each pair into key and value. The values for each key will be in the same order as they appeared in `string`.

* `strpos`(_string_, _substring_) → bigint

Returns the starting position of the first instance of `substring` in `string`. Positions start with `1`. If not found, `0` is returned.`strpos`(_string_, _substring_, _instance_) → bigint

Returns the position of the N-th `instance` of `substring` in `string`. When `instance` is a negative number the search will start from the end of `string`. Positions start with `1`. If not found, `0` is returned.

* `starts_with`(_string_, _substring_) → boolean

Tests whether `substring` is a prefix of `string`.

* `substr`(_string_, _start_) → varchar

This is an alias for `substring()`.`substring`(_string_, _start_) → varchar[#](https://prestosql.io/docs/current/functions/string.html#substring)

Returns the rest of `string` from the starting position `start`. Positions start with `1`. A negative starting position is interpreted as being relative to the end of the string.

* `substr`(_string_, _start_, _length_) → varchar

This is an alias for `substring()`.`substring`(_string_, _start_, _length_) → varchar

Returns a substring from `string` of length `length` from the starting position `start`. Positions start with `1`. A negative starting position is interpreted as being relative to the end of the string.

* `translate`(_source_, _from_, _to_) → varchar

Returns the `source` string translated by replacing characters found in the `from` string with the corresponding characters in the `to` string. If the `from` string contains duplicates, only the first is used. If the `source` character does not exist in the `from` string, the `source` character will be copied without translation. If the index of the matching character in the `from` string is beyond the length of the `to` string, the `source` character will be omitted from the resulting string.

Here are some examples illustrating the translate function:

```sql
SELECT translate('abcd', '', ''); -- 'abcd'
SELECT translate('abcd', 'a', 'z'); -- 'zbcd'
SELECT translate('abcda', 'a', 'z'); -- 'zbcdz'
SELECT translate('Palhoça', 'ç','c'); -- 'Palhoca'
SELECT translate('abcd', 'b', U&'\+01F600'); -- a😀cd
SELECT translate('abcd', 'a', ''); -- 'bcd'
SELECT translate('abcd', 'a', 'zy'); -- 'zbcd'
SELECT translate('abcd', 'ac', 'z'); -- 'zbd'
SELECT translate('abcd', 'aac', 'zq'); -- 'zbd'
```

* `trim`(_string_) → varchar

Removes leading and trailing whitespace from `string`.

* `upper`(_string_) → varchar

Converts `string` to uppercase.

* `word_stem`(_word_) → varchar

Returns the stem of `word` in the English language.

* `word_stem`(_word_, _lang_) → varchar

Returns the stem of `word` in the `lang` language.

### Unicode Functions <a href="#unicode-functions" id="unicode-functions"></a>

* `normalize`(_string_) → varchar

Transforms `string` with NFC normalization form.

* `normalize`(_string_, _form_) → varchar

Transforms `string` with the specified normalization form. `form` must be be one of the following keywords:

| Form   | Description                                                    |
| ------ | -------------------------------------------------------------- |
| `NFD`  | Canonical Decomposition                                        |
| `NFC`  | Canonical Decomposition, followed by Canonical Composition     |
| `NFKD` | Compatibility Decomposition                                    |
| `NFKC` | Compatibility Decomposition, followed by Canonical Composition |

{% hint style="info" %}
This SQL-standard function has special syntax and requires specifying `form` as a keyword, not as a string.
{% endhint %}

* `to_utf8`(_string_) → varbinary

Encodes `string` into a UTF-8 varbinary representation.

* `from_utf8`(_binary_) → varchar

Decodes a UTF-8 encoded string from `binary`. Invalid UTF-8 sequences are replaced with the Unicode replacement character `U+FFFD`.

* `from_utf8`(_binary_, _replace_) → varchar

Decodes a UTF-8 encoded string from `binary`. Invalid UTF-8 sequences are replaced with `replace`. The replacement string `replace` must either be a single character or empty (in which case invalid characters are removed).
