# Geolocating Assets

Geolocation adds the geographic coordinates where an asset was created as metadata to your assets. You can add geolocation metadata to your web content, Data Lists, and Documents & Media. This feature is provided for you out-of-the-box. However, you must first enable it in your assets in order to use it.

Let’s examine how you can enable geolocation in your web content.

### Geolocating Web Content <a href="#geolocating-web-content" id="geolocating-web-content"></a>

To use geolocation in your web content, you must create a structure and template that includes a Geolocation field.

![Figure 1: Add a geolocation field to your structure to enable geolocation in your web content.](https://liferay-support.zendesk.com/hc/article\_attachments/360030160351/geo-structure.png)



1. Create a structure with a Geolocation field like in the image above.
2. Create a new template and select the structure you just created with the geolocation field.
3. Scroll down to the _Script_ heading and locate the _Fields_ section. Here are _Content_ and _Geolocation_ snippets.
4. Click on the snippets to add them to the template and _Save_.

![Figure 2: Add the Content and Geolocation snippets to create your web content template quickly.](https://liferay-support.zendesk.com/hc/article\_attachments/360030160431/web-content-geolocation-template.png)

To set your location for the web content, you can share your location with the browser, type a specific address into the address bar on the map, or even drag the indicator and drop it in any point in the map and the address is automatically updated to reflect the new point. Once the web content is saved, the location is added as metadata to the web content.

![Figure 3: You can enter your location in the address bar, move the indicator to a location, or share your location with the browser.](https://liferay-support.zendesk.com/hc/article\_attachments/360030191232/web-content-geo-create.png)

Figure 3: You can enter your location in the address bar, move the indicator to a location, or share your location with the browser.

Note: Depending on your browser settings, you may need to configure it to share your location.

![Figure 4: Make sure your browser is configured to share your location.](https://liferay-support.zendesk.com/hc/article\_attachments/360030191172/share-location-dialog.png)

### Geolocating Data Lists <a href="#geolocating-data-lists" id="geolocating-data-lists"></a>

To use geolocation in your dynamic data lists, you must first create a data definition that includes a geolocation field.

1. Open the Product Menu and navigate to _Content & Data_ → _Dynamic Data Lists_.
2. Click the _Options_ menu and select _Manage Data Definitions_.
3. Click the _Add_ button to create a new data definition.
4. Enter a name, optional description, and parent data definition if you have one.
5. Scroll down and add a _Geolocation_ field to the data definition, along with any other fields you wish to add and _Save_.
6. Go back to the Dynamic Data Lists screen and click the _Add_ button (![add](https://liferay-support.zendesk.com/hc/article\_attachments/360030160391/icon-add.png)) to create a new list.
7. Enter a name and optional description.
8. Finally, click the _Select_ button and choose the newly created data definition.

Now that your data list is complete, you can use the Data List Display portlet to display it.

### Geolocating Documents and Media <a href="#geolocating-documents-and-media" id="geolocating-documents-and-media"></a>

To enable geolocation in Documents and Media, you must first create a document type that includes geolocation metadata. You can add geolocation metadata as part of a Metadata Set or as part of the new document type. To add geolocation metadata as part of a Metadata Set:

1. Open the Product Menu and navigate to _Content & Data_ → _Documents and Media_. Open the _Options_ menu, and select _Metadata Sets_.
2. Click the _Add_ (![add](https://liferay-support.zendesk.com/hc/article\_attachments/360030160391/icon-add.png)) button and enter a name, optional description, and Parent Metadata Set if you have one.
3. Scroll down and add a _Geolocation_ field, along with any additional fields you wish to have, and _Save_.

To create the new document type with geolocation:

1. Navigate to _Documents and Media_, open the _Options_ menu and select _Document Types_.
2. Click the _Add_ button (![add](https://liferay-support.zendesk.com/hc/article\_attachments/360030160391/icon-add.png)) and enter a name and optional description.
3. Scroll down to the Main Metadata Fields heading and add a _Geolocation_ field along with any other fields you wish to have for the document type.
4. If you are using a Metadata Set, scroll down to the Additional Metadata Fields heading, click the _Select Metadata Set_ button.
5. Choose your Metadata Set with the geolocation metadata and _Save_.
6. Navigate back to the _Documents and Media_ screen and click the _Add_ button (![add](https://liferay-support.zendesk.com/hc/article\_attachments/360030160391/icon-add.png)) and select your newly created document type.
7. Fill out the information for the document, and just as with the web content, your location is automatically obtained from the browser and added to your document.

Once your assets are geolocation enabled, you can use the Asset Publisher to display the location of the assets on a map, using the map display template. Check out the Configuring Display Settings section to learn more.

![Figure 5: The Asset Publisher can display your geolocated assets on a map.](https://liferay-support.zendesk.com/hc/article\_attachments/360030160411/geo-map.png)

