# Features

## Dynamic Data Model

The core foundation of BigConnect Explorer is the dynamic, semantic data model. It represents the way you store, correlate and query all information and it’s a conceptual data model that expresses information in a factual way: objects, relations and attributes.&#x20;

Typically, the data model is used to provide meaning to the information stored in BigConnect, either at ingestion time or later on, during the life-cycle of the data. It can be defined in the beginning or any time later on, to adapt to any data structure, type or meaning. Any information stored in the system can be mapped to an object (concept), can have some relations and some attributes.&#x20;

Concepts can be anything you can think of: persons, vehicles, bank transactions, phone calls, equipment, companies, locations, events, network packets, log files etc. They are linked to each other using meaningful relations like “works at”, “lives in”, “has friend”, “is brother of”, “sent from IP”, “source file” etc.

Concepts and relations can also have attributes. For a company these can be the company name, formation date, address etc. For a person these can be its first name, last name, birth date, phone number, email address etc. For a “works at” relation we can have the “start date” and “end date” attributes to denote when the person started to work at a company. For a “sent from IP” relation we can have the “timestamp” and the “user” etc.

## Ingestion

BigConnect Explorer can ingest and process any kind of information: databases, office documents, text files, XML files, HTML files, images, audio and video files or streams.

Data ingestion can be manual (using the UI) for small amounts of data or batched, automated using [Data Pipelines](../pipelines-studio/).

Data ingestion is a visual process in BigConect Explorer. Either by using the user interface or Data Pipelines, data ingestion and data model mapping will be done in an easy, visual way. This ensures lower error rates and high process maintainability.

![BigConnect Explorer Data Ingestion](<../../.gitbook/assets/image (158).png>)

## Visual Interface

BigConnect provides an extensible integrated visual console where users can query and analyze the data. The console is collaborative, and all actions are taken inside a space. This means that all data that is added, modified, queried or analyzed can be contained to that space alone. Data changes that happen inside a space can be published to be available for everybody.

### Enterprise Search

