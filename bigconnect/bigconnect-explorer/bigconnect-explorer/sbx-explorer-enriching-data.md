# Enriching Data

Enriching is the process of deriving additional information about an existing item:

* For a picture or video this means OCR, Object Detection, Face Recognition, Number Plate Recognition, Age/Gender detection etc. to derive additional information about the picture like the text it contains, what objects it has in it, what persons, cars etc.
* For a textual document this means Named Entity Recognition, Sentiment Analysis, Classification, Relationship Extraction etc.
* For an audio file this means Speech-to-Text, Named Entity Recognition, Sentiment Analysis, Classification, Relationship Extraction etc.
* For an IP address it can mean things like whether it's blacklisted, registered in an international fraud registry etc.
* For an address it can mean to geocode the address and extract the latitude and longitude etc.

Enrichments can be applied both manually and automatically using **Data Workers**. A **Data Worker** is like an 'agent'  that looks at every piece of information in the system and does some work knowing the context of a piece of information.&#x20;

BigConnect supplies some pre-built Data Workers for various enrichments like:

* &#x20;OCR
* Object Tagging
* Face Recognition
* Speech-to-Text
* Named Entity Recognition
* Sentiment Analysis
* Classification
* Relationship Extraction
* Summarization
* Similarity

You can also easily build custom ones.

Let's see how the process works for an image and a text document. First, download the following files:

* [https://github.com/bigconnect/demos/raw/master/explorer/basics/coronavirus.txt](https://github.com/bigconnect/demos/raw/master/explorer/basics/coronavirus.txt)
* [https://github.com/bigconnect/demos/raw/master/explorer/basics/panga.jpg](https://github.com/bigconnect/demos/raw/master/explorer/basics/panga.jpg)

Login to BigConnect Explorer using the default username **admin** and password **admin.** If this is the first time you login, you will be taken to the default dashboard created for you.

![](<../../../.gitbook/assets/image (595).png>)

Go to **Analyze** in the upper menu bar.

![](<../../../.gitbook/assets/image (458).png>)

Click on the **Graph** card (or the **New..**. button and choose **Graph**) and an empty graph will be created for you:

![](<../../../.gitbook/assets/image (616).png>)

Drag & drop the two downloaded files on the empty graph.

{% hint style="info" %}
You can also click on the UPLOAD card and select the files using the upload dialog
{% endhint %}

A popup will be shown to ask you how you want to load the files into the system. Just leave the defaults and click **Import**

![](<../../../.gitbook/assets/image (508).png>)

The system will load and process the files. After a short while you should see four items on your graph:

![](<../../../.gitbook/assets/image (549).png>)

### Text items

Select the item that contains text information. You can see that some words in the text are underlined and that the item has a Negative Sentiment.

![](<../../../.gitbook/assets/image (559).png>)

These are automated enrichments that were applied by the system. The underlined words are Named Entities that the system detected and it marked them for you to "**resolve**".

{% hint style="info" %}
"**Resolving**" and entity means to associate a piece of text or image (a word in our case) to a new object in the system.&#x20;
{% endhint %}

Click on the word **Austria** and you will see a popup where you can resolve the word to a new object:

![](<../../../.gitbook/assets/image (449).png>)

The system found that Austria is a probable Location object. Click on **Resolve** to proceed.

![](<../../../.gitbook/assets/image (577).png>)

The popup above allows you to create a new object with concept **Location** and name **Austria**. This is what BigConnect suggests you should do. If the Austria Location object exists in the system, then the popup  will tell you to associate the word with the existing object instead of creating a new one.

Click "**Resolve**" to proceed.

{% hint style="info" %}
When you "resolve" a word or piece of image, the system will create a relationship of type **hasEntity** between the item that contains the word to the target resolved object.
{% endhint %}

![](<../../../.gitbook/assets/image (506) (1).png>)

You can see that the word Austria is now bolded. This means that this word is resolved and you can click on it to see details about the target object.

{% hint style="info" %}
**You can also select a word or piece of text to resolve it to an object.**&#x20;
{% endhint %}

Now select the text item on the graph, right click on it and choose **Add Related** from the context menu. Click again **Add Related** and the Austria Location will be added to the graph automatically.

![](<../../../.gitbook/assets/image (442).png>)

### Image Items

For images things work in a similar way. Click on the image item on the graph:

![](<../../../.gitbook/assets/image (607).png>)

You can see that the image object has 3 blue boxes: **Animal, Hat, Sunglasses**. These are the objects the system automatically identified in the image.

To manually annotate an image, you can draw a rectangle with your mouse on the image. Place the mouse on the image, click and then draw the rectangle. Once you finished, the system will ask you to "resolve" the piece of the image to an object, in a similar way to text items.

![](<../../../.gitbook/assets/image (542).png>)

Enter the name of the new entity, its concept and click "Resolve as New".

{% hint style="info" %}
If you want to associate the piece of the image with an existing object, just type the name of the object instead of _Yogi Bear_ and the system will do a search for you to bring you existing entities with the provided keyword.
{% endhint %}

