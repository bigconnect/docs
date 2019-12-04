# Auto Enrichment

All data that is ingested or changed is processed by a series of Data Workers. Think of them as jobs that run in a distributed, asynchronous way to analyze each and every piece of data and produce new information from it. Examples can be numerous: extracting names of persons from text, detecting objects in an image, extracting IPs and events from log files, detect sentiment in a piece of text, create new connections based on some attributes etc.

![Data Enrichment in BigConnect](../.gitbook/assets/image%20%28284%29.png)

BigConnect provides a large number of pre-built Data Workers for:

* Text Analytics
* Image analytics
* Video analytics
* Network analytics \(Cyber\)
* OSINT
* Web Crawling
* Social Crawling
* Document Management
* Face Recognition
* etc.

Custom Data Workers can be built easily using the Java language with a few lines of code.

