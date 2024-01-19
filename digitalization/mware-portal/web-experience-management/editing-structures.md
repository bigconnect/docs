# Editing Structures

To start, go to the _Structures_ page.

1. From _Site Administration_ go to _Content & Data_ → _Web Content_.
2. Open the _Structures_ tab.

This page shows you all the web content structures in this Site. You can add new web content structures, edit existing ones, manage the templates associated with a structure, edit the permissions of a structure, and copy or delete structures.

![Figure 1: Structures are not pre-installed. You have to make your own.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159691/manage-structures.png)

Note: When you copy a structure, the portal generates a unique ID for the copied structure, but every other attribute of the copied structure, including the name, is the same as that of the original. When you copy web content structure, enter a new name for it to avoid confusing it with the original. During the copy process, you’re prompted to choose whether to copy any detail templates or list templates associated with the structure. For information on detail templates and list templates, please refer to Dynamic Data Lists.

_Basic Web Content_, which you used in previous exercises, lives at the _Global_ scope so that it is available to all Sites. This structure and template are used automatically if a custom structure and template are not added.

### Structure Fields <a href="#structure-fields" id="structure-fields"></a>

Now, create a new Structure:

1. Click _Add_ (![Add Structure](https://liferay-support.zendesk.com/hc/article\_attachments/360030190672/icon-add.png)).
2. Give your Structure a name.

Structures are essentially a set of fields organized in a certain way. The interface on this page provides an easy way to add and organize whatever fields you need. Each element that you add has three icon options that you can click:

**Settings:** (![Settings](https://liferay-support.zendesk.com/hc/article\_attachments/360030159671/icon-wrench.png)) Changes the name and label and set other information about the field, like whether or not it is required.

**Delete:** (![Delete](https://liferay-support.zendesk.com/hc/article\_attachments/360030190592/icon-trash.png)) Removes the field from the structure.

**Duplicate:** (![Duplicate](https://liferay-support.zendesk.com/hc/article\_attachments/360030190632/icon-wysiwyg-add.png)) Duplicates the field and all its settings and iterates the _Name_ to avoid conflicts.

Web content structures can inherit characteristics from other structures. A child structure inherits all the parent’s fields and settings. You can use this to make a similar structure to one that already exists. For example, if you have _Sports Article_ and you want to create _In-depth Sports Article_, set _Sports Article_ as the parent and the _In-dept Sports Article_ inherits all its fields, letting you add new ones for more in-depth information.

**Note:** Due to import/export operations, it’s possible to have both a global and a Site-scoped structure with the same `structureKey`. If this happens, the Site-scoped structure takes precedence, and you can’t access the global structure from that Site.

You can also manually customize a structure’s XML in _Source_ mode. By default the _View_ mode is selected, but you can click the _Source_ tab to switch. This method is for more experienced developers.

Take a moment to add, delete, and rearrange different elements.

![Figure 2: The structure editor gives you many options to customize your Web Content.](https://liferay-support.zendesk.com/hc/article\_attachments/360030190652/web-content-structure-editor.png)

The following fields can be in structures:

**Boolean:** Adds a checkbox onto your structure, which stores either `true` (checked) or `false` (unchecked). Template developers can use this as a display rule.

**Color:** Adds a way to choose a color.

**Date:** Adds a pre-formatted text field that displays a date picker to assist in selecting the desired data. The format for the date is governed by the current locale.

**Decimal:** Similar to _Number_, except that it requires a decimal point (.) be present.

**Documents and Media:** Adds an existing uploaded document to attach to the structure. Can also upload documents into the Document Library.

**Geolocation:** Adds a map that displays a configured location. The geolocation system can work in two ways: letting the system know your current location (especially useful on mobile devices) and giving the user directions to a another place.

**HTML:** An area that uses a WYSIWYG editor to enhance the content.

**Image:** Adds the browse image application into your structure. You can select an image from the Documents and Media library or upload an image from your computer’s storage. If uploading an image from your personal computer to the web content article, it is only available for that article.

**Integer:** Similar to _Number_, except that it constrains user input to whole numbers.

**Link to Page:** Inserts a link to another page in the same site.

**Number:** Presents a text box that only accepts numbers as inputs, but puts no constraints on the kind of number entered.

**Radio:** Presents the user with a list of options to choose from using radio button inputs.

**Select:** Presents a selection of options for the user to choose from using a combo box. Can be configured to allow multiple selections, unlike _Radio_.

**Separator:** Adds a horizontal line between fields.

**Text:** Used for items such as titles and headings.

**Text Box:** Used for the body of your content or long descriptions.

**Web Content:** Provides a way to select a web content article.

These fields provide all you need to model any information type you would want to use as web content. Portal customers have used structures to model everything from articles, to video metadata, to wildlife databases.
