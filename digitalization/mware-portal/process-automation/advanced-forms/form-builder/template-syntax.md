# Template syntax

### Templates <a href="#templates" id="templates"></a>

You define a template by setting a placeholder with the following syntax:

```
{$foo}
```

where `foo` is a name which must match one of the template parameters configured below.

### Template parameters <a href="#template-parameters" id="template-parameters"></a>

You can add new parameters with the "+" icon and remove them with the "-" icon.

The following options are available for each template parameter:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-264298a8f9c054e7124b6d38a4000f8a09a727da%252Ftemplate-parameter-options.png%3Falt=media\&width=300\&dpr=4\&quality=100\&sign=e39ca810381d70cc5c68d2b45f6d5c5c06490ca27a510853b4ab3ef03cdb857f)

* Parameter name: this must be unique among parameters.
* Parameter value:
  * **Control Value:** the value of a form control.
  * **XPath Expression:** calculated expression.
  * **All Control Values:** the value of all form controls.
    * This is only available for the email body.
    * This is experimental.
  * **Links**&#x20;
    * Links are available for the following:
      * Email Body
      * Label
      * Hint
      * Help Message
      * Explanatory Text
    * Links include:
      * Link to the "edit" page
      * Link to the "view" page
      * Link to the "new" page
      * Link to the "summary" page
      * Link to the "home" page
      * Link to the PDF file
  * **Automatic PDF only**
    * Image (form logo by default)
    * Form title
    * Page number
    * Page count

A template may omit references to any or all template parameters.

### Links <a href="#links" id="links"></a>

Links are intended to point to some Advanced Forms pages or resources (namely, the PDF file). The end user might see such links in explanatory text and email bodies, in particular.

<figure><img src="https://doc.orbeon.com/~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-M4Uik0YvALonqST3SbT%252F-M4UilXSzCfo29i3G1n1%252Ftemplate-parameter-links-example.png%3Fgeneration=1586449030236823%26alt=media&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=115d73e3f50feb818cfdb09276cefe1da9fb99892a4d427926ebf5204dd5c9df" alt=""><figcaption></figcaption></figure>

To insert the URL, you use the template syntax within the "URL" field of the link dialog:

<figure><img src="https://doc.orbeon.com/~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252Fda4b18a94f8cdc0cc0ea7b45d45cc5e5fffecb60.png%3Fgeneration=1593110546549524%26alt=media&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=50e7228e24ab3f3ebbf12dbaf453abd0e2f92b4ea9b8ff2bf834b630ab74c5fc" alt=""><figcaption></figcaption></figure>

Form Runner requires the ability to know how to reach Form Runner. In some cases (use of a reverse proxy), Advanced Forms cannot determine this automatically. For this purpose, the following property allows setting the external Form Runner URL. By default, it is empty, and can set it to an absolute URL as follows:

```
<property 
    as="xs:string"
    name="oxf.fr.external-base-url"
    value="https://forms.acme.org/forms"/>
```

You can't use links to point back to Form Runner when using embedding.

### Localization <a href="#localization" id="localization"></a>

When the form definition has more than one language:

* Each language has its own localized template.
* Template parameters are not localized and are available no matter what language is selected.

### Examples <a href="#examples" id="examples"></a>

In the following example of a dynamic control label, the `$name` variable refers to the subsequent `name` parameter. The Template Parameters section declares that `name` parameter to refer to the `name` control.

<figure><img src="https://doc.orbeon.com/~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LImUI-s4XdNuE_d1HEH%252F-LImUPKirryB_JmDLxOc%252Fcontrol-settings-label-dynamic.png%3Fgeneration=1533076681294826%26alt=media&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=c63eb704f7a35a0153c9fd59208cc5d593af36098e4f7a62a006f56daefbdcb5" alt=""><figcaption></figcaption></figure>

This is how this would appear in the running form:

<figure><img src="https://doc.orbeon.com/~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LImUI-s4XdNuE_d1HEH%252F-LImUPKkm1VXRZ4-udcg%252Fcontrol-settings-label-dynamic-runtime.png%3Fgeneration=1533076680490317%26alt=media&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=d06779015aca0eb217fe165bf8cfc2bf7744c90fc2035a7df3a2cb664c8bfc9e" alt=""><figcaption></figcaption></figure>

In the following example of dynamic email subject, the `$title` and `$author` variables refer also to the subsequent parameters.

<figure><img src="https://doc.orbeon.com/~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LFZt2gpdgtXSCCHe0-f%252F-LFZtEcvvfVnf-hiy65c%252Femail-settings-subject.png%3Fgeneration=1529627345314154%26alt=media&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=cc64a449741ab7f03324f09bc507527b0e4f6dcfba12079e15c18b772de0ff0f" alt=""><figcaption></figcaption></figure>

The text of the template is localized as usual when the form definition has more than one language.
