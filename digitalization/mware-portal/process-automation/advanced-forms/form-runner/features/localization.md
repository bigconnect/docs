# Localization

There are three distinct categories of languages to consider:

1. The languages of the forms you create.
2. The languages of the Form Builder user interface.
3. The languages of the Form Runner user interface.

This means that a form author could use Form Builder in the French language, create a form in Mandarin Chinese, and run it with the Form Runner's English user interface.

For more on how to localize your forms in Form Builder, see [Form Localization in Form Builder](https://doc.orbeon.com/form-builder/localization). For more on how to localize the Form Builder or Form Runner user interface, see [Localizing Orbeon Forms](https://doc.orbeon.com/contributors/localizing-orbeon-forms). In what follows, we'll discuss how Form Runner picks which language to use at runtime, and what languages are supported by Form Builder and Form Runner out-of-the-box.

### Language selection at runtime <a href="#language-selection-at-runtime" id="language-selection-at-runtime"></a>

#### Determination of the form's language <a href="#determination-of-the-forms-language" id="determination-of-the-forms-language"></a>

The following applies to Form Runner pages which deal with a given form definition, such as the Summary page and Detail page. It doesn't apply to the Form Runner Home page, which deals with multiple form definitions.

To determine what language to use, Form Runner determines a list of _available languages_ for the form, as well as a _requested language_. From these, it determines the language to actually use.

The list of available languages for the current form is selected as follows:

* It starts with the list of languages defined in the form definition. There is always at least one such language present.
* If the [`oxf.fr.available-languages.*.*` property](https://doc.orbeon.com/configuration/properties/form-runner#available-languages) specifies at least one language, then only those languages which are both in the form definition _and_ specified by the property are retained.

_NOTE: The result can be an empty selection._

The requested language is determined following this order of priority:

1. The language just selected by the user in the Form Runner language selector.
2. The current Liferay language if Form Runner is used via the [Liferay proxy portlet](https://doc.orbeon.com/form-runner/link-embed/liferay-proxy-portlet) and the "Send Liferay language" option is selected.
3. The value of the `fr-language` request parameter if specified.
4. The value of the `fr-language` servlet session attribute if present.
5. The value of the [`oxf.fr.default-language.*.*` property](https://doc.orbeon.com/configuration/properties/form-runner#default-language) if present.
6. English (`en`) if everything else fails.

Then the actual form language is selected:

* If the requested language is one of the available languages, then it is selected.
* Otherwise, if the default language specified with `oxf.fr.default-language.*.*` is one of the available languages, then it is selected.
* Otherwise the first language available in the form definition is selected.

_NOTE: This means that one language is always picked, even if it is not an "available" language._

Once a language is selected, it is stored as the `fr-language` session attribute so that it is remembered when the user navigates pages. This behavior can be turned off if the `fr-remember-language=false` is passed as request parameter.

#### Determination of the Form Runner user interface language <a href="#determination-of-the-form-runner-user-interface-language" id="determination-of-the-form-runner-user-interface-language"></a>

**Summary page and Detail page**

The form's language is first selected as described above. Then the actual Form Runner user interface language is selected:

* If the form's language is one of the available Form Runner UI languages, then it is selected.
* Otherwise, if the default language specified with `oxf.fr.default-language.*.*` is one of the available Form Runner UI languages, then it is selected.
* Otherwise English is selected.

_NOTE: This means that the Form Runner user interface can be the same as the language of the form, or be different. For example, if your form definition is in Mandarin Chinese, but Form Runner doesn't support that language for its UI yet, the Form Runner UI will be in English._

**Home page**

The list of available languages is selected as follows:

* It starts with the list of Form Runner user interface languages. There is always at least one such language present.
* If the [`oxf.fr.available-languages` property](https://doc.orbeon.com/configuration/properties/form-runner#available-languages) (without wildcards) specifies at least one language, then only those languages which are both Form Runner user interface languages _and_ specified by the property are retained.

_NOTE: The result can be an empty selection._

The requested language is determined following this order of priority:

1. The language just selected by the user in the Form Runner language selector.
2. The current Liferay language if Form Runner is used via the [Liferay proxy portlet](https://doc.orbeon.com/form-runner/link-embed/liferay-proxy-portlet) and the "Send Liferay language" option is selected.
3. The value of the `fr-language` request parameter if specified.
4. The value of the `fr-language` servlet session attribute if present.
5. The value of the [`oxf.fr.default-language` property](https://doc.orbeon.com/configuration/properties/form-runner#default-language) if present.
6. English (`en`) if everything else fails.

Then the actual Form Runner user interface language is selected:

* If the requested language is one of the available languages, then it is selected.
* Otherwise, if the default language specified with `oxf.fr.default-language` is one of the available languages, then it is selected.
* Otherwise English is selected.

Once a language is selected, it is stored as the `fr-language` session attribute so that it is remembered when the user navigates pages. This behavior can be turned off if the `fr-remember-language=false` is passed as request parameter.

### Form Builder and Form Runner user interface <a href="#form-builder-and-form-runner-user-interface" id="form-builder-and-form-runner-user-interface"></a>

See [Supported languages](https://doc.orbeon.com/form-runner/features/supported-languages).

### Right to left languages <a href="#right-to-left-languages" id="right-to-left-languages"></a>

\[SINCE Orbeon Forms 2017.2] If in Form Builder you create a form in Arabic or Hebrew, then at runtime the direction is automatically set to right-to-left, and the browser renders the page accordingly. However, note that:

* Form Runner itself isn't localized in Arabic or Hebrew, which means that labels of standard interface elements, like the "Save" button, will show in English. If you need to create forms in one of those 2 languages, you will also want to [provide translations for those resources](https://doc.orbeon.com/contributors/localizing-orbeon-forms) in Arabic or Hebrew.
* As an exception to the rule, section titles will still be shown to the left, instead of the right (see [issue #3306](https://github.com/orbeon/orbeon-forms/issues/3306)).

### Known limitations <a href="#known-limitations" id="known-limitations"></a>

Form Builder (Orbeon Forms PE only) lets you create forms in multiple languages. Most world languages are available, with some limitations described below.

1. PDF output: There are some known issues with ligatures in some Indian languages such as Hindi or Tamil.
2. See the previous section for limitations specific to right-to-left languages.

We are glad to get help to address these two limitations.
