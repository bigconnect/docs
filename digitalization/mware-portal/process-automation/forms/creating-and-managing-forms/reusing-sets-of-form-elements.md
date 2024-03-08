# Reusing Sets of Form Elements

Forms often request the same sets of information, such as name, birthday, and address. Rather than adding these fields for every new form, you can group elements as a set to be reused. Element Sets include the layout and configuration of the fields.

NOTE

* Forms created using an Element Set _do not_ inherit changes that are made later to an Element set.
*   A simpler form of repeatability is achieved by simply dropping one or more form fields on top of another form field to create a Fields Group. Fields Groups are useful within a single form for managing the entire group (moving the group, deleting it, or duplicating it) at once.

    ![Field Grooups offer simpler field reusability and bulk management within a single form.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/reusing-sets-of-form-elements/images/04.png)

### [Creating Element Sets](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/reusing-sets-of-form-elements#creating-element-sets) <a href="#creating-element-sets" id="creating-element-sets"></a>

To create an Element Set:

1. Open the _Product Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) then click the compass icon (![Compass](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-compass.png)) on the _Site Administration_ menu.
2. Select the site where the form will be created.
3. Click _Content & Data_ → _Forms_.
4.  Click the _Element Sets_ tab.

    ![Element Sets Tab](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/reusing-sets-of-form-elements/images/01.png)
5. Click the _Add_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).

The steps to create an Element Set are the same as [creating a form](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/creating-forms). For demonstration, create an Element Set for users to submit their contact information.

1.  Enter the following:

    **Name**: Contact Information **Description**: Contact Information Element Set
2. Click the _Add Element_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3. Drag and drop the desired Text and Numeric Fields and configure the labels accordingly.
4. Click _Save_ when finished.

The Element Set has been created and is ready for use.

TIP

You can use Element Sets to create Element Sets.

### [Using Element Sets](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/reusing-sets-of-form-elements#using-element-sets) <a href="#using-element-sets" id="using-element-sets"></a>

To use an Element Set in a Form:

1. Open the Form Builder.
2. Open the Elements sidebar by clicking the _Add_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3.  Click _Element Sets_.

    ![Using Element Sets.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/reusing-sets-of-form-elements/images/02.png)
4.  Drag the Element Set onto the Form Builder, just like you would any single Form Element.

    ![Drag and drag the element set into the builder.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/reusing-sets-of-form-elements/images/03.png)

    Once the Element Set has been added to a form, the layout and individual elements can be configured and updated without affecting the base Element Set.
5. Click _Publish Form_ button when finished.

\
