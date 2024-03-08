# Personalizing Custom Forms Using Form Fragments

Form fragments auto-generate forms based on custom objects. If Liferay’s out-of-the-box form fragments don’t satisfy your use case, you can create custom form fragments and alter the style of your forms by [mapping your custom form fragment to a field type](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/personalizing-custom-forms-using-form-fragments#map-custom-form-fragments-to-form-fields).

The custom form fragments use HTML, CSS, and JavaScript code to create an element with customized style and functionality. This way, you can customize your forms using an existing design system and/or components library. See [Creating Form Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/creating-form-fragments) for more information.

IMPORTANT

Form fragments can only be used with custom objects. Liferay hides the Form Components fragment set until you have at least one published object. Once published, users with access to the object can view and use the Form Components fragments. See [Creating Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects) for more information.

### [Map Custom Form Fragments to Form Fields](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/personalizing-custom-forms-using-form-fragments#map-custom-form-fragments-to-form-fields) <a href="#map-custom-form-fragments-to-form-fields" id="map-custom-form-fragments-to-form-fields"></a>

To alter the default style and functionality of a form field, you can map a [custom form fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/creating-form-fragments) to a field type:

1. Open the _Site Menu_ ( ![Site Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png) ), and click _Design_ → _Fragments_.
2.  In the Fragments application, click _Options_ ( ![Options Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png) ) in the applications bar → _Configuration_.

    ![It's possible to map fragments to fields through the Configuration menu in fragments.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/personalizing-custom-forms-using-form-fragments/images/01.png)
3.  Select the _Form Fragment_ (on the right column) to be applied to the Field Type (on the left column).

    ![Apply a custom form fragment to a field type.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/personalizing-custom-forms-using-form-fragments/images/02.png)
4. Choose the fragment set under your site’s tab. Fragments with a compatible type of field appear.
5.  Select the custom form fragment you want to apply.

    ![The options that are compatible with the field can be selected.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/personalizing-custom-forms-using-form-fragments/images/03.png)

You can check if the custom fragment is applied to the field type by adding a form to a content or template page. See [Using Fragments to Build Forms](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/using-fragments-to-build-forms) to learn more about building forms using fragments.

Once you’ve applied the custom form fragment to the field type, the new style (on the left) is applied to the field type instead of the out-of-the-box style (on the right).

![The fields mapped to the custom form fragments (on the left) look different from the ones created with out-of-the-box fragments (on the right).](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/personalizing-custom-forms-using-form-fragments/images/04.png)
