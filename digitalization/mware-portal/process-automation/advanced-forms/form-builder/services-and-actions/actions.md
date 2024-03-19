# Actions

The Form Builder Actions Editor is an [Orbeon Forms PE](https://www.orbeon.com/download) feature and allows you to implement simple actions in your form. The basic philosophy goes as follows:

1. React to an event occurring on the form, such as the form being loaded or a user action.
2. Call an HTTP or database service:
   * passing in parameters from the form,
   * receiving parameters back from the service.
3. Use the returned parameters to update the form

Actions are tightly coupled with services. In the future, support might be added for actions which do not require services.

### Basic action configuration <a href="#basic-action-configuration" id="basic-action-configuration"></a>

#### Creating or updating an action <a href="#creating-or-updating-an-action" id="creating-or-updating-an-action"></a>

You create a new action or update an existing action in the "Advanced" tab of the toolbox. This opens the Actions Editor.

#### Reordering actions <a href="#reordering-actions" id="reordering-actions"></a>

\[SINCE Orbeon Forms 2018.2]

You can reorder actions by drag and drop in the toolbox. Sometimes, the specific order of actions is important, or you might just want to organize them in a way that makes sense for your form.

![List of actions in the toolbox](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LV0ejayznqIdn0saGMq%252F-LV0esyiorYFXWsQQ7hN%252Ftoolbox-actions.png%3Fgeneration=1546216250988647%26alt=media\&width=300\&dpr=4\&quality=100\&sign=7b102ce209fdd03ee28ef2b89682e9b5e182d3e5ac8c25ef21e595e098c96592)

#### With Orbeon Forms 2017.1 or newer <a href="#with-orbeon-forms-2017.1-or-newer" id="with-orbeon-forms-2017.1-or-newer"></a>

\[SINCE Orbeon Forms 2017.1]

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-65f9fa1ea9ac5c7a7b19faa4d423afbed8d80eed%252Factions-general-settings.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=b6be65925dd41bde6638591197ee4588384aea2a7b812d06e71450c14953390f)

Actions Editor General Settings

This is the meaning of the fields of the dialog:

* **Action Name**
  * This is the name of the action, as seen by Form Builder.
  * Must start with a letter, and may not contain spaces.
* **Run condition**
  * **Always**: Run the action independently from the form mode.
  * **In "new" mode only**: Run the action only in creation mode, that is when the user creates new data, as opposed to editing, viewing, emailing, or generating a PDF.
  * **When formula evaluates to true**: \[SINCE Orbeon Forms 2021.1] Allows you to enter your own condition using XPath, which, if it evaluates to `false`, will prevent the service from running (see [video](https://www.youtube.com/watch?v=\_3mIk0OS6G8&)).
* **Run action when**
  * Specifies a phrase built of the following parts:
    * "the control"
      * label / name of the control
        * "changes its value"
        * or "appears or changes its value"
        * or "is activated" (a button is activated or the "Enter" key is pressed in a text line)
    * or "the form loads"
      * "after the controls are ready"
        * This is the default.
        * The action runs when controls are "live", which means you can set their values and list of choices.
      * "after the data is ready"
        * The action runs when the data is ready:
          * It has been loaded from the database if needed.
          * Its initial values and calculations are up to date.
        * You cannot set control values and list of choices as a result.
        * You can store datasets as a result.
      * "before the data's initial values are calculated"
        * The action runs before the data is ready.
        * You cannot set control values and list of choices as a result.
        * You can store datasets as a result.
* **Service to Call.**
  * The service to call as once the action is triggered.

Like for services, once your action is defined, the Save buttons saves it to the form. You can come back to it and modify it later by clicking on the Edit icon next to the action name. You can also delete the action using the Remove button.

### Handling the service request <a href="#handling-the-service-request" id="handling-the-service-request"></a>

#### User interface <a href="#user-interface" id="user-interface"></a>

Each row instructs the action to take a value and pass it to the service, and you can add as many such rows as needed using the "+" button, and remove existing entries with the dropdown menu.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-ecba2859b5584e94311a9aecefdf6787e2ecdb36%252Factions-request.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=ddf13338793b27119fe6629bfc58456e17bc1d929c6c7471b6788a912b43c433)

Actions Editor Service Request Actions

#### How a value is provided to the service <a href="#how-a-value-is-provided-to-the-service" id="how-a-value-is-provided-to-the-service"></a>

* With HTTP services doing a POST:
  * You provide a formula, which must point to an element or attribute node of the request body defined in the HTTP service under "XML Request Body".
*   With HTTP services doing a GET or DELETE:

    * The HTTP Service Editor [allows you to define URL parameters](https://doc.orbeon.com/form-builder/advanced/services-and-actions/http-services#url-parameters) when using the `GET` and `DELETE` HTTP methods.
    * In this case, the HTTP Service Editor implicitly creates an XML document representing these parameters, as in the example below. The Actions Editor doesn't yet support specifying URL parameters directly by name; instead, you need to enter a formula under "Set Service Request Values", like `//userId` or `//userName`.

    Copy

    ```
    <params>
        <userId>1</userId>
        <userName>test</userName>
    </params>
    ```
* With Database services:
  * To set the first query parameter, use the value "1" (without the quotes), the second, "2", etc.

#### Passing a control value <a href="#passing-a-control-value" id="passing-a-control-value"></a>

Select "Control Value" in the first dropdown.

Parameters:

* **Control**:
  * Specifies the control whose value must be used.
  * A single "closest" control will be selected:
    * If the control is not within a repeated grid or section, then that unique control is updated.
    * Otherwise, the control which is the closest by following repetitions and repeat indexes is chosen.
* **Destination XPath Expression**:
  * The expression is evaluated in the context of root element of the XML request data to send to the service.
  * The expression must point to an element or attribute node of the request body. If multiple nodes are returned, only the first one is considered.

#### Passing a value from a formula <a href="#passing-a-value-from-a-formula" id="passing-a-value-from-a-formula"></a>

\[SINCE Orbeon Forms 2021.1]

Select "Formula" in the first dropdown.

Parameters:

* **Formula**:
  * The given formula is evaluated and converted to a string.
* **Destination XPath Expression**:
  * The expression is evaluated in the context of root element of the XML request data to send to the service.
  * The expression must point to an element or attribute node of the request body. If multiple nodes are returned, only the first one is considered.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-e05859624d9d84198a6e5c835fce15e0dce56e87%252Factions-request-formula.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=c291c3ca2968c8778b3d17e3f9c64286f61be5f8f071e450ebce4466b4181cac)

Action request formula

### Handling the service response <a href="#handling-the-service-response" id="handling-the-service-response"></a>

#### User interface <a href="#user-interface-1" id="user-interface-1"></a>

\[SINCE Orbeon Forms 2017.1]

You access the response in the Service Response Actions tab. This allows adding, removing, and moving response actions:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-ee1d6ea721d68e3f3fa54fe7acd02a667b1f1a42%252Factions-response.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=88803e76ddeefe27f69ec40dfff6a23603096f846e9c417c34f9537c2beb6a51)

