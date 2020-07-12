# Images

The following data workers are available for extracting information from images:

* Metadata
* OCR
* Object Detection
* Image Captioning
* Face Recognition

## Metadata extractor

This data worker receives an image and extracts all available [EXIF](https://en.wikipedia.org/wiki/Exif) metadata:  

* Camera manufacturer
* Camera model
* Orientation
* Date and time
* GeoLocation
* Author
* etc.

## OCR

This data worker extracts all available text from the provided image.

## Object Detector

This data worker uses neural models to perform object detection inside an image. The following underlying extractors are available:

* Pre-trained models with up to 80 object classes
* Cloud APIs \(Google, Azure\)
* Custom models

Custom models can be trained easily using the provided [Data Labeling]() tool.

## Image Captioning

This data worker uses neural models to describe an image. The following underlying extractors are available:

* Pre-trained model
* Cloud APIs \(Google, Azure\)
* Custom models

Custom models can be trained easily using the provided [Data Labeling]() tool.

## Face Recognition

This data worker performs face detection and recognition on images using a custom pre-trained DNN model. Integration with other Facial Reconition technology providers is available:

* Cloud APIs \(Azure, Google\)
* [NEC](https://www.nec.com/en/global/solutions/biometrics/face/index.html?)
* [3divi](https://face.3divi.com/)

