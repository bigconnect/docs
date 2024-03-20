# Displaying Categories using Display Pages

Using a display page template, you can render web content, documents, and blog entries in a display page. Starting with version 7.4, you can also render categories on a display page. This can be useful if you want to showcase multiple products or services you organized using categories. Because display pages are ideal for showing a catalog of elements in a consistent way, you can use category-based display pages to provide users a consistent experience when browsing your products or services catalog.

### Displaying Categories in Display Pages <a href="#displaying-categories-in-display-pages" id="displaying-categories-in-display-pages"></a>

You render categories in a display page the same way you render other types of display page content:

* Create a display page template for categories and set this template as the default one.
* Select the display page template of your choice when you create or edit the category.

You can also assign a display page template to multiple categories at once.

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Categorization_ → _Categories_.
2. Check the categories you want to update and click _Page_ (![Page](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-page.png)) or, from the top Actions menu (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) menu, select _Assign Display Page Template_.

![Set the display page template as default using the Actions menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/displaying-categories-using-display-pages/images/01.png)

### Example: Using Category-Based Navigation on a Display Page <a href="#example-using-category-based-navigation-on-a-display-page" id="example-using-category-based-navigation-on-a-display-page"></a>

This step-by-step example shows you how to use categories in combination with a display page template to display the services of a fictional insurance company.

1. Create and edit the categories you want to display.
2. Create the display page template.
3. Edit and publish the display page template.

WARNING

The following example shows how to modify a default display page template. To avoid disruptions in your production site, complete this procedure on a test site.

#### Step 1: Creating and Editing the Categories <a href="#step-1-creating-and-editing-the-categories" id="step-1-creating-and-editing-the-categories"></a>

Create the categories if they do not exist yet, and include the information you want to map on the display page. This example creates two new categories with a description and a picture, each category corresponding to a different service.

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Categorization_ → _Categories_.
2. Create a new vocabulary named Vehicle Ownership.
3. Create a new category named Car Insurance. Click _Save_.
4. In the categories application, click _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for your new category and select _Edit_.
5. Click the _Images_ tab and click _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to add an image for the category.
6.  Click the _Details_ tab and add a description of this category.

    ![Edit the category and add a description.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/displaying-categories-using-display-pages/images/02.png)
7. Click _Save_.

#### Step 2: Creating the Display Page Template <a href="#step-2-creating-the-display-page-template" id="step-2-creating-the-display-page-template"></a>

Starting with version 7.4, the display page template includes an option to use categories as the content type.

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Design_ → _Page Templates_.
2. Click _Display Page Templates_ and click _Add_ (![Add Image](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3.  Select the master page template you want to use for this display page template.

    TIP

    Every page or page template requires a master page template. To learn more, read Master Page Templates.
4.  Enter the _Name_ of your display page template and in the Content Type drop-down menu, select _Category_.

    ![Select category as the content type of your new display page template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/displaying-categories-using-display-pages/images/03.png)
5. Click _Save_.

#### Step 3: Edit and publish the Display Page Template <a href="#step-3-edit-and-publish-the-display-page-template" id="step-3-edit-and-publish-the-display-page-template"></a>

After you create the display page template, you can edit the components mappings and layout.

TIP

The display page template editor works like the content page editor. See Adding Elements to Content Pages for more information.

This example shows how to map the category fields using a card fragment.

1. Drag and drop a container fragment to the default drop zone.
2. On top of the container fragment, drag and drop a new card fragment.
3. Click the _Browser_ (![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png)) button and then click the card’s title to open the mapping options.
4. In the Mapping tab, click the _Source_ drop-down menu and select _Specific Content_
5. Click _Item_ and select the category you want to map.
6.  Click the _Field_ drop-down menu and select _Name_ to map your category’s name to the card’s title.

    ![Map the category fields in the display page template using a card fragment.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/displaying-categories-using-display-pages/images/04.png)
7. Repeat the same mapping process for the card description. This list summarizes the card fragment to category field mappings in the example:
   * Title → Name
   * Content → Description
   * Image → Main Image
8. Click _Publish_.
9.  Under the Display Page Templates list, click _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the new display page template and choose _Mark as Default_.

    ![Set the display page template as default using the Actions menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/displaying-categories-using-display-pages/images/05.png)
10. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), navigate to _Categorization_ → _Categories_, and select the vocabulary with the categories from step 1.
11. On any category, click _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _View Display Page_.

    ![Open the display page for one of the categories using Actions option.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/displaying-categories-using-display-pages/images/06.png)

The display page uses your display page template mappings to show the category fields. The display page URL is unique for the selected category.

![The display page template shows the category fields and provides a unique URL.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/displaying-categories-using-display-pages/images/07.png)
