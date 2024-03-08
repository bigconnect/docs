# Defining Categories and Vocabularies for Content

Using Categories, you can group similar assets and organize your content. Along with [Tags](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/tagging-content-and-managing-tags), Categories help people find relevant information through search or navigation. For more information about Categories in Liferay DXP and the different usage scenarios, see [Organizing Content with Categories and Tags](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/organizing-content-with-categories-and-tags).

You group categories using [vocabularies](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#defining-vocabularies), and you create a hierarchy of categories using [subcategories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#creating-subcategories). You can use the vocabularies and categories at the Global Site in any other Site, but you can only create or edit Global Vocabularies at the Global Site.

NOTE

Only Site administrators can manage Categories and Vocabularies in the Categories panel.

### [Defining Vocabularies](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#defining-vocabularies) <a href="#defining-vocabularies" id="defining-vocabularies"></a>

1. Go to the Site where you want to create the Vocabulary.
2. Open the Product Menu and, under the Site Menu, go to _Categorization_ → _Categories_.
3. Click _New Vocabulary_ (![New Vocabulary](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png)).
4. Enter a _Name_ for the Vocabulary and, optionally, a _Description_.
5.  Complete the rest of the Category options:

    **Allow Multiple Categories:** Enable this option to use more than one Category from the Vocabulary to categorize the asset. When disabled, you can use only one Category from the Vocabulary.

    **Visibility:** See [Vocabularies Visibility](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/organizing-content-with-categories-and-tags#vocabulary-visibility) for more information.

    IMPORTANT

    You cannot change the Visibility after saving the Vocabulary.

    **Associated Asset Type:** Select the allowed asset types for applying a Category from this Vocabulary. Set _Required_ to _Yes_ if you want this type of asset always to have a category.

    TIP

    Click the _Add_ button under the _Associated Asset Type_ section to include more than one asset type.

    ![Define the category options for multiple asset types.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content/images/02.png)
6. Click _Save_.
7.  To edit an existing vocabulary, click the _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) menu next to the Vocabulary name and select _Edit_.

    ![Edit an existing Vocabulary using the Actions menu.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content/images/08.png)

### [Defining Categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#defining-categories) <a href="#defining-categories" id="defining-categories"></a>

You create and manage Categories in the Site Menu → _Categorization_ → _Categories_ panel. You can also create new categories from the Categorization section in the Content Editor, using the Select button for each Vocabulary.

IMPORTANT

You can create a maximum of 150 Categories in one Vocabulary.

![Add new Categories from the Categorization section in the Content Editor.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content/images/04.png)

#### [Creating Categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#creating-categories) <a href="#creating-categories" id="creating-categories"></a>

1. Open the Product Menu and, under the Site Menu, go to _Categorization_ → _Categories_.
2. Under the _Vocabularies_ list, click the Vocabulary to add the new Category.
3. Click _Add Category_ (![Add Category](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
4. Enter a _Name_ for the Category and, optionally, a _Description_.
5.  Under the Display Page section, choose a specific Display Page for the Category or use the default one.

    NOTE

    Display Pages for Categories are available starting with Liferay DXP 7.4. For more information, read [Displaying Categories Using Display Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/displaying-categories-using-display-pages).
6. Click _Save_ or _Save and Add a New One_ to add more categories.
7. To edit an existing category, click the _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) menu next to the category’s name.

#### [Creating Subcategories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#creating-subcategories) <a href="#creating-subcategories" id="creating-subcategories"></a>

You can create a nested hierarchy of Categories and Subcategories:

1. Open the Product Menu and, under the Site Menu, go to _Categorization_ → _Categories_.
2. Under the _Vocabularies_ list, click the Vocabulary you want to modify.
3. Click the category name where you want to create the new subcategory.
4. In the _Add New Subcategory_ screen, enter the subcategory’s _Name_ and, optionally, a _Description_.
5. Click _Save_ or _Save and Add a New One_ to add more subcategories.

#### [Moving Categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#moving-categories) <a href="#moving-categories" id="moving-categories"></a>

You can move a category to a different Vocabulary, or as a subcategory in the same Vocabulary.

1. Open the Product Menu and, under the Site Menu, go to _Categorization_ → _Categories_.
2.  Click the _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) menu next to the category you want to move and select _Move_.

    ![Use the Move option to organize Categories.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content/images/03.png)
3.  Select the _Vocabulary_ and _Category_ where you want to move the Category.

    NOTE

    You can only move a Category into a Vocabulary of the same [Visibility type](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/organizing-content-with-categories-and-tags#vocabulary-visibility).

#### [Editing Additional Category Properties](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#editing-additional-category-properties) <a href="#editing-additional-category-properties" id="editing-additional-category-properties"></a>

After creating a Category, you can define these additional options:

* Details
* Images
* Friendly URL
* Properties

To access these options, click the _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) menu next to the Category you want to modify and select _Edit_.

![Edit the Category to define additional options.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content/images/10.png)

#### [Changing the Maximum Number of Categories per Vocabulary](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#changing-the-maximum-number-of-categories-per-vocabulary) <a href="#changing-the-maximum-number-of-categories-per-vocabulary" id="changing-the-maximum-number-of-categories-per-vocabulary"></a>

You can create a maximum of 150 Categories for each Vocabulary. But a very large number of categories may impact performance and load times. To set a lower maximum number,

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Instance Settings_ → _Assets_.
2.  Click _Categorization_ in the left navigation. Input a new maximum value (e.g. `2`) and click _Save_.

    ![Set a new maximum value of Categories.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content/images/11.png)
3.  A message now appears when you reach the maximum value, and the user cannot create additional categories.

    ![A message appears when you reach the maximum value.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content/images/12.png)
