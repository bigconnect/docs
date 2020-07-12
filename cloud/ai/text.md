# Text

Text information is processed various out-of-the-box Data Workers listed below.

BigConnect uses the following NLP frameworks to perform various tasks:

* [spaCy](https://spacy.io/)
* [Stanford CoreNLP](https://stanfordnlp.github.io/CoreNLP/)
* Cloud based APIs \(Google, Azure\)

Custom models have been trained for both frameworks and are available as part of the provided Data Workers.

## Entity Extractor

BigConnect has an advanced, pluggable [Named Entity Recognition](https://en.wikipedia.org/wiki/Named-entity_recognition) \(NER\) engine. By default it extracts **Persons**, **Locations** and **Organizations** but can be easily extended to work with other labels as well.

Besides just relying on a trained model to perform NER, BigConnect also employs advanced heuristics to filter out bad or ambiguous entities.

New **spaCy** entity extraction models can be easily trained for domain-specific applications usind the provided [Data Labeling]() tool.

## Relationship Extractor

Extracting relationships from text is not a trivial job. The process involves advanced disambiguation techniques and is in many ways similar to [Information Extraction \(IE\)](https://en.wikipedia.org/wiki/Information_extraction). Relations are extracted between two entities that are present in a piece of text.

## Sentiment Analysis

Sentiment Analysis is a special case of text classification. This Data Worker classifies a text by three labels: POSITIVE, NEUTRAL, NEGATIVE.

## Classifier

This data worker receives textual information and applies a label based on the configured method of classification: `dictionary` or `trained model`.

{% hint style="info" %}
**spaCy** classification models can be easily trained using the [Data Labeling]() tool provided by BigConnect.
{% endhint %}

## Translation

This data worker perform text translation from the input language to a desired language. The following translation systems are supported:

* [Google Translate](https://cloud.google.com/translate/) \(online\)
* [SYSTRAN Pure Neural Server](https://www.systransoft.com/translation-products/systran-pure-neural-server/) \(offline\)

## Summarizer

This data worker received a text and returns a short summary of the provided text.

## Language Detector

This data worker received a text and returns the detected language. 

All languages are supported.

## Supported languages

The supported languages varies depending on the type of task and existing implementation. The most accurate models are the ones for the English language obviously.

If a language is not supported, we will [translate](text.md#translation) the information to English and perform the analysis on the translated text. 

{% hint style="info" %}
We found that this approach works the best for non-English languages.
{% endhint %}



