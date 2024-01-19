# Custom maps

By default, Answers uses OpenStreetMaps for map visualizations, but there are a few customization options.

## Finding Map Settings

To find the map settings:

1. At the top right of the screen, click **gear** icon > **Admin settings**.
2. Select `Maps` from the navigation on the left.

## Map Tile Server

As mentioned above, Answers uses the OpenStreetMaps tile server. However, if your organization requires a different look or level of detail in your map visualizations, you can change the map tile server by adding the map tile server path to the first field on the page. The path must either be a URL that starts with “http://” or “https://” or a relative path to a local file in the JVM’s classpath.

## Custom Region Maps

If you need a map that focuses on a specific region, you may want to upload a custom GeoJSON map. We recommend a GeoJSON file size of 5MB or less.

To get started, click the `Add a map` button. A modal will appear, asking for:

* The name of the map
* The URL of the GeoJSON file
* The JSON property that Answers should use as your region’s identifier (a distinct way of identifying this specific region map)
* The JSON property that Answers should use as the display name (how the name of the region map appears in the user interface)

If you do not currently have a GeoJSON file, there are many tools available to create one, such as MapShaper or GeoJSON.io. If you’re just interested in reading more about GeoJSON, we recommend that you start here.
