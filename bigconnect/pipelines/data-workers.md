# BigGraph Data Workers

These are Data Workers implemented using Data Pipelines flows. Out-of-the-box, the following Data Workers are available:

* DW\_Age\_Detector
* DW\_Category\_Detection
* DW\_Emotion\_Detector
* DW\_Entity\_Extractor
* DW\_Face\_Detector
* DW\_Face\_Features\_Detector
* DW\_Gender\_Detector
* DW\_Image\_Captioning
* DW\_Image\_Metadata\_Extractor
* DW\_Location\_Resolver
* DW\_Object\_Detector
* DW\_OCR\_Detector
* DW\_Rabbit\_consumer
* DW\_Sentiment\_Analysis
* DW\_Speech\_to\_Text

They are triggered automatically whenever a changed in made in BigGraph and will execute only if their isHandled condition is true.
