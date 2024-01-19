# Forms

Portal Forms gives you robust form building capability.

Because the complexity of use cases for Forms varies from a single input field to many pages of fields with different configurations, it makes sense to show you how to build and publish simple forms very quickly, and then show you all the additional features you can use for more complex use cases. Here’s a sampling of the what the Forms application can do:

* Populate a Select or Radio field with a REST Data Provider
* Make a field appear based on the value of another field
* Add extra pages to the form
* Enable CAPTCHA for a form
* Store results in JSON
* Enable workflow for the form
* Redirect to a different URL after a successful form submission
* Send an email notification to administrators whenever a form is submitted
* Provide a default value (entered if left alone by the user) or a placeholder value (not entered if left alone by user) for each field
* Validate fields using a number of different criteria
* Redirect users to a success page after form submission
* Define Form Rules to create dynamic form behavior (for example, show or hide a field based on input in another field).
* Translate form text into any supported language.
* Create partial forms (with fields and other elements and specific configurations) and save them for reuse.
* Drag and drop fields onto the form layout.
* Duplicate a form instead of starting a similar form from scratch.

Despite this long list of more complex options, developing a simple, elegant form to suit basic needs takes little effort. The next article covers basic form building.

### Forms and Lists <a href="#forms-and-lists" id="forms-and-lists"></a>

When you need a form, what you’re really looking for is data. There are two applications for building forms to collect precisely the data you need:

1. Portal Forms: The primary form building application is for the simplest one or two question survey to the most complex, multi-page, homeowners insurance application containing rules and lists populated by a REST data provider.
2. Dynamic Data Lists (DDL): Provides a user interface tool for building reusable form- and list-based applications intended for display on pages, using templates.

### Which Form Builder Should I Use? <a href="#which-form-builder-should-i-use" id="which-form-builder-should-i-use"></a>

So the question “Which form builder should I use?” can be restated to “When should I use Dynamic Data Lists?”

* Use Dynamic Data Lists (DDL) if you need a way for users to enter data, _and_ you need to display the data in the user interface.
* Use DDL if you need to style your lists and forms with templates.
* Use DDL if there’s a field type you need that’s not included (yet) in Portal Forms. These are the field types included in DDL that _are not_ in Portal Forms at the time of this writing: - Color - Geolocation - Web Content - Link to Page

It’s important to note that these (and more!) form field types will be included in future versions of the Portal Forms application.

When all form building features are fully merged into Portal Forms, the best features of DDL, all the new features of Portal Forms, and all future improvements will be in one application. Now is the time to familiarize yourself with Portal Forms and begin using it for all your form building needs, except for the narrow use cases described above.
