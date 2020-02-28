# Data Enrichment

All data that is ingested or changed is processed by a series of Data Workers. Think of them as jobs that run in a distributed, asynchronous way to analyze each and every piece of data and produce new information from it. Examples can be numerous: extracting names of persons from text, detecting objects in an image, extracting IPs and events from log files, detect sentiment in a piece of text, create new connections based on some attributes etc.

![](../../.gitbook/assets/image%20%28526%29.png)

{% hint style="info" %}
Enrichment is done based on the type of information: text, image, video, audio, log files, network packets etc.
{% endhint %}

{% hint style="success" %}
**Data workers work in tandem and trigger whenever information is new or changed in the system. This means that they will be run one after another based on what the previous extractor did.**
{% endhint %}

**Example**:

For an image file, the following sequence is applied:

* The **MimeType Extractor** detectes the mime type of the file and sets it on the new image object
* The **Image Metadta Extractor** sees there is a new object with image mime-type and triggers itself to process the image, extract metadata and store it on the image object
* The **Image OCR Extractor** sees there is a new object with image mime-type and triggers itself to extract the text from the image and stores the text on the image with the `text/plain` mimetype
* The **Text Entity Extractor** worker sees there is a new attribute on an object with text/plain mime-type and performs entity extraction on the stored text
* The **Text Sentiment Analysis Extractor** worker sees there is a new attribute on an object with text/plain mime-type and performs sentiment analysis on the stored text, storing the sentiment value on the original object.
* .....

BigConnect provides a large number of pre-built Data Workers detailed in the next sub-sections.

