# Localizing Object Definitions and Entries

Objects are integrated with the localization framework, so you can add translations for definition labels, and users can enter localized values for text fields. With this integration, you can localize objects to better serve your global users.

### Localizing Object Definition Labels <a href="#localizing-object-definition-labels" id="localizing-object-definition-labels"></a>

When configuring an object definition, you can localize the definition’s labels, as well as labels for its fields, relationships, actions, and view columns. This determines how these elements appear in object layouts and views.

![Localize the Object definition's labels, as well as labels for its fields, relationships, actions, and view columns.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/localizing-object-definitions-and-entries/images/01.png)

Additionally, you can localize validation error messages.

![Localize validation error messages.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/localizing-object-definitions-and-entries/images/02.png)

### Enabling Localization for Text Fields <a href="#enabling-localization-for-text-fields" id="enabling-localization-for-text-fields"></a>

In addition to localizing definition labels, you can enable translation support for custom text fields so end users can localize their input:

1. Open the Objects application and begin editing the desired definition.
2.  In the Details tab, toggle _Enable Entry Translation_.

    ![Enable entry translation.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/localizing-object-definitions-and-entries/images/03.png)
3. Click _Save_.

Now when adding or configuring Text, Long Text, or Rich Text fields, you can determine whether the field is localizable.

![Determine whether text fields are localizable.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/localizing-object-definitions-and-entries/images/04.png)

IMPORTANT

Localized fields cannot be mandatory. Also, when using localization, field validations, conditions, filters, and sorting only support the default language.

While enabled, you can use the language selector button to localize the field’s value when creating entries.

![Use the language selector button to localize text field values.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/localizing-object-definitions-and-entries/images/05.png)
