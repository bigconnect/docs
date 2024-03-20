# Exporting and Importing Page Templates

Sometimes it’s easier to edit templates in a code editor. Once you’ve created a page template, you can export it, edit it locally in the editor of your choice, and then import the template back into the portal. You can import the template into any site on the system.

For more general information about the export/import framework, see Exporting/Importing Site Pages and Content.

NOTE

Page Template Collections are called Page Template Sets in the portal UI.

### Exporting a Page Template <a href="#exporting-a-page-template" id="exporting-a-page-template"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Design_ → _Page Templates_.
2. Select the _Page Templates_ tab.
3. Select the page template set that contains the desired template.
4.  Click _Actions_ (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired content page template and select _Export_.

    ![Click Export to export your page template as a ZIP file](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/adding-pages/exporting-and-importing-page-templates/images/01.png)

The content page template downloads as a ZIP file to your local machine.

TIP

You can export more than one content page template at once.

The last published version of the page template is always exported. If the page template has never been published, it cannot be exported.

The exported ZIP file contains these files:

* `page-template-collection.json`: Includes the name of the set where the page template is saved and any other metadata.
* An optional thumbnail file
* `page-template.json`: Contains the page template name and any other metadata.
* `page-definition.json`: specifies the structure and content of the Page Template.

The ZIP file may contain different types of page templates as well, like `display-page-template.json`, `master-page.json`, `page-template-collection.json` and `page-template.json`.

### Importing a Page Template <a href="#importing-a-page-template" id="importing-a-page-template"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Design_ → _Page Templates_.
2.  Click _Options_ (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) in the Application Bar and select _Import_.

    ![The import function is located in the options menu in the Application Bar.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/adding-pages/exporting-and-importing-page-templates/images/02.png)
3.  Select the desired page template file.

    Click _Replace File_ if you want to change it.
4.  Click _Import_.

    If you import an existing page template, see Importing an Existing Page Template for more information.

    To cancel the operation and go back to the page templates page, click _Cancel_.

    ![Page templates are imported as ZIP files.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/adding-pages/exporting-and-importing-page-templates/images/03.png)
5. Once the importing process is complete, you are redirected to an [import outcome page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/exporting-and-importing-page-templates#import-outcome-page).
6. Click _Upload Another File_ to import another file or _Done_ to go back to the page templates page.

IMPORTANT

To transfer page templates between DXP environments, use the LAR export and import method.

#### Importing an Existing Page Template <a href="#importing-an-existing-page-template" id="importing-an-existing-page-template"></a>

If you import a page template and items from the ZIP file already exist, you can select one of three options:

**Do Not Import Existing Items**: Only new items are imported.

**Overwrite Existing Items**: Duplicated existing items are overwritten.

**Keep Both**: A numbered copy of existing items is imported.

Once the importing process is complete, you are redirected to an import outcome page.

![Choose one of three options when you import a duplicated page template](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/adding-pages/exporting-and-importing-page-templates/images/04.png)

#### Import Outcome Page <a href="#import-outcome-page" id="import-outcome-page"></a>

When users import files, they are redirected to a new page with details about the importing process.

On the import outcome page, users can see the successfully imported entries, warnings (successful imports that could cause malfunctions), and errors (entries that weren’t imported).

![The import outcome page shows warnings and errors related to the importing process](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/adding-pages/exporting-and-importing-page-templates/images/05.png)