Actions Editor Service Response Actions

#### Setting the value of a control <a href="#setting-the-value-of-a-control" id="setting-the-value-of-a-control"></a>

As a result of running an action, you can set a form control's value from data returned by a service using:

* \[SINCE Orbeon Forms 2017.1] the "Set Control Value" action
* \[UNTIL Orbeon Forms 2016.3] the "Set Response Control Values" section

Parameters:

* **Destination Control:**
  * Specifies the control whose value must be set.
  * A single "closest" control will be selected:
    * If the destination control is not within a repeated grid or section, then that unique control is updated.
    * Otherwise, the control which is the closest by following repetitions and repeat indexes is chosen.
* **Source XPath Expression:**
  * The expression is evaluated in the context of root element of the XML data returned by the service.
  * The expression can point to an element or attribute node of the response body, but can also be a more complex expression. Its result is converted to a string.

#### Setting the choices of a selection control <a href="#setting-the-choices-of-a-selection-control" id="setting-the-choices-of-a-selection-control"></a>

**Basics**

As a result of running an action, you can set a selection control's set of items (AKA "itemset") using:

* \[SINCE Orbeon Forms 2017.1] the "Set Control Choices" action
* \[UNTIL Orbeon Forms 2016.3] the "Set response Selection Control Items" section

Selection controls include dropdown menus, checkboxes, and more.

* **Destination Selection Control.**
  * Specifies the selection control whose items must be set. Only selection controls appear in this list.
  * Depending on the relative position of the source of the action and the target selection controls, one or more "closest" controls can be selected (see the detailed explanation of the behavior below).
* **Choices XPath expression.**
  * The XPath expression must point to a set of element or attribute nodes of the response body returned by the service.
  * For each node returned, an item is created.
* **Label XPath expression.** The XPath expression must return the text of the label for an item. It is relative to the current item node.
* **Value XPath expression.** The XPath expression must return the text of the value for an item. It is relative to the current item node.

**Adjustment of control values**

\[SINCE Orbeon Forms 4.3]

Each selection control's selected value(s) are updated to be in range following the new itemset:

* For single-selection controls: if the item value currently stored in the instance data is not part of the returned set of items, the value is cleared.
* For multiple-selection controls: any of the space-separated values currently stored in the instance data that are not part of the returned set of item values are removed.

