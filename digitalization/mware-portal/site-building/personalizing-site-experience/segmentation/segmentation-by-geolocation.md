# Segmentation By Geolocation

You can configure _Segments_ by geolocation to display different pages dynamically or content based on the user’s country. To display different pages, you can use Experiences, and to display specific content on the page, you can use Collections.

### Creating a Segment by Geolocation <a href="#creating-a-segment-by-geolocation" id="creating-a-segment-by-geolocation"></a>

First, create a _User Segment_ following the steps in Creating User Segments.

1. Once created, drag and drop the _IP Geocoder Country_ session property onto the canvas.
2.  Select one of the four conditions from the comparison drop-down: _equals_, _not equals_, _contains_, _not contains_.

    ![Use the IP Geocoder Country session property to create a Segment by geolocation.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/segmentation/segmentation-by-geolocation/images/01.png)
3. Select a Country from the Country drop-down.
4. Click _Save_.

You can combine multiple properties with conditions. See Configuring Segment Conditions to learn more about combining conditions. Once created, you can use this Segment to create personalized experiences for users. See Content Page Personalization and Personalizing Collections for more information.

### Configuring the Geolocation Database <a href="#configuring-the-geolocation-database" id="configuring-the-geolocation-database"></a>

A geolocation database contains a mapping between IP addresses and their countries of origin. To configure this,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and go to _Control Panel_ → _System Settings_.
2.  Click on _Third Party_ from the _Platform_ section.

    ![You can set the file path to a custom geolocation database.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/segmentation/segmentation-by-geolocation/images/02.png)
3. Click _IP Geocoder Configuration_ from the left menu to set the path to a custom geolocation database.

The file path is empty by default. When empty, it uses the free GeoLite2 database to resolve IP addresses. Optionally, you can set the path to a different geolocation database with the `.mmdb` extension on your server to override the default database.

\
