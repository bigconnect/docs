# Geolocating Assets

Geolocation adds the geographic coordinates where an asset was created as metadata to an asset. This feature is provided as an optional field which can be included in a Web Content structure, Dynamic Data List field, or a Documents and Media Document Type or a Metadata Set.

### Geolocating Web Content <a href="#geolocating-web-content" id="geolocating-web-content"></a>

To use geolocation in your web content, you must create a [structure and template](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/understanding-web-content-structures) that includes a Geolocation field.

![Figure 1: Add a geolocation field to your structure to enable geolocation in your web content.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/01.png)

1. Create a structure with a Geolocation field like in the image above.
2. Create a new template and select the structure you just created with the geolocation field.
3. Scroll down to the _Script_ heading and locate the _Fields_ section. Here are _Content_ and _Geolocation_ snippets.
4.  Click on the snippets to add them to the template.

    ![Figure 2: Add the Content and Geolocation snippets to create your web content template quickly.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/02.png)
5. Click _Save_ when finished.

To set your location for the web content, you can share your location with the browser, type a specific address into the address bar on the map, or even drag the indicator and drop it in any point in the map and the address is automatically updated to reflect the new point. Once the web content is saved, the location is added as metadata to the web content.

![You can enter your location in the address bar, move the indicator to a location, or share your location with the browser.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/15.png)

NOTE

Depending on your browser settings, you may need to configure it to share your location.

### Geolocating Data Lists <a href="#geolocating-data-lists" id="geolocating-data-lists"></a>

To use geolocation in your Dynamic Data Lists, you must first create a data definition that includes a geolocation field.

#### Creating a New Data List Definition <a href="#creating-a-new-data-list-definition" id="creating-a-new-data-list-definition"></a>

1. Open the Product Menu and navigate to your site.
2.  Click _Content & Data_ → _Dynamic Data Lists_.

    ![Navigate to the DDL in the Site Administration.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/03.png)
3.  Click the _Options_ (![Options menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) menu and select _Manage Data Definitions_.

    ![Navigate to Manage Data Definitions to create a new Definition.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/04.png)
4. Click the _Add_ (![add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) button to create a new data definition.
5. Enter a name, optional description, and parent data definition if you have one.
6.  Scroll down and add a _Geolocation_ field to the data definition, along with any other fields you wish to add.

    ![Create a new Definition.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/05.png)
7. Click _Save_ when finished.

Now that the Data Definition has been created, create a new Data List using this definition.

#### Creating a New Data List <a href="#creating-a-new-data-list" id="creating-a-new-data-list"></a>

To create a data list that uses the new data definition:

1. Navigate back to the Dynamic Data Lists screen and click the _Add_ button (![add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to create a new list.
2. Enter a name and optional description.
3. Click the _Select_ button and choose the newly created data definition.
4.  Select whether this form has a Workflow.

    ![Create a new Data List.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/06.png)
5. Click _Save_ when finished.

Now that your data list is complete, you can use the [Data List Display portlet](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/dynamic-data-lists/getting-started-with-dynamic-data-lists) to display it.

### Geolocating Documents and Media <a href="#geolocating-documents-and-media" id="geolocating-documents-and-media"></a>

To enable geolocation in Documents and Media, you must first create a [Document Type](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-document-types) that includes geolocation metadata. Alternately, you can add geolocation metadata as part of a [Metadata Set](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets). Once you have created that Metadata Set, it can be included as part of a Document Type.

#### Adding Geolocation in a Metadata Set <a href="#adding-geolocation-in-a-metadata-set" id="adding-geolocation-in-a-metadata-set"></a>

Follow the steps below:

1. Open the Product Menu and navigate to your site.
2. Click _Content & Data_ → _Documents and Media_.
3.  Click the _Metadata Sets_ tab.

    ![Navigate to the Metadata Sets tab in DM](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/07.png)
4. Click the _Add_ (![add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) button and enter a name, optional description, and Parent Metadata Set if you have one.
5.  Scroll down and add a _Geolocation_ field, along with any additional fields you wish to have.

    ![Drag and drop the Geolocation field along with other fields to create your Metadata Set.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/08.png)
6. Click _Save_ when finished.

#### Adding a Geolocation Field in a Document Type <a href="#adding-a-geolocation-field-in-a-document-type" id="adding-a-geolocation-field-in-a-document-type"></a>

Follow the steps below:

1. Open the Product Menu and navigate to your site.
2. Click _Content & Data_ → _Documents and Media_.
3.  Click the _Document Types_ tab.

    ![Navigate to the Document Types tab in DM.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/09.png)
4. Click the _Add_ button (![add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and enter a name and optional description.
5.  Scroll down to the Main Metadata Fields heading and add a _Geolocation_ field along with any other fields you wish to have for the document type.

    ![Navigate to the Document Types tab in DM.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/10.png)
6. If you are using a Metadata Set, scroll down to the Additional Metadata Fields heading, click the _Select Metadata Set_ button.
7. Choose your Metadata Set with the geolocation metadata.
8. Click _Save_.

#### Adding Geolocation to a Document <a href="#adding-geolocation-to-a-document" id="adding-geolocation-to-a-document"></a>

Follow the steps below:

1.  Navigate to the _Documents and Media_ tab.

    ![Navigate to the Documents and Media tab in DM.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/11.png)
2.  Click the _Add_ button (![add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select your newly created document type.

    ![Navigate to the Documents and Media tab in DM.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/12.png)
3.  Fill out the information for the document and your location is automatically obtained from the browser and added to your document.

    ![Navigate to the Documents and Media tab in DM.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/13.png)
4. Click _Publish_ when finished.

Once your assets are geolocation-enabled, the Asset Publisher can display the location of the assets on a map, using the map display template. Check out Configuring the Asset Publisher Display Settings to learn more.

![The Asset Publisher can display your geolocated assets on a map.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/geolocating-assets/images/14.png)
