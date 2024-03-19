# Advanced Forms

Manage complex forms, give access permissions to users, produce PDF and Excel files, collect and submit completed file data with attachments, and more.

Features:

#### Form Builder and Form Runner <a href="#form-builder-and-form-runner" id="form-builder-and-form-runner"></a>

* Security features
  * Protecting against attacks: [doc](https://doc.orbeon.com/configuration/advanced/security)
  * Content-Security-Policy header: [doc](https://doc.orbeon.com/configuration/advanced/security/content-security-policy)
  * Field-level encryption: [doc](https://doc.orbeon.com/form-builder/advanced/field-level-encryption)
* Inserting and reordering grid rows: [blog post](https://blog.orbeon.com/2013/11/inserting-and-reordering-grid-rows.html)
* Repeated grids and sections
  * Repeat settings: [doc](https://doc.orbeon.com/form-builder/form-editor/container-settings)
  * Repeated grids: [doc](https://doc.orbeon.com/form-builder/form-editor/repeated-grids), [older blog post](https://blog.orbeon.com/2012/04/support-for-repeats-lands-in-form.html)
  * Repeated sections: [blog post](https://blog.orbeon.com/2014/01/repeated-sections.html)
  * Options for new repeat repetitions: [blog post](https://blog.orbeon.com/2015/10/repeated-grids-and-sections-just-got.html)
  * Minimal appearances of repeated grids and sections: [blog post](https://blog.orbeon.com/2015/12/leaner-repeated-sections-and-grids.html)
  * Frozen repetitions
* Section Templates:
  * main feature: [doc](https://doc.orbeon.com/form-builder/advanced/section-templates)
  * merging: [doc](https://doc.orbeon.com/form-builder/advanced/section-templates#merging-section-templates)
* Singleton forms: [doc](https://doc.orbeon.com/form-runner/advanced/singleton-form)
* Versioning of form definitions: [doc](https://doc.orbeon.com/form-runner/features/versioning), [blog post about concept](https://blog.orbeon.com/2014/02/form-versioning.html), [blog post about publish options](https://blog.orbeon.com/2015/01/choosing-best-versioning-option-when.html)
  * overwriting of existing version: [doc](https://doc.orbeon.com/form-builder/form-editor/publishing#versioning)
  * associating a comment with a given form version: [doc](https://doc.orbeon.com/form-builder/form-editor/publishing#versioning), [blog post](https://blog.orbeon.com/2016/09/versioning-comments.html)
  * simple data migration: [doc](https://doc.orbeon.com/form-runner/features/simple-data-migration), [blog post](https://blog.orbeon.com/2018/09/simple-data-migration.html)
* Viewing data revision history: [doc](https://doc.orbeon.com/form-runner/features/revision-history)
* Form field validation: [doc](https://doc.orbeon.com/form-builder/form-editor/validation), [blog post](https://blog.orbeon.com/2013/07/enhanced-validation-in-form-builder-and.html)
  * required fields (also via formula, see [blog post](https://blog.orbeon.com/2014/09/control-required-values-with-formulas.html))
  * whitespace trimming: [doc](https://doc.orbeon.com/form-builder/form-editor/validation#trimming-leading-and-trailing-spaces), [blog post](https://blog.orbeon.com/2016/02/required-fields-more-subtle-than-you.html)
  * data types such as string, number, date, etc.
  * multiple constraints with formulas
  * common constraints: [doc](https://doc.orbeon.com/form-builder/form-editor/validation#commonconstraints), [blog post](https://blog.orbeon.com/2015/07/how-common-constraints-work.html)
    * minimum/maximum length
    * positive, positive or zero, negative or zero, or negative value
    * maximum fractional digits
    * attachment sizes and file types
  * errors, warnings, and informational validations
  * custom alert messages per validation
* Access control
  * Owner / group permissions: [doc](https://doc.orbeon.com/form-runner/access-control/owner-group), [blog post](https://blog.orbeon.com/2013/09/ownergroup-based-permissions-aka-see.html)
  * Organizations: [doc](https://doc.orbeon.com/form-runner/access-control/organization)
* Persistence (databases)
  * persistence API: [doc](https://doc.orbeon.com/form-runner/api/persistence)
  * persistence implementations: [doc](https://doc.orbeon.com/form-runner/persistence/db-support)
  * relational database support: [doc](https://doc.orbeon.com/form-runner/persistence/relational-db), [blog post about new indexes](https://blog.orbeon.com/2016/06/new-indexes-boost-performance-with.html)
* Datasets: [doc](https://doc.orbeon.com/form-runner/features/datasets), [blog post](https://blog.orbeon.com/2017/01/datasets.html)
* Multitenancy: [doc](https://doc.orbeon.com/form-runner/features/multitenancy)
* User menu: [doc](https://github.com/orbeon/orbeon-forms-doc/blob/master/form-runner/access-control/login-logout/README.md)

#### Form Builder <a href="#form-builder" id="form-builder"></a>

* 12-column layout: [doc](https://doc.orbeon.com/form-builder/form-editor/form-area#the-12-column-layout), [blog post](https://blog.orbeon.com/2018/05/the-12-column-layout.html)
  * Drag & drop of cell boundaries: [blog post](https://blog.orbeon.com/2018/10/resizing-cells-with-drag-drop-in-form.html)
* Summary page: [doc](https://doc.orbeon.com/form-builder/summary-page)
* Toolbox:
  * features: [doc](https://doc.orbeon.com/form-builder/form-editor/toolbox)
  * configurability: [doc](https://doc.orbeon.com/form-builder/advanced/extensibility/metadata)
  * XBL components: [doc](https://doc.orbeon.com/xforms/xbl)
  * undo and redo: [doc](https://doc.orbeon.com/form-builder/form-editor/undo-redo), [blog post](https://blog.orbeon.com/2017/12/new-orbeon-forms-20172-feature-undo-and.html)
  * cut, copy and paste: [doc](https://doc.orbeon.com/form-builder/form-editor/cut-copy-paste)
* Form Settings: [doc](https://doc.orbeon.com/form-builder/form-settings)
  * General Settings
  * HTML form description
  * Form Options: [doc](https://doc.orbeon.com/form-builder/form-settings#form-options)
    * Singleton forms: [doc](https://doc.orbeon.com/form-runner/advanced/singleton-form)
    * Wizard view: [doc](https://doc.orbeon.com/form-runner/features/wizard-view)
  * Control Settings
  * View Options
  * PDF Options
  * About this Form
* Control Settings: [doc](https://doc.orbeon.com/form-builder/form-editor/control-settings)
  * control name
  * Summary page options
  * custom CSS classes
  * easy switching of control appearances: [blog post](https://blog.orbeon.com/2015/06/how-new-form-builder-appearance.html)
  * validation: [doc](https://doc.orbeon.com/form-builder/form-editor/validation)
  * formulas: [doc](https://doc.orbeon.com/form-builder/formulas)
  * help text (plain text and rich text)
    * appearance of help messages: [blog post](https://blog.orbeon.com/2014/01/improving-how-we-show-help-messages.html)
* Choices editor: [doc](https://doc.orbeon.com/form-builder/form-editor/itemset-editor)
  * Hints for checkboxes and radio buttons: [blog post](https://blog.orbeon.com/2014/02/hints-for-checkboxes-and-radio-buttons.html)
* Section Settings: [doc](https://doc.orbeon.com/form-builder/form-editor/section-settings)
  * whether section is collapsible
  * whether section is initially collapsed
* Publishing a form definition: [doc](https://doc.orbeon.com/form-builder/form-editor/publishing)
* Explanation text: [blog post](https://blog.orbeon.com/2015/04/adding-explanatory-text-to-your-forms.html)
* Extension API: [doc](https://doc.orbeon.com/form-builder/advanced/extensibility/extension-api)
* Access control for editing forms: [doc](https://doc.orbeon.com/form-runner/access-control)
* Internationalization (i18n) / localization (l10n): [doc](https://doc.orbeon.com/form-builder/localization)
* Services and actions
  * HTTP Services: [doc](https://doc.orbeon.com/form-builder/advanced/services-and-actions/http-services)
    * JSON support (for reading only): [doc](https://doc.orbeon.com/xforms/submission/submission-standard#json-support)
    * setting URL parameters
  * Database Services: \[doc]\(form-builder/database-services.md
  * Actions: [doc](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions)
* XML Schema Support: [doc](https://doc.orbeon.com/form-builder/advanced/xml-schema-support)
* Source code editor: [doc](https://doc.orbeon.com/form-runner/component/source-code-editor)whether section is collapsible
* Extension API: [doc](https://doc.orbeon.com/form-builder/advanced/extensibility/extension-api)
* Basic keyboard shortcuts
  * Cut/Copy/Paste: [doc](https://doc.orbeon.com/form-builder/form-editor/cut-copy-paste#keyboard-shortcuts)
  * Undo/Redo: [doc](https://doc.orbeon.com/form-builder/form-editor/undo-redo#keyboard-shortcuts)
  * Save button: [doc](https://doc.orbeon.com/form-builder/form-editor#keyboard-shortcuts)

#### Form Runner <a href="#form-runner" id="form-runner"></a>

* Standard look & feel
* Automatic calculations dependencies: [doc](https://doc.orbeon.com/form-runner/features/automatic-calculations-dependencies), [blog post](https://blog.orbeon.com/2018/10/automatic-calculation-dependencies.html)
* Custom XBL components: [doc](https://doc.orbeon.com/xforms/xbl)
* Summary Page: [blog post](https://blog.orbeon.com/2014/06/the-form-builder-summary-page-and-form.html)
  * search
* Detail Page
  * Responsive design for mobile devices such as smartphones: [doc](https://doc.orbeon.com/form-runner/features/responsive-design), [blog](https://blog.orbeon.com/2015/08/responsive-design.html)
    * show numeric keypad/numbers and punctuation" keyboard pane when possible: [doc](https://doc.orbeon.com/form-runner/component/number#mobile-support), [blog post](https://blog.orbeon.com/2016/01/better-numeric-input-on-mobile.html)
  * Review mode (printable)
  * Wizard view: [doc](https://doc.orbeon.com/form-runner/features/wizard-view), [introduction blog post](https://blog.orbeon.com/2012/12/form-runner-wizard-view.html)
    * validated mode: [blog post](https://blog.orbeon.com/2015/03/new-wizard-validated-mode.html), [doc](https://github.com/orbeon/orbeon-forms-doc/blob/master/feature/wizard-view.md#validated-mode)
    * highlighting of sections in error/accessible sections
    * buttons appearing only on the wizard's last page
    * status information for each section
    * subsection navigation
    * optional separate table of contents
* File scan API: [doc](https://doc.orbeon.com/form-runner/api/other-apis/file-scan-api)
* Forms Admin Page: [doc](https://doc.orbeon.com/form-runner/pages/forms-admin-page), [blog post](https://blog.orbeon.com/2014/06/the-form-builder-summary-page-and-form.html)
* Published Forms Page: [doc](https://doc.orbeon.com/form-runner/pages/published-forms-page), [blog post](https://blog.orbeon.com/2014/06/the-form-builder-summary-page-and-form.html)
* Landing page: [doc](https://doc.orbeon.com/form-runner/pages/landing-page)
* Excel Import: [doc](https://doc.orbeon.com/form-runner/advanced/excel)
* Buttons and Processes: [doc](https://doc.orbeon.com/form-runner/advanced/buttons-and-processes), [blog post](https://blog.orbeon.com/2013/04/more-powerful-buttons.html)
* Autosave: [doc](https://doc.orbeon.com/form-runner/persistence/autosave), [blog post](https://blog.orbeon.com/2013/10/autosave.html)
* Export
  * [Export of form definitions and form data](https://doc.orbeon.com/form-runner/features/exporting-form-definitions-and-form-data)
  * Excel export
  * XML export
* [Purging historical data](https://doc.orbeon.com/form-runner/features/purging-historical-data)
* PDF
  * Production: [doc](https://doc.orbeon.com/form-runner/features/pdf-production)
    * Automatic
    * Template-based: [doc](https://doc.orbeon.com/form-runner/features/pdf-production/pdf-templates)
      * multiple PDF templates
  * Automatic highlighting of links [blog post](https://blog.orbeon.com/2015/04/automatic-web-links-in-pdf-files.html)
  * Customizable file name: [doc](https://github.com/orbeon/orbeon-forms-doc/blob/master/configuration/properties/form-runner/form-runner-detail-page.md#custom-pdf-filename)
  * TIFF production: [doc](https://doc.orbeon.com/form-runner/features/tiff-production)
  * Ability to send to services: [blog post](https://blog.orbeon.com/2016/08/submitting-pdf-file-to-external-service.html)
* Validation
  * as-you-type validation
  * explicit validation: [doc](https://github.com/orbeon/orbeon-forms-doc/blob/master/configuration/properties/form-runner/form-runner-detail-page.md#validation-mode)
  * centralized error summary showing currently relevant errors: [component doc](https://doc.orbeon.com/form-runner/component/error-summary), [blog post on warnings/infos enhancements](https://blog.orbeon.com/2013/07/enhanced-validation-in-form-builder-and.html)
* Sending emails
  * controlling which attachments are included: [Form Builder doc](https://doc.orbeon.com/form-builder/form-editor/control-settings#basic-options), [Form Runner doc](https://github.com/orbeon/orbeon-forms-doc/blob/master/configuration/properties/form-runner/form-runner-detail-page/form-runner-email/README.md#attachment-properties)
  * controlling To, Cc, Bcc, From: [doc](https://doc.orbeon.com/form-builder/form-editor/control-settings), [blog post](https://blog.orbeon.com/2017/05/more-flexible-email-senders-and.html)
  * Email templates: [doc](https://doc.orbeon.com/form-builder/advanced/email-settings), [blog post](https://blog.orbeon.com/2018/11/email-templates.html)
* Captcha: [properties doc](https://github.com/orbeon/orbeon-forms-doc/blob/master/configuration/properties/form-runner/form-runner-detail-page.md#captcha), [component doc](https://doc.orbeon.com/form-runner/component/captcha), [blog post](https://blog.orbeon.com/2011/12/stop-spammer-by-adding-captcha-to-your.html)
* Appearance of repeated sections: [properties doc](https://doc.orbeon.com/configuration/properties/form-runner#appearance-of-repeated-sections), [component doc](https://doc.orbeon.com/form-runner/component/grid#repeated-mode)
* Appearance of repeated grids: [properties doc](https://doc.orbeon.com/configuration/properties/form-runner#appearance-of-grids-sections), [component doc](https://doc.orbeon.com/form-runner/component/section#repeated-mode)
* Function library: [doc](https://doc.orbeon.com/xforms/xpath/extension-functions/extension-form-runner), [blog post](https://blog.orbeon.com/2016/08/a-stable-function-library-for-form.html)
* Repeated content synchronization: [doc](https://doc.orbeon.com/form-builder/advanced/services-and-actions/synchronize-repeated-content)
* Grids
  * HTML tables-free layout: [blog post](https://blog.orbeon.com/2016/06/saying-farewell-to-html-tables.html))
* Services and APIs
  * [Duplicate form data API](https://doc.orbeon.com/form-runner/api/other-apis/duplicate-form-data)
  * [Generate XML Schema API](https://doc.orbeon.com/form-runner/api/other-apis/duplicate-form-data)
  * [List form data attachments API](https://doc.orbeon.com/form-runner/api/persistence/list-form-data-attachments)
  * [Publish form definition API](https://doc.orbeon.com/form-runner/api/other-apis/publish)
  * [Run form in the background API](https://doc.orbeon.com/form-runner/api/other-apis/run-form-background)

#### Form controls <a href="#form-controls" id="form-controls"></a>

* Text controls
  * Text field
  * Plain text area
  * Formatted text area
  * Password field
* Utility controls
  * Explanatory text
  * Calculated value
  * Hidden field: [doc](https://doc.orbeon.com/form-runner/component/hidden), [blog post](https://blog.orbeon.com/2019/02/hidden-fields.html)
* Typed controls
  * Number
    * control rounding when formatting/capturing data
    * left/right alignment
  * Email address
  * Currency
  * US phone number
  * US state
* Date and time controls
  * Date, time, date and time
    * date picker
    * option to exclude dates from the date picker: [doc](https://doc.orbeon.com/form-builder/form-editor/validation#dates-to-exclude-constraint)
    * date control: [doc](https://doc.orbeon.com/form-runner/component/date)
    * time control: [doc](https://doc.orbeon.com/form-runner/component/time)
  * Dropdown date
  * Fields date
* Selection controls [doc](https://doc.orbeon.com/form-builder/form-editor/toolbox#selection-controls)
  * Dropdown menu
  * Radio buttons
  * Radio buttons with "Other": [doc](https://doc.orbeon.com/form-runner/component/open-select1)
  * Checkboxes
  * Scrollable checkboxes
  * Yes/No answer: [doc](https://doc.orbeon.com/form-runner/component/yesno-input)
  * Single checkbox: [doc](https://doc.orbeon.com/form-runner/component/checkbox-input)
  * Dynamic data dropdown
  * Autocomplete
* Attachments
  * File attachment
  * Image attachment
  * Static image
  * Static video
  * Handwritten signature: [doc](https://doc.orbeon.com/form-runner/component/handwritten-signature)
  * Image annotation: [blog post](https://blog.orbeon.com/2013/08/new-image-annotation-control.html)
  * Control maximum size and file types: [blog post](https://blog.orbeon.com/2017/04/improved-constraints-on-attachments.html)
* Buttons
  * Button
  * Link button
* Other
  * Character counter: [doc](https://doc.orbeon.com/form-runner/component/character-counter)
  * Custom components: [doc](https://doc.orbeon.com/xforms/xbl)
    * support JavaScript Companion Classes: [doc](https://doc.orbeon.com/xforms/xbl/advanced-topics/javascript)

#### Databases <a href="#databases" id="databases"></a>

* Database support: [doc](https://doc.orbeon.com/form-runner/persistence/db-support)
* SQL Server support in Orbeon Forms: [blog post](https://blog.orbeon.com/2014/05/sql-server-support-in-orbeon-forms.html)
* PostgreSQL support in Orbeon Forms: [blog post](https://blog.orbeon.com/2014/12/postgresql-support-in-orbeon-forms.html)

#### Form handling <a href="#form-handling" id="form-handling"></a>

* Session heartbeat: [doc](https://doc.orbeon.com/contributors/state-handling)
* Browser back/forward button support: [doc](https://doc.orbeon.com/contributors/state-handling)

#### Embedding <a href="#embedding" id="embedding"></a>

* Server side Embedding: [doc](https://doc.orbeon.com/form-runner/link-embed/java-api), [blog post](https://blog.orbeon.com/2014/09/embedding-support-in-orbeon-forms-47.html), [Form Builder embedding blog post](https://blog.orbeon.com/2017/02/form-builder-embedding.html)
* Liferay proxy portlet: [doc](https://doc.orbeon.com/form-runner/link-embed/liferay-proxy-portlet)
* Liferay full portlet: [doc](https://doc.orbeon.com/form-runner/link-embed/liferay-full-portlet)

#### Performance <a href="#performance" id="performance"></a>

* Limiter filter to limit the number of concurrent form requests: [doc](https://doc.orbeon.com/configuration/advanced/limiter-filter)
* Internal service requests: [blog post](https://blog.orbeon.com/2015/01/saying-goodbye-to-internal-http.html)

#### Misc <a href="#misc" id="misc"></a>

* Loading indicator and spinners: [blog post](https://blog.orbeon.com/2016/04/how-do-you-tell-users-something-is.html)
* Namespaced jQuery to avoid conflicts with other jQuery versions
* Run modes: [doc](https://doc.orbeon.com/configuration/advanced/run-modes), [blog](https://blog.orbeon.com/2012/05/run-modes.html)
