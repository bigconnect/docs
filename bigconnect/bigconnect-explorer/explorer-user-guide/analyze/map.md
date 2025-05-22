# Map

Data with geo-spatial information associated (latitude, longitude) can be displayed on an interactive map with support for layers, annotations and aggregations.

![BigConnect Explorer Spatial Analysis](<../../../../.gitbook/assets/image (39).png>)

New objects can be created at specific points on the map and will automatically receive location information for future reference. Practical use cases can include defining POIs, locating events, cars, phone calls, network packets etc.

The map can be annotated using points, lines and polygons that can become themselves objects inside the system.

BigConnect Explorer can use publicly-available maps (Bing, OSM) or its own, embedded map server to serve geospatial data for enterprise use-cases. The embedded map server can serve custom OGC-compatible spatial layers throught the WMS or WFS protocols.&#x20;

The embedded map server can be provisioned with custom maps, layers and styles to achieve any desired use-case. By default, administrative and road maps are provided for Romania, and other maps can be added on request.    &#x20;

### Navigating the map

You can use various mouse gestures to navigate the map:

* Pan
* Zoom (mouse scroll)
* Select

Graph entities can be selected and filtered in two ways:

* selecting a number of items and then filtering them based on common property values
* using the timeline

<figure><img src="../../../../.gitbook/assets/image (1086).png" alt=""><figcaption></figcaption></figure>

### Creating objects

Users can create new entities directly on the map:

* Right-click on the map and choose **Create...**
* Choose the type of object: Point, Line, Polygon
* Draw the object
* Set the concept type and properties
