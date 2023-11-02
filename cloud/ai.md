# Intelligence

The AI Service contains a number of carefully chosen deep-learning models to fulfill tasks like Named Entity Recognition, Translation, Sentiment analysis and more.

The following AI models are available as REST services:

* Named Entity Recognition
* Text in the Wild & OCR
* Personality Insights
* Sentiment
* Face detection
* Face recognition
* Face age detection
* Face features extractor
* Face gender detection
* Face emotion detection
* Object detection
* Location resolver
* License Plate Recognition
* Speech to Text (American English only)
* Fraud detection
* Image Metadata extractor (EXIF)&#x20;
* Topic modeling
* Image captioning

The API endpoints are listed by each service in the URL section:

![](<../.gitbook/assets/image (409).png>)

To make a call, prepend the service URL to the URL endpoint listed in each box. Example:

`https://ai-170393591705357.cloud.bigconnect.io/<ENDPOINT>`

{% hint style="success" %}
The REST API expects JSON for the request body.
{% endhint %}

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
