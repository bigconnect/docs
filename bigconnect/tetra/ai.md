# AI Models

The AI Service contains a number of carefully chosen deep-learning models to fulfill tasks like Named Entity Recognition, Translation, Sentiment analysis and more.

The following AI models are available as REST services:

* Named Entity Recognition
* Sentiment Analysis
* Language Detection
* Text in the Wild & OCR
* Text summarization
* Text classification
* Sentence Similarity
* Face detection
* Face recognition
* Face age detection
* Face features extractor
* Face gender detection
* Face emotion detection
* Object detection
* Location resolver
* License Plate Recognition
* Speech-to-Text
* Image Metadata extractor (EXIF)&#x20;
* Topic modeling
* Image captioning

Romanian and English language is available for all textual models.

Each service has its own JSON input requirements for text, image, video or audio content. The actual content needs to be Base64 encoded and placed in an appropriate JSON property, as follows:

{% tabs %}
{% tab title="Text" %}
```javascript
{
    "text": "<BASE64_ENCODED_VALUE>"
}
```
{% endtab %}

{% tab title="Image" %}
```javascript
{
    "image": "<BASE64_ENCODED_VALUE>"
}
```
{% endtab %}

{% tab title="Audio" %}
```javascript
{
    "audio": "<BASE64_ENCODED_VALUE>"
}
```
{% endtab %}
{% endtabs %}

### Example

Predict sentiment of sentence: "_The bright spot in the banking industry has been the investment banks because they are cashing in on resurgent capital markets._"

1. Base64 encode the sentence. The result should be:

```
VGhlIGJyaWdodCBzcG90IGluIHRoZSBiYW5raW5nIGluZHVzdHJ5IGhhcyBiZWVuIHRoZSBpbnZl
c3RtZW50IGJhbmtzIGJlY2F1c2UgdGhleSBhcmUgY2FzaGluZyBpbiBvbiByZXN1cmdlbnQgY2Fw
aXRhbCBtYXJrZXRzLgo=
```

2\. Call the service REST endpoint using `curl` :

```bash
curl -XPOST https://ai-170393591705357.cloud.bigconnect.io/api/sentiment-analysis/1/predict -d'  
{
    "text": "VGhlIGJyaWdodCBzcG90IGluIHRoZSBiYW5raW5nIGluZHVzdHJ5IGhhcyBiZWVuIHRoZSBpbnZlc3RtZW50IGJhbmtzIGJlY2F1c2UgdGhleSBhcmUgY2FzaGluZyBpbiBvbiByZXN1cmdlbnQgY2FwaXRhbCBtYXJrZXRzLg=="
}
'
```

The output will be:

`{"entities": [{"sentiment": "Positive"}]}`
