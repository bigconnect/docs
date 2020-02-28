# Text Features

## Preprocessing

Text features are treated in the same way of sequence features, with a couple differences. Two different formattings/splittings happen, one that splits at every character and one that uses a spaCy based tokenizer \(and removes stopwords\) are used, and two different key are added to the HDF5 file, one containing the matrix of characters and one containing the matrix of words. The same thing happens in the JSON file, where there are dictionaries for mapping characters to integers \(and the inverse\) and words to integers \(and their inverse\). In the model definition you are able to specify which level of representation to use, if the character level or the word level.

The parameters available for preprocessing are:

* `missing_value_strategy` \(default `fill_with_const`\): what strategy to follow when there's a missing value in a binary column. The value should be one of `fill_with_const` \(replaces the missing value with a specific value specified with the `fill_value` parameter\), `fill_with_mode` \(replaces the missing values with the most frequent value in the column\), `fill_with_mean` \(replaces the missing values with the mean of the values in the column\), `backfill` \(replaces the missing values with the next valid value\).
* `fill_value` \(default `""`\): the value to replace the missing values with in case the `missing_value_strategy` is `fill-value`.
* `padding` \(default `right`\): the direction of the padding. `right` and `left` are available options.
* `padding_symbol` \(default `<PAD>`\): the string used as a padding symbol. Is is mapped to the integer ID 0 in the vocabulary.
* `unknown_symbol` \(default `<UNK>`\): the string used as a unknown symbol. Is is mapped to the integer ID 1 in the vocabulary.
* `lowercase` \(default `false`\): if the string has to be lowercased before being handled by the formatter.
* `word_sequence_length_limit` \(default `256`\): the maximum length of the text in words. Texts that are longer than this value will be truncated, while texts that are shorter will be padded.
* `word_tokenizer` \(default `space_punct`\): defines how to map from the raw string content of the CSV column to a sequence of words. The default value `space_punct` splits the string using a regular expression that separates also punctuation. Other options are: `space` \(splits on space\), `underscore` \(splits on underscore\), `comma`\(splits on comma\), `json` \(decodes the string into a set or a list through a JSON parser\), and a set of format functions that rely on [spaCy](https://spacy.io/).
* `word_most_common` \(default `20000`\): the maximum number of most common words to be considered. If the data contains more than this amount, the most infrequent words will be treated as unknown.
* `char_sequence_length_limit` \(default `1024`\): the maximum length of the text in characters. Texts that are longer than this value will be truncated, while sequences that are shorter will be padded.
* `char_tokenizer` \(default `characters`\): defines how to map from the raw string content of the CSV column to a sequence of characters. The default value and only available option is `characters` and the behavior is to split the string at each character.
* `char_most_common` \(default `70`\): the maximum number of most common characters to be considered. if the data contains more than this amount, the most infrequent characters will be treated as unknown.

### **SPACY BASED WORD FORMAT OPTIONS**

The spaCy based `tokenizer` options are functions that use the powerful tokenization and NLP preprocessing models provided the library. Several languages are available: English \(code `en`\), Italian \(code `it`\), Spanish \(code `es`\), German \(code `de`\), French \(code `fr`\), Portuguese \(code `pt`\), Dutch \(code `nl`\), Greek \(code `el`\) and Multi \(code `xx`, useful in case you have a dataset of different languages\). For each language different functions are available: - `tokenize`: uses spaCy tokenizer, - `tokenize_filter`: uses spaCy tokenizer and filters out punctuation, numbers, stopwords and words shorter than 3 characters, - `tokenize_remove_stopwords`: uses spaCy tokenizer and filters out stopwords, - `lemmatize`: uses spaCy lemmatizer, - `lemmatize_filter`: uses spaCy lemmatizer and filters out punctuation, numbers, stopwords and words shorter than 3 characters, - `lemmatize_remove_stopwords`: uses spaCy lemmatize and filters out stopwords.

In order to use these options, you have to download the the spaCy model:

```yaml
python -m spacy download <language_code>
```

and provide `<language>_<function>` as `tokenizer` like: `english_tokenizer`, `italian_lemmatize_filter`, `multi_tokenize_filter` and so on. More details on the models can be found in the [spaCy documentation](https://spacy.io/models).

## Input Features and Encoders

The encoders are the same used for the **Sequence Features**. The only difference is that you can specify an additional `level` parameter with possible values `word` or `char` to force to use the text words or characters as inputs \(by default the encoder will use `word`\).

## Output Features and Decoders

The decoders are the same used for the **Sequence Features**. The only difference is that you can specify an additional `level` parameter with possible values `word` or `char` to force to use the text words or characters as inputs \(by default the encoder will use `word`\).

## Measures

The measures are the same used for the **Sequence Features**.