**Behavior starting with Orbeon Forms 2016.1**

In the presence of repeated grids or sections, the destination selection control can resolve to zero, one or more concrete controls.

The way this works is that the "closest" controls are searched. This means:

* If the destination selection control is not within a repeated grid or section, then the single destination control is updated.
* If the source of the action is _within the same repeated iteration_ as the destination selection control, then that single destination control is updated. Occurrences of the selection control on other repeat repetitions are not updated.
* If the source of the action is at a higher level compared to the destination selection control, then all repetitions of the selection control are updated, and subsequent new repetitions added will also use the new itemset.

**Behavior up to Orbeon Forms 4.10 included**

All the previously available items of the selection control(s) identified are replaced with the items specified. In other words, the itemset for the given control is global. This is the case even in the presence of repeated grids or sections.

**Internationalization**

\[SINCE Orbeon Forms 4.7]

Your service should return localized labels for all the languages supported by your form. For instance, if your form is available in English and French, a service you use to populate a dropdown with a list of countries might return:

Copy

```
<response>
    <row>
        <value>us</value>
        <lang>en</lang>
        <label>United States</label>
    </row>
    <row>
        <value>us</value>
        <lang>fr</lang>
        <label>États-Unis</label>
    </row>
    <row>
        <value>ch</value>
        <lang>en</lang>
        <label>Switzerland</label>
    </row>
    <row>
        <value>ch</value>
        <lang>fr</lang>
        <label>Suisse</label>
    </row>
</response>
```

After the service is called, the _items_, _label_, and _value_ XPath expressions you wrote when defining the action are executed once per language supported by the form, and for each execution the `$fr-lang` variable is set to current language. So in the case of our hypothetical service returning a list of countries, you will define the _items_ as `/response/row[lang = $fr-lang]`, the _value_ simply as `value`, and _label_ as `label`.

While in theory this allows you to have the _values_ depend on the language, to avoid unexpected behavior when users switch languages or different users look at the same data using a different language, you should make sure that values are the same for all languages, and only the _labels_ differ between languages.

#### Storing the response to a dataset <a href="#storing-the-response-to-a-dataset" id="storing-the-response-to-a-dataset"></a>

\[SINCE Orbeon Forms 2017.1]

As a result of running an action, you can store the entire set of data returned by the service into a [dataset](https://doc.orbeon.com/form-runner/features/datasets):

* use the "Save to Dataset" action
* enter a dataset name

Each dataset name identifies a unique dataset for the form. Depending on the use case, dataset names can be reused or be unique.

A dataset name must:

* start with a letter or `_`
* continue with letters, digits, `.`, `-` or `_` (if the name has more than one character)

You access data from a dataset using the [`fr:dataset()`](https://doc.orbeon.com/xforms/xpath/extension-functions/extension-form-runner#frdataset) function.

For more on datasets, see [Datasets](https://doc.orbeon.com/form-runner/features/datasets).

_NOTE: Section templates keep their own local datasets, separate from the main form._

### Namespace handling <a href="#namespace-handling" id="namespace-handling"></a>

At this point, you can't declare custom namespace mappings in the Actions Editor. So say you have a response looking like this:

Copy

```
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <my:items xmlns:my="http://example.org/my">
            <my:item>
                <my:label>Cat</my:label>
                <my:value>cat</my:value>
            </my:item>
            <my:item>
                <my:label>Dog</my:label>
                <my:value>dog</my:value>
            </my:item>
            <my:item>
                <my:label>Bird</my:label>
                <my:value>bird</my:value>
            </my:item>
        </my:items>
    </soap:Body>
</soap:Envelope>
```

or, using the default namespace mechanism:

Copy

```
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <items xmlns="http://example.org/my">
            <item>
                <label>Cat</label>
                <value>cat</value>
            </item>
            <item>
                <label>Dog</label>
                <value>dog</value>
            </item>
            <item>
                <label>Bird</label>
                <value>bird</value>
            </item>
        </items>
    </soap:Body>
</soap:Envelope>
```

In both these cases, `<my:item>` (or `<item>`) and nested elements are in the `http://example.org/my` namespace. This means that your XPath expression must match elements in a namespace and that, in theory, you need a custom namespace mapping in form builder. Since this is not supported yet, you can work around the issue by using XPath expressions with wildcards:

* Items: `/soap:Envelope/soap:Body/*:items/*:item`
* Label: `*:label`
* Value: `*:value`

You can even use shorter variations if element names are use consistently, for example:

* Items: `//*:item`
* Label: `*:label`
* Value: `*:value`

These expressions with wildcards ignore namespace information completely, so you have to be careful if your XML document contains elements with the same name but in different namespaces.
