# Using Fragments to Build Forms

Liferay includes form fragments for building object forms in content pages. To build a form, add a form container to a content page or page template and map it to a published object. The container automatically populates fragments mapped to each object field. You can organize, remove, or add fragments manually. Once published, end users can create object entries using the form.

To build forms in content pages,

* Place form fragments only in a mapped form container.
* Map every form fragment in the form to an object field to prevent data loss.
* Include fragments in the form container for all mandatory object fields.
* Make mandatory form fragments visible.
* Include a visible submit button in the form container.

NOTE

When mapping an object to a Form Container fragment, a submit button is generated. You can map its text using [mapping settings](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference#mapping-settings).

To create a button with [link settings](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference#link-settings) for redirection, add `type="button"` to the button fragment.

By default, Liferay includes the [Form Components](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/default-fragments-reference#form-components) fragment set, but you can create your own. See [Creating Form Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/creating-form-fragments) for more information.

In addition, form fragments can only be used with [custom objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects). Therefore, Liferay hides the Form Components fragment set until you have at least one published object. Once published, users with access to the object can view and use the Form Components fragments.

### [Creating a Form](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/using-fragments-to-build-forms#creating-a-form) <a href="#creating-a-form" id="creating-a-form"></a>

1.  Begin editing a content page or page template.

    Display page templates do not support form fragments.
2. (Optional) Design a layout using fragments. See [Building Responsive Layouts with Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/building-a-responsive-site/building-responsive-layouts-with-the-grid-fragment) for more information.
3.  Add the _Form Container_ fragment to the page and map it to an object.

    The container automatically generates a Submit button and fragments for all object fields. They appear in alphabetical order. Mandatory fields are marked accordingly.

    ![Add the form container and map it to an object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/using-fragments-to-build-forms/images/01.png)
4. Remove non-mandatory fields that you don’t want in the form.
5. Drag and drop form fragments to reorganize them. By default, form elements are organized alphabetically.
6.  (Optional) Add basic fragments to the form container. You can use basic components (e.g., grids, separators, paragraphs, images) to provide structure and style to your form container.

    ![Add basic fragments to the form container and reorganize your page elements to create the desired structure.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/using-fragments-to-build-forms/images/02.png)
7.  Configure the fragments. Form fragments include both [standard](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments) and [unique](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/general-settings-reference#form-fragment-options) configuration options.

    NOTE

    Form fragment configuration options become available after mapping the fragment to an object field.

    ![Configure the form fragments.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/using-fragments-to-build-forms/images/03.png)
8.  [Beta Feature](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#beta-feature-flags) (Optional) If draft mode is enabled for the object, configure the Submitted Entry Status for the form’s submit button: _Approved_ or _Draft_.

    IMPORTANT

    When using form fragments to update entries in a display page, you cannot change an entry’s status from approved to draft.

    ![Configure the Submitted Entry Status for the form's submit button.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/using-fragments-to-build-forms/images/04.png)
9. Click _Publish_ when finished.

### [Supported Field Types for Form Components](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/using-fragments-to-build-forms#supported-field-types-for-form-components) <a href="#supported-field-types-for-form-components" id="supported-field-types-for-form-components"></a>

Each form fragment can support one or more object field types. The default form components support these fields.

| Form Fragment    | Object Field Types                                |
| ---------------- | ------------------------------------------------- |
| CAPTCHA          | N/A                                               |
| Checkbox         | Boolean                                           |
| Date             | Date                                              |
| File Upload      | Attachment                                        |
| Multiselect List | Multiple Picklist                                 |
| Numeric Input    | Integer, Long Integer, Decimal, Precision Decimal |
| Rich Text        | Rich Text                                         |
| Select from List | Relationship, Picklist                            |
| Submit Button    | N/A                                               |
| Text Input       | Text, Long Text                                   |
| Textarea         | Text, Long Text                                   |

TIP

If categorization is enabled for the object, you can use the Tags and Categories form fragments to add metadata to object entries.

\
