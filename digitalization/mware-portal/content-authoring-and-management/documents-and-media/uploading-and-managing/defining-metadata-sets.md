# Defining Metadata Sets

With Documents and Media, you can define reusable groups of metadata fields that can be added to custom Document Types. Once created, these Metadata Sets can be selected while [creating or editing a Document Type](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-document-types) and are added after any fields defined directly in the Document Type form.

Follow these steps to define a new Metadata Set.

1.  Navigate to the Documents and Media application in a Site or Asset Library.

    To access Documents and Media in a Site, open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Content & Data_ → _Documents and Media_.

    ![Click on Documents and Media under Content & Data in the Site Menu.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets/images/01.png)

    To access Documents and Media in an Asset Library, navigate to any Library where it’s enabled and click on _Documents and Media_.

    ![Click on Documents and Media in the Asset Library.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets/images/02.png)
2. Click on the _Metadata Sets_ tab.
3. Click the _Add_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to access the New Metadata Sets form.
4. Enter a _name_ for the Metadata Set.
5.  In the _Builder_ tab, drag and drop the desired fields into the designated drop zone.

    ![Add fields in the Builder tab.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets/images/03.png)

    Each field is highly configurable, with Basic and Advanced options. If desired, you can create field groups by dragging and dropping fields on top of one another. These groups can also be configured.

    ![Configure each field.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets/images/04.png)
6. (Optional) Click on the _Properties_ tab, and enter a _description_.
7. Click on _Save_.

Once saved, the new Metadata Set can be accessed and selected when creating or editing Document Types. See [Defining Document Types](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-document-types) or more information.

### [For 7.3.x and Earlier Versions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets#for-73x-and-earlier-versions) <a href="#for-73x-and-earlier-versions" id="for-73x-and-earlier-versions"></a>

#### [Creating Metadata Sets](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets#creating-metadata-sets) <a href="#creating-metadata-sets" id="creating-metadata-sets"></a>

1. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) then click the compass icon (![Compass](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-compass.png)) on the _Site Administration_ menu.
2. Select the site where the form will be created.
3. Click _Content & Data_ → _Documents and Media_.
4. Click the _Metadata Sets_ tab.
5. Click the _Add_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)). The New Metadata Set form appears.
6. Enter a name.
7. Expand the _Details_ section.
8. Enter a description for the new Metadata Set.
9. Leave the **Parent Metadata** field blank if this is a parent. To select a Metadata Set to extend, click the _Select_ button for _Parent Metadata Set_ and then select the desired Metadata Set.
10. Drag and drop the metadata fields into the editor.

    ![Add your metadata set's fields to the canvas.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets/images/05.png)
11. Click _Save_.

The following metadata fields are available:

| Metadata Field Type | Description                                                                                                      |
| ------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Boolean             | Check the check box                                                                                              |
| Color               | Specify a color.                                                                                                 |
| Date                | Enter a date.                                                                                                    |
| Decimal             | Enter a decimal number up to 2 decimal points.                                                                   |
| Documents and Media | Select a file from a Documents and Media library.                                                                |
| Geolocation         | Specify a location to associate with the document.                                                               |
| HTML                | Use a WYSIWYG editor to enter HTML to enhance the content.                                                       |
| Integer             | Enter an integer                                                                                                 |
| Link to Page        | Link to another page in the same site.                                                                           |
| Number              | Enter a decimal number or an integer.                                                                            |
| Radio               | Displays several clickable options but only one option can be selected at a time.                                |
| Select              | This is just like the radio field except that the options are hidden and must be accessed from a drop-down menu. |
| Text                | Enter a single line of text.                                                                                     |
| Text Box            | Enter multiple lines of text or separate paragraphs.                                                             |
| Web Content         | Select a Web Content article.                                                                                    |

#### [Editing Metadata Sets](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets#editing-metadata-sets) <a href="#editing-metadata-sets" id="editing-metadata-sets"></a>

You can edit your fields to reflect their intended metadata. For example, a text field’s default label is _Text_. If you want to use the text field as a title, for instance, then you should change the field’s label to _Title_. Besides the Label field, you can also configure the default values, variable names, mouse-over tips, widths, and other settings. Some fields have a _Required_ setting for specifying whether users must populate the field. If a field’s _Repeatable_ setting is _Yes_, users can add multiple consecutive instances of the field to the document’s metadata.

1. Click on the field on the canvas. This automatically selects the _Settings_ tab on the left.
2.  To edit a setting value, double-click it in the Settings table and enter the new value.

    ![Editing a field's values](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets/images/06.png)
3. Click _Save_ when finished.

#### [Translating a Metadata Set](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets#translating-a-metadata-set) <a href="#translating-a-metadata-set" id="translating-a-metadata-set"></a>

You can translate each of a metadata set’s field values to any supported locales.

1. Click _Add Translation_.
2.  Click on the language (for example: _French (France)_). Repeat to add another locale.

    ![Adding a translation.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets/images/07.png)
3. Once added to the Available Translations section, click on the flag representing the language.
4.  Modify the desired values.

    ![Translating a field name](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/managing-metadata/defining-metadata-sets/images/08.png)
5. Click _Save_ when finished.

\
