# Configuring Structure Fields

There are many options available for configuring each structure field. Some of them relate to how the fields are displayed or how users interact with them, but probably the most important field configuration is the **Name**. When you create a new field, it has a random name generated that looks like `TextField4882`. In most cases, you should change this to something that is more memorable and more descriptive. If you create a matching template, you don’t want to have to remember if `TextField4882` was the field for entering an applicant’s name or annual salary.

Practice this now.

1. In your structure, add an _HTML_ element.
2. Hover over the field and select the _Configuration_ icon (![Configuration](https://liferay-support.zendesk.com/hc/article\_attachments/360030159751/icon-wrench.png)).
3. Change the _Field Label_ value to _Instructions_ and the _Name_ value to _steps_. Now your template writer has a variable by which he or she can refer to this field.

Here’s a list of all the configurable settings available for a structure’s fields:

**Type:** Lists the type of field placed in the definition. This is not editable but is available to reference from a template.

**Field Label:** Sets the text that can be displayed with the field. This is the human-readable text that the user sees.

**Show Label:** Select _Yes_ to display the Field Label.

**Required:** Select _Yes_ to mark the field required. If a field is required, users must enter a value for it in order to submit content using this structure.

**Name:** The name of the field internally, automatically generated. Since this is the variable name that you can read the data from in a template or display page, you should enter a descriptive name.

**Predefined Value:** When a user creates a new web content article based on a structure that has predefined values for various fields, the predefined values appear in the form as defaults for those fields.

**Tip:** Each field can have a small help icon, with a tooltip attached that displays helpful information. If you want to provide text for the tooltip, you may enter it here.

**Indexable:** Select _Yes_ to permit your field to be indexed for search.

**Localizable:** Select _Yes_ to permit localization for this field.

**Repeatable:** Select _Yes_ to make your field repeatable. Users can then add as many copies of this field as they need. For example, if you’re creating a structure for articles, you might want a repeatable Author field in case you have multiple authors for a particular article.

**Multiple:** Select _Yes_ to enable a multi-selection list (only available for the Select field).

**Options:** Changes the options available for selection. You can add and remove options as well as edit each individual option’s display name and value (only available for Radio and Select fields).

**Style:** Changes the line separator’s style (only available for Separator).

#### STRUCTURE DEFAULT VALUES <a href="#structure-default-values" id="structure-default-values"></a>

You can define Structure Default Values for repeatable values in content created from that structure. They can also set defaults for the portal’s standard asset fields (like tags, categories, and related assets) and the content of the structure fields, while also setting a default template for displaying the structure data.

![Figure 1: You can edit default values via the Actions button of the Manage Structures interface.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159711/structure-actions.png)

Returning to the newspaper scenario again, suppose you want all sports articles to have the same display page (sports page), the same categories, or the same set of tags. Instead of adding them for each article or wondering if your users are adding them to every web content article, you can add these characteristics once for every sports article by creating default values for the structure. Creating default values is not part of creating a new structure, so make sure you have an existing structure.

To edit a structure’s default values:

1. Go to _Site Administration_ → _Content & Data_ → _Web Content_ and click on the _Structures_ tab to see the structures list.
2.  Find the _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030159771/icon-actions.png)) for the desired structure and select _Edit Default Values_ from the menu to view a window like the one below.

    This form manages the structure settings. It duplicates the function of the _Predefined Value_ field setting (see above), but is much more convenient for setting or editing a large number of defaults at once.

![Figure 2: You can define values for your structure fields and the standard asset metadata fields.](https://liferay-support.zendesk.com/hc/article\_attachments/360040158792/structure-default-values.png)

If you prefer to reset all your structure’s default values, you can select the _Reset Values_ button. This resets all values for the structure’s fields and all properties shown in the right panel.

Every new web content you create with this structure is preloaded with the data you inserted. Next, you’ll learn about assigning permissions.