[ElasticSearch ](https://www.elastic.co)is used behind the scenes to search through the data model and compute aggregations. This means that search results are delivered near-real-time and search queries can be as complex as you can imagine.

![BigConnect Explorer Search](<../../.gitbook/assets/image (146).png>)

The UI provides a Google-like search experience, enriched with filters, sorting, refinements and spatial queries. The search field supports the Lucene query syntax so you can use special things like wildcard (\*), question mark (?), keyword matching, phrase matching, grouping, boolean queries (AND, OR), fuzzy queries (\~), proximity matching (\~4), range searches (amount:\[500 TO 800]) and boosts (^).&#x20;

Example:

`("foo bar" AND "quic*")^1.5 OR "q?ry" –"john"`

Spatial queries can use the map to pinpoint the center and area of interest:

![BigConnect Spatial Search](<../../.gitbook/assets/image (42).png>)

On top of classic faceted full-text search, more advanced queries can be performed using the [Cypher](broken-reference) query language, using the integrated Cypher query editor with autocomplete features:

`MATCH (nicole:Actor {name: 'Nicole Kidman'})-[:ACTED_IN]->(movie:Movie)` \
`WHERE movie.year < 2015` \
`RETURN movie`

Search queries together with their filters can be saved as Data Sets and can be used all over the place: on the Dashboard for displaying search results and aggregations, for creating Alerts, loading data in the available analysis tools etc.

### Items

Data items like concepts and relations can have a lot of information associated using model-defined attributes. The UI provides a unified way of managing the attributes of an item or a group of items in what we call the **Detail View**.

The Detail view contains all information about a selected object or a group of selected objects. You can change the attributes, attach an image, see assigned tags, navigate through its relations and execute relevant actions for the object (ex. Delete, Add Watch, Export etc.)

![BigConnect Explorer Detail View](<../../.gitbook/assets/image (75).png>)

![BigConnect Explorer Detail View](<../../.gitbook/assets/image (106).png>)

If a group of objects is selected, the Detail view will display statistics about the common attributes of the selected objects.

![BigConnect Explorer Selection Statistics](<../../.gitbook/assets/image (203).png>)

The Detail view is completely extendable to fit any needs using the UI plugin mechanism.

Besides the various objects that can be loaded into the platform, BigConnect Explorer has special support for some common item types: **text**, **image**, **audio** and **video**.

**Text items** have by definition some textual content that can be annotated by selecting pieces of text and marking them as new objects or relations. This is handy if you need to infer new relations from textual content such as mentioned persons, companies, locations, IP addresses or items found in a dictionary, detected by an NLP engine or that satisfy a certain regex pattern.

![BigConnect Explorer Text Annotation](<../../.gitbook/assets/image (22).png>)

**Images** have by definition an associated image. They can be text items as well, so they can have text attached to as well, from an OCR engine for example. In the same way as text items, the image can be annotated to mark relevant existing or objects. This can be done manually or automatically using the Face Recognition or Object Detection enrichments, or even by using Cloud Services like Azure Cloud Computer Vision.

![BigConnect Explorer Image Annotation](<../../.gitbook/assets/image (17) (1).png>)

**Audio items** have by definition an associated audio track. BigConnext Explorer will show a player for these types of items to play the contents. Audio items can be text items as well, so they can have text attached, from a speech-to-text engine for example. Sample plugins are provided for speech-to-text using Google Cloud Speech API.

![BigConnect Explorer Audio Player with Transcriptions](<../../.gitbook/assets/image (82).png>)

**Video items** have by definition associated video content and can also be text items. Since a video can optionally have an audio track, a Data Worker can be used to split and OCR the video frames and the audio track to extract all relevant information from the video, such as text, found objects, people etc.

![BigConnect Explorer Video Player ](<../../.gitbook/assets/image (179).png>)

### Dashboard

The Explorer  includes a dashboard where users are being taken upon login. Various widgets can be added or removed from the dashboard to bring to focus any relevant information.

![BigConnect Explorer Dashboard](<../../.gitbook/assets/image (2) (1).png>)

### Analysis Tools

Data analysis is performed using analysis tools. Out of the box, four tools are provided: Graph, Map, Timeline and Charts but one can create any kind of analysis product using the SDK.

Any number of analysis tool instances can be added inside a space to facilitate analysis from multiple perspectives.

#### Link Analysis

For analyzing the data in its core connected form, we provide an advanced graph tool that can be used to create connections, see how objects are related, find connections between objects and more.

![BigConnext Explorer Link Analysis](<../../.gitbook/assets/image (170).png>)

The graph is interactive, you can drag objects around, create new objects and relations and explore the entire data model.

Objects can be copied and pasted from anywhere onto the graph, external files can be dragged & dropped in to create new objects and various layout and ranking algorithms can be applied on the displayed data such as Page Rank, Betweenness Centrality, Closeness Centrality and Degree Centrality.

Finding how two objects are connected is quite easy as well. Just use the Find Path feature to find out.

![BigConnect Explorer Find Path](<../../.gitbook/assets/image (11) (1).png>)

#### Spatial Analysis

Data with geo-spatial information associated (latitude, longitude) can be displayed on an interactive map with support for layers, annotations and aggregations.

![BigConnect Explorer Spatial Analysis](<../../.gitbook/assets/image (39).png>)

New objects can be created at specific points on the map and will automatically receive location information for future reference. Practical use cases can include defining POIs, locating events, cars, phone calls, network packets etc.

The map can be annotated using points, lines and polygons that can become themselves objects inside the system.

BigConnect Explorer can use publicly-available maps (Bing, OSM) or its own, embedded map server to serve geospatial data for enterprise use-cases. The embedded map server can serve custom OGC-compatible spatial layers throught the WMS or WFS protocols.&#x20;

The embedded map server can be provisioned with custom maps, layers and styles to achieve any desired use-case. By default, administrative and road maps are provided for Romania, and other maps can be added on request.    &#x20;

#### Charts

Quick charts can be created using the Chart tool. A wide selection of charts is available.

![](<../../.gitbook/assets/image (48).png>)

#### Timeline

The timeline is present on every screen and can display the distribution of items over time, if items have date/time attributes. Multiple date/time attributes can be displayed on the timeline and you can zoom-in/zoom-out to specific time periods and select the objects of interest.

![](<../../.gitbook/assets/image (176).png>)

### Administration

The console includes administration capabilities for editing the semantic model, managing users and roles, defining behaviors and ingesting data from various data sources. The admin console is customizable and can be extended by creating 3rd party plugins.

## Behaviors

Multiple saved searches (normal and Cypher queries) can be grouped together to define a behavior. A score is provided for each query and a threshold for the entire behavior. All objects that satisfy the queries and are over the threshold will be compliant with the defined behavior.

## Alerting

Items of interest can be added to a watch list by choosing which attributes/relations to listed for changes. You will be notified once a change has been made to the respective items via email or the dashboard.

## Security

Security is enforced at the UI level and at the data level. At the UI level, security is achieved by users, roles and privileges that control what users can do in the console, both at a global level and at the space level.

At the data level, fine-grained access can be controlled up to the attribute level. Labels can be applied to items, attributes and relations to restrict access to sensitive information. Only users with the required authorizations can see the data items that have security labels associated. The authorizations and security labels can be user-defined to provide maximum flexibility and extensibility.

## Extensibility

BigConnect Explorer is designed to be highly extensible and customizable at all levels using plugins, both for the backend and the front-end.



