# Designing Web Content with Templates

While templates aren’t required to display web content (i.e. you can use a Display Page Template to map the structure fields), developers can create templates to display the elements of the structure in the markup they want. In essence, templates are scripts that tell the portal how to display content in the structure. Changes to the structure require corresponding changes to any templates that use it, because new or deleted fields produce errors on the page. Unless a template is generic and meant to be embedded in other templates, a template must have a matching structure.

The portal only supports creating web content templates in FreeMarker (FTL) by default. FreeMarker is the preferred, recommended language, and what you’ll use in the next example.
