# Creating Structures

Follow these steps to create a structure:

1. Open the _Site Menu_ ( ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Content & Data_ → _Web Content_.
2. Select the _Structures_ tab.
3. Click _Add_ ( ![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) to create a new structure.
4. Enter the title of your new structure.
5. Drag and drop the [available fields](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/creating-structures#structure-fields) onto the structure and [configure their settings](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/configuring-structure-fields#configurable-settings) to create the structure.
6. Optionally, add a _Description_ for your structure.
7. Click _Save_.

![When creating a new structure, you can drag fields from the palette and drop them on the canvas.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-structures/creating-structures/images/01.png)

WARNING

Due to import/export operations, it’s possible to have both a global and a site-scoped structure with the same `structureKey`. If this happens, the site-scoped structure takes precedence, and you can’t access the global structure from that site.

TIP

To discover additional configuration options for web content and structures (e.g., [enable setting a custom structure key](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/whats-new-with-web-content-structures-in-7-4#setting-the-web-content-structure-key)), look in System Settings → Content and Data → Web Content → Administration.

### [Structure Fields](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/creating-structures#structure-fields) <a href="#structure-fields" id="structure-fields"></a>

You can place these fields on a structure:

**Boolean:** A checkbox that stores either `true` (checked) or `false` (unchecked). For example, you can use this field to set a display rule for the structure’s content.

**Color:** A color picker.

**Date:** A pre-formatted text field that displays a date picker. The date format depends on the current locale.

**Date and Time:** A pre-formatted text field that displays a date and time picker. The date and time format depend on the current locale.

**Geolocation:** A map that displays a configured location. Geolocation provides both the current location and directions to another place.

**Grid:** A customized matrix with values that can be personalized for the rows and columns. The user can only select one option for each row.

**Image:** An image selector to include an image from the Documents and Media library or your computer.

NOTE

If you upload an image from your computer, the image is only available for that particular article.

**Link to Page:** A selector to include a link to another page in the same site.

**Multiple Selection:** A list of options to choose from using a checkbox list. This field allows multiple selections.

**Numeric:** A field that accepts integer or decimal numbers based on the selected numeric type. Non-numeric input is not accepted.

**Rich Text:** A WYSIWYG editor to offer more formatting options for writing content.

**Select from List:** A selection of options to choose from using a drop-down list. This field optionally allows multiple selections.

**Separator:** A horizontal line to separate fields.

**Single Selection:** A list of options to choose from using radio buttons. This field allows one selection only.

**Text:** A text field for short entries, such as titles or headings.

**Upload:** A file selector to include a file from the documents and media library or your computer.

**Web Content:** A selector to include an existing web content article.

See [Configuring Structure Fields](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/configuring-structure-fields) for more information on configuring and editing Structure fields.

### [Translating a Structure](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/creating-structures#translating-a-structure) <a href="#translating-a-structure" id="translating-a-structure"></a>

You can add a translation for the structure, either while creating it or later.

![Manage the translations of a Web Content Structure.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-structures/creating-structures/images/02.png)

1. From the Add or Edit Structure screen, open the language selector by clicking the locale (e.g.,`en-us`), and then select _Manage Translations_.
2. Click _Add_ ( ![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) in the Manage Translations modal and choose the language.
3. Click _Done_.
4. Click the default locale again and select the new locale to begin translating.
5. Translate as much of the Structure’s text as you’d like, then click _Save_.

To delete a translation, go back to the Manage Translations modal and click _Trash_ ( ![Trash icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-trash.png) ).

To edit a translation, select the translation using the language selector, make any changes, and click _Save_.

\
