# Action syntax

Orbeon Forms supports services and actions. With Orbeon Forms 2018.2, a first step towards more powerful actions is introduced. There is no interface for it yet, but instead an XML configuration which you can paste into the form definition via the ["Edit Source"](https://doc.orbeon.com/form-builder/advanced/edit-source) dialog.

### Enhancements <a href="#enhancements" id="enhancements"></a>

In addition to the features available through the ["Actions" dialog](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions), the following enhancements are available:

* Call an action in response to _multiple events_.
* Support _more event types_.
* Call an _arbitrary number_ of services.
* Run actions _without_ calling services.
* Clear repeated grid or repeated section repetitions.
* Add repeated grid or repeated section repetitions.
* _Repeatedly run_ parts of an action. \[SINCE Orbeon Forms 2019.1]
* _Conditionally run_ parts of an action. \[SINCE Orbeon Forms 2019.1]

### Updating the form definition <a href="#updating-the-form-definition" id="updating-the-form-definition"></a>

You place listeners and actions within the source code, preferably before the end of the main `<xf:model>` content. For example:

Copy

```
    <!-- other Form Builder code here -->

    <fr:listener
        version="2018.2"
        .../>
        
    <fr:action name="my-action" version="2018.2">
        ...
    </fr:action>
    
    <!-- Put `<fr:listener>` and `<fr:action>` just above this. -->
</xf:model>
```

### Example <a href="#example" id="example"></a>

The following example:

* listens to an activation event on the button `my-button`
* in response, calls the `my-action` action, which
  * sets values from controls into the `my-service` service request
  * calls the `my-service` service
* when the service call completes
  * the `my-table` repeated grid is cleared
  * iterating over the XML response, for each iteration
    * adds a repetition at the end of the `my-table` repeated grid
    * sets the value of controls on the new repetition
  * finally, all the `result-dropdown` dropdown control items are updated

Copy

```
    <!-- other Form Builder code here -->
    
    <fr:listener
        version="2018.2"
        events="activated"
        controls="my-button"
        actions="my-action"/>
    
    <fr:action name="my-action" version="2018.2">
    
        <fr:service-call service="my-service">
            <fr:value control="foo" ref="/*/foo"/>
            <fr:value control="bar" ref="/*/bar"/>
        </fr:service-call>
    
        <fr:repeat-clear repeat="my-table"/>
    
        <fr:data-iterate ref="/*/row">
            <fr:repeat-add-iteration repeat="my-table" at="end"/>
            <fr:control-setvalue value="@field" control="result-field" at="end"/>
            <fr:control-setvalue value="@dropdown" control="result-dropdown" at="end"/>
        </fr:data-iterate>
    
        <fr:control-setitems
            items="/*/response/item"
            label="@label"
            value="@value"
            control="result-dropdown"/>
    
    </fr:action>

    <!-- Put `<fr:listener>` and `<fr:action>` just above this. -->
</xf:model>
```

### Listeners <a href="#listeners" id="listeners"></a>

#### Basic syntax <a href="#basic-syntax" id="basic-syntax"></a>

A listener looks like this:

Copy

```
<fr:listener
    version="2018.2"
    modes="..."
    events="..."
    controls="..."
    actions="..."
>
```

| Attribute  | Mandatory                                                                              | Value                                 | Comment                                                                                                                    |
| ---------- | -------------------------------------------------------------------------------------- | ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `version`  | Yes                                                                                    | format version                        | always `2018.2`                                                                                                            |
| `modes`    | No                                                                                     | space-separated list of modes         | The listener is enabled for each mode listed only. If absent, the listener is enabled for all modes.                       |
| `events`   | Yes                                                                                    | space-separated list of event names   | When more than one event name is present, the listener reacts if _any_ of the listed events is present.                    |
| `controls` | Yes for events which relate to a particular control, like `enabled` or `value-changed` | space-separated list of control names | When more than one control name is present, the listener reacts if an event is dispatched to _any_ of the listed controls. |
| `actions`  | No, but nothing will happen if there is not at least one action referenced             | space-separated list of action names  | When more than one action name is present, _all_ the specified actions are called when the listener reacts to an event.    |

_NOTE: It is disallowed to mix and match, in a single listener, events for which a control name is required and events for which a control name is not required. Instead, use multiple listeners._

#### Modes supported <a href="#modes-supported" id="modes-supported"></a>

* `new`
* `edit`
* `view`
* `pdf`

xxx link to new modes page with explanations

#### Events supported <a href="#events-supported" id="events-supported"></a>

Controls:

* `enabled`: the control has become enabled
* `disabled`: the control has become disabled
* `visible`: the control has become visible (for example in a wizard page)
* `hidden`: the control has become hidden (for example in a wizard page)
* `value-changed`: the value of an enabled control has changed
* `activated`: the control has been activated (clicked, or enter in text field)
* `item-selected`: an item of an enabled control has been selected
* `item-deselected`: an item of an enabled control has been deselected

Form load:

* `form-load-before-data`: run before the data's initial values are calculated
* `form-load-after-data`: run when the data is ready
* `form-load-after-controls`: run after the controls are ready

See also [Running processes upon page load](https://doc.orbeon.com/configuration/properties/form-runner/form-runner-detail-page#running-processes-upon-page-load) for the detail of the form load events.

### Actions <a href="#actions" id="actions"></a>

#### Basic syntax <a href="#basic-syntax-1" id="basic-syntax-1"></a>

An action looks like this:

Copy

```
<fr:action
    version="2018.2" 
    name="my-action" 
>
```

| Attribute | Mandatory | Value          | Comment                               |
| --------- | --------- | -------------- | ------------------------------------- |
| `version` | Yes       | format version | always `2018.2`                       |
| `name`    | Yes       | action name    | must be unique in the form definition |

### Control structures <a href="#control-structures" id="control-structures"></a>

#### Iterating over data <a href="#iterating-over-data" id="iterating-over-data"></a>

\[SINCE Orbeon Forms 2019.1]

**Basic usage**

Copy

```
<fr:data-iterate ref="...expression...">
    ...
</fr:data-iterate>
```

`<fr:data-iterate>` allows you to iterate over data. The contained actions are executed once for each value returned by the expression.

Containing actions can include one or more calls to services.

| Attribute | Mandatory | Value                     | Comment                                      |
| --------- | --------- | ------------------------- | -------------------------------------------- |
| `ref`     | Yes       | sequence XPath expression | runs in the current XPath evaluation context |

In the following example, each repetition adds a row to the grid, calls a service, passing the attachment id, and sets the attachment value on the last row.

Copy

```
<fr:action name="populate-attachments" version="2018.2">

    <fr:service-call service="get-attachments-list"/>
    <fr:repeat-clear repeat="my-grid"/>

    <fr:data-iterate ref="/*/row">
        <fr:repeat-add-iteration repeat="my-grid" at="end"/>
        <fr:service-call service="get-attachment">
            <fr:url-param name="text" value="attachment-id"/>
        </fr:service-call>
        <fr:control-setattachment control="my-attachment" at="end"/>
    </fr:data-iterate>

</fr:action>
```

**Nesting of iterations**

\[SINCE Orbeon Forms 2019.2]

Calls to `<fr:data-iterate>` can be nested. This allows, for example, filling nested repeated sections and/or grids with the result of a service call that returns hierarchical data.

Example:

Copy

```
<fr:action name="my-action" version="2018.2">
    <fr:service-call service="get-nobel-prizes"/>
    <fr:repeat-clear repeat="prizes"/>
    <fr:data-iterate ref="/*/prizes/_">
        <fr:repeat-add-iteration repeat="prizes"/>
        <fr:control-setvalue value="year"     control="year"     at="end"/>
        <fr:control-setvalue value="category" control="category" at="end"/>
        <fr:repeat-clear repeat="laureates"/>
        <fr:data-iterate ref="laureates/_">
            <fr:repeat-add-iteration repeat="laureates"/>
            <fr:control-setvalue value="firstname"  control="firstname"  at="end"/>
            <fr:control-setvalue value="surname"    control="surname"    at="end"/>
            <fr:control-setvalue value="motivation" control="motivation" at="end"/>
        </fr:data-iterate>
    </fr:data-iterate>
</fr:action>
```

#### Conditions <a href="#conditions" id="conditions"></a>

\[SINCE Orbeon Forms 2019.1]

Copy

```
<fr:if condition="...boolean expression...">
    ...
</fr:if>
```

`<fr:if>` allows you to conditionally run a block of actions.

| Attribute   | Mandatory | Value                    | Comment                                      |
| ----------- | --------- | ------------------------ | -------------------------------------------- |
| `condition` | Yes       | boolean XPath expression | runs in the current XPath evaluation context |

In the following example, with the repetition performed by `<fr:data-iterate>`, the call to the service that retrieves an attachment depends on whether there is a non-blank attachment id provided.

Copy

```
<fr:action name="populate-attachments" version="2018.2">

    <fr:service-call service="get-attachments-list"/>
    <fr:repeat-clear repeat="my-grid"/>

    <fr:data-iterate ref="/*/row">
        <fr:repeat-add-iteration repeat="my-grid" at="end"/>
        <fr:if condition="xxf:non-blank(attachment-id)">
            <fr:service-call service="get-attachment">
                <fr:url-param name="text" value="attachment-id"/>
            </fr:service-call>
            <fr:control-setattachment control="my-attachment" at="end"/>
        </fr:if>
    </fr:data-iterate>

</fr:action>
```

### Asynchronous actions <a href="#asynchronous-actions" id="asynchronous-actions"></a>

[\[SINCE Orbeon Forms 2023.1\]](https://doc.orbeon.com/release-notes/orbeon-forms-2023.1)

Previously, actions were run synchronously. That is, they were blocking any other form processing until completed. It is now possible to run actions asynchronously. This means that the form processing can continue while the actions are running in the background. This is particularly useful when the actions call services.

You can enable asynchronous actions:

* at the action level, using the `async="true` attribute
* at the form level, using the `oxf.fr.detail.actions.async.*.*` property

Example of configuration property:

Copy

```
<property 
    as="xs:boolean" 
    name="oxf.fr.detail.actions.async.*.*"                            
    value="true"/>
```

It is currently not possible to mix and match synchronous and asynchronous service calls within a given action.

An asynchronous action, by default, causes a response to the client (web browser) to wait forever for its completion. This is done to enhance the backward-compatibility of asynchronous actions. This can be disabled:

* at the action level, using the `response-must-await="0ms"` attribute (or `0` followed by any other unit allowed as shown below)
* at the form level, using the `oxf.fr.detail.actions.response-must-await.*.*` property

Example of configuration property:

Copy

```
<property 
    as="xs:string" 
    name="oxf.fr.detail.actions.response-must-await.*.*"              
    value="0ms"/>
```

If `response-must-await` is set to `0ms`, a response to the client doesn't wait for the completion of asynchronous actions. Instead, the response is sent immediately, and the pending action's service calls continue to wait in the background. The client polls the server at regular intervals to check for the completion of such pending services, and then resumes the processing of the actions. This is useful when an action calls long-running services, and you don't want to keep the client waiting for its completion.

Possible values for this property and attribute are:

* `forever`
* `"$length$unit"` (whitespace allowed around and between tokens)
  * `$length` is a positive long integer value
  * `$unit` is as described below

| Unit | Description  |
| ---- | ------------ |
| `h`  | hours        |
| `m`  | minutes      |
| `s`  | seconds      |
| `ms` | milliseconds |

Practically, units in the `ms` and `s` ranges are the most useful.

Examples:

* `response-must-await="0ms"`: don't wait
* `response-must-await="200ms"`: wait up to 200 milliseconds
* `response-must-await="5s"`: wait up to 5 seconds
* `response-must-await="forever`"\`: wait indefinitely

For example, the Form Runner Landing page runs multiple background services to load lists of published forms. The page waits a few hundreds of milliseconds for the completion of those services, and if they take longer, for example due to a slow database, the page is shown to the user, and the services continue to run in the background.

In the future, asynchronous actions will likely be enabled by default.

### Resolution of form controls <a href="#resolution-of-form-controls" id="resolution-of-form-controls"></a>

Many individual actions refer to form controls. When form controls are present in a repeated section or repeated grid, there can be ambiguity as to which control is being referred to.

For this reason, an optional `at` attribute can be specified. That attribute is a space-separated list of tokens, one for each repetition level.

A token can take the following values:

* `start`: the first repetition
* `end`: the last repetition
* a strictly positive integer: the repetition at that specific position
* `all`: all controls [\[SINCE Orbeon Forms 2023.1\]](https://doc.orbeon.com/release-notes/orbeon-forms-2023.1)

When the `at` attribute is not present, it defaults to being relative to the action source. This usually means targeting the first repetition.

The following example sets the value of the control `my-repeated-value` in all repetitions of the `my-repeated-value` repeated grid to `42`

Copy

```
<fr:control-setvalue 
    value="'42'" 
    control="my-repeated-value" 
    at="all"/>
```

Assume now that the control `my-repeated-value` is within a repeated grid, itself nested within two levels of repeated sections. The following example sets the value of the control `my-repeated-value` in the third repetition of the last repetition of the first repetition to `42`:

Copy

```
<fr:control-setvalue 
    value="'42'" 
    control="my-repeated-value" 
    at="start end 3"/>
```

### Individual actions <a href="#individual-actions" id="individual-actions"></a>

#### Calling a service <a href="#calling-a-service" id="calling-a-service"></a>

Copy

```
<fr:service-call service="...service name...">
    <fr:value value="..." ref="..."/>
</fr:service-call>
```

`<fr:service-call>` calls a service by name.

| Attribute | Mandatory | Value                       | Comment                     |
| --------- | --------- | --------------------------- | --------------------------- |
| `service` | Yes       | name of the service to call | must be an existing service |

**Passing a value**

Copy

```
<fr:value value="..." ref="..."/>
```

or:

Copy

```
<fr:value control="..." ref="..."/>
```

When calling an [HTTP service](https://doc.orbeon.com/form-builder/advanced/services-and-actions/http-services), you can set XML request body values using nested `<fr:value>` elements.

| Attribute | Mandatory | Value                  | Comment                                              |
| --------- | --------- | ---------------------- | ---------------------------------------------------- |
| `control` | No        | control name           | either this or `value` must be specified             |
| `value`   | No        | value expression       | either this or `control` must be specified           |
| `ref`     | Yes       | destination expression | points to an element or attribute in the request XML |

**Passing a URL parameter**

Copy

```
<fr:url-param name="..." value="..."/>
```

When calling an [HTTP service](https://doc.orbeon.com/form-builder/advanced/services-and-actions/http-services), you can pass URL parameters using nested `<fr:url-param>` elements.

| Attribute | Mandatory | Value            | Comment                                    |
| --------- | --------- | ---------------- | ------------------------------------------ |
| `control` | No        | control name     | either this or `value` must be specified   |
| `value`   | No        | value expression | either this or `control` must be specified |
| `name`    | Yes       | parameter name   | URL parameter name                         |

**Passing a SQL parameter**

Copy

```
<fr:sql-param index="..." value="..."/>
```

When calling a [database service](https://doc.orbeon.com/form-builder/advanced/services-and-actions/database-services), you can pass parameters using nested `<fr:sql-param>` elements.

| Attribute | Mandatory | Value            | Comment                                    |
| --------- | --------- | ---------------- | ------------------------------------------ |
| `control` | No        | control name     | either this or `value` must be specified   |
| `value`   | No        | value expression | either this or `control` must be specified |
| `index`   | Yes       | positive integer | SQL query parameter index                  |

#### Removing all repetitions of a repeat <a href="#removing-all-repetitions-of-a-repeat" id="removing-all-repetitions-of-a-repeat"></a>

Copy

```
<fr:repeat-clear
    repeat="..."
    at="..."/>
```

| Attribute | Mandatory | Value                                                                  | Comment                                 |
| --------- | --------- | ---------------------------------------------------------------------- | --------------------------------------- |
| `repeat`  | Yes       | repeated grid or repeated section name                                 |                                         |
| `at`      | No        | space-delimited position tokens: `start`, `end`, or a positive integer | missing leading tokens default to `end` |

This action starts by identifying a single repeated grid or section with the `repeat` attribute. See `<fr:repeat-add-iteration>` for details. The only difference is that with this action, `at` is only used to identify the ancestor repeated sections if any.

With `my-repeated-grid` nested within `my-repeated-section`:

*   Remove all repetitions of `my-repeated-section`:

    Copy

    ```
    <fr:repeat-clear repeat="my-repeated-section"/>
    ```
*   Remove all repetitions of the last `my-repeated-grid`:

    Copy

    ```
    <fr:repeat-clear repeat="my-repeated-grid" at="end"/>
    ```
*   Remove all repetitions of the first `my-repeated-grid`:

    Copy

    ```
    <fr:repeat-clear repeat="my-repeated-grid" at="start"/>
    ```
*   Remove all repetitions of the second `my-repeated-grid`:

    Copy

    ```
    <fr:repeat-clear repeat="my-repeated-grid" at="2"/>
    ```

#### Adding repetitions to a repeat <a href="#adding-repetitions-to-a-repeat" id="adding-repetitions-to-a-repeat"></a>

Copy

```
<fr:repeat-add-iteration
    repeat="..."
    at="..."/>
```

| Attribute | Mandatory | Value                                                                  | Comment                                 |
| --------- | --------- | ---------------------------------------------------------------------- | --------------------------------------- |
| `repeat`  | Yes       | repeated grid or repeated section name                                 |                                         |
| `at`      | No        | space-delimited position tokens: `start`, `end`, or a positive integer | missing leading tokens default to `end` |

This action starts by identifying a single repeated grid or section with the `repeat` attribute. If the repeated grid or section is at the top-level, there is only one possible match. If the repeated grid or section is _nested_ within one or more repeated sections, then a single repetition of the ancestor repeated sections is determined using the optional `at` attribute.

With `my-repeated-grid` nested within `my-repeated-section`:

*   Insert a new repetition at the end of the last `my-repeated-grid`:

    Copy

    ```
    <fr:repeat-add-iteration repeat="my-repeated-grid" at="end end"/>
    ```

    or:

    Copy

    ```
    <fr:repeat-add-iteration repeat="my-repeated-grid" at="end"/>
    ```

    or:

    Copy

    ```
    <fr:repeat-add-iteration repeat="my-repeated-grid"/>
    ```
*   Insert a new repetition at the end of the first `my-repeated-grid`:

    Copy

    ```
    <fr:repeat-add-iteration repeat="my-repeated-grid" at="start end"/>
    ```
*   Insert a new repetition at the start of the first `my-repeated-grid`:

    Copy

    ```
    <fr:repeat-add-iteration repeat="my-repeated-grid" at="start start"/>
    ```
*   Insert a new repetition after repetition 2 of the third `my-repeated-grid`:

    Copy

    ```
    <fr:repeat-add-iteration repeat="my-repeated-grid" at="3 2"/>
    ```
*   Insert a new repetition at the end of `my-repeated-section`:

    Copy

    ```
    <fr:repeat-add-iteration repeat="my-repeated-section" at="end"/>
    ```

    or:

    Copy

    ```
    <fr:repeat-add-iteration repeat="my-repeated-section"/>
    ```

#### Removing repetitions from a repeat <a href="#removing-repetitions-from-a-repeat" id="removing-repetitions-from-a-repeat"></a>

Copy

```
<fr:repeat-remove-iteration
    repeat="..."
    at="..."/>
```

| Attribute | Mandatory | Value                                                                  | Comment                                 |
| --------- | --------- | ---------------------------------------------------------------------- | --------------------------------------- |
| `repeat`  | Yes       | repeated grid or repeated section name                                 |                                         |
| `at`      | No        | space-delimited position tokens: `start`, `end`, or a positive integer | missing leading tokens default to `end` |

This action starts by identifying a single repeated grid or section with the `repeat` attribute. See `<fr:repeat-add-iteration>` for details.

With `my-repeated-grid` nested within `my-repeated-section`:

*   Remove the last repetition of the last `my-repeated-grid`:

    Copy

    ```
    <fr:repeat-remove-iteration repeat="my-repeated-grid" at="end end"/>
    ```

    or:

    Copy

    ```
    <fr:repeat-remove-iteration repeat="my-repeated-grid" at="end"/>
    ```

    or:

    Copy

    ```
    <fr:repeat-remove-iteration repeat="my-repeated-grid"/>
    ```
*   Remove the last repetition of the first `my-repeated-grid`:

    Copy

    ```
    <fr:repeat-remove-iteration repeat="my-repeated-grid" at="start end"/>
    ```
*   Remove repetition 2 of the third `my-repeated-grid`:

    Copy

    ```
    <fr:repeat-remove-iteration repeat="my-repeated-grid" at="3 2"/>
    ```

#### Setting the value of a control <a href="#setting-the-value-of-a-control" id="setting-the-value-of-a-control"></a>

Copy

```
<fr:control-setvalue/>
```

`<fr:control-setvalue/>` sets the value of a form control.

| Attribute | Mandatory | Value                                                                  | Comment                                 |
| --------- | --------- | ---------------------------------------------------------------------- | --------------------------------------- |
| `control` | Yes       | control name                                                           |                                         |
| `value`   | Yes       | value expression                                                       | value to set                            |
| `at`      | No        | space-delimited position tokens: `start`, `end`, or a positive integer | missing leading tokens default to `end` |

Copy

```
<fr:control-setvalue
    control="my-control"
    value="current-date()"/>
```

#### Clearing the value of a control <a href="#clearing-the-value-of-a-control" id="clearing-the-value-of-a-control"></a>

\[SINCE Orbeon Forms 2020.1]

Copy

```
<fr:control-clear/>
```

`<fr:control-clear/>` clears the value of a control. For attachment controls, this clears the file but also the file metadata (filename, mediatype, and size).

| Attribute | Mandatory | Value                                                                  | Comment                                 |
| --------- | --------- | ---------------------------------------------------------------------- | --------------------------------------- |
| `control` | Yes       | control name                                                           |                                         |
| `at`      | No        | space-delimited position tokens: `start`, `end`, or a positive integer | missing leading tokens default to `end` |

Example:

Copy

```
<fr:control-clear control="my-control"/>
```

#### Setting the choices of a selection control <a href="#setting-the-choices-of-a-selection-control" id="setting-the-choices-of-a-selection-control"></a>

Copy

```
<fr:control-setitems/>
```

`<fr:control-setitems/>` sets the choices (or "itemset") of a selection control such as a dropdown or radio buttons.

| Attribute | Mandatory | Value                                                                  | Comment                                 |
| --------- | --------- | ---------------------------------------------------------------------- | --------------------------------------- |
| `control` | Yes       | control name                                                           |                                         |
| `items`   | Yes       | XPath expression returning one XPath item for each choice              |                                         |
| `label`   | Yes       | relative XPath expression returning the label for the current choice   |                                         |
| `hint`    | No        | relative XPath expression returning the hint for the current choice    | \[SINCE Orbeon Forms 2020.1]            |
| `value`   | Yes       | relative XPath expression returning the value for the current choice   |                                         |
| `at`      | No        | space-delimited position tokens: `start`, `end`, or a positive integer | missing leading tokens default to `end` |

_NOTE: Hints are only supported for checkboxes and radio buttons as of Orbeon Forms 2020.1._

#### Writing to a dataset <a href="#writing-to-a-dataset" id="writing-to-a-dataset"></a>

Copy

```
<fr:dataset-write
    name="..."/>
```

| Attribute | Mandatory | Value        | Comment |
| --------- | --------- | ------------ | ------- |
| `name`    | Yes       | dataset name |         |

This action takes the latest service result and saves it to the dataset specified by name. Example:

Copy

```
<fr:dataset-write
    name="my-dataset"/>
```

#### Clearing a dataset <a href="#clearing-a-dataset" id="clearing-a-dataset"></a>

\[SINCE Orbeon Forms 2022.1.2]

Copy

```
<fr:dataset-clear
    name="..."/>
```

| Attribute | Mandatory | Value        | Comment |
| --------- | --------- | ------------ | ------- |
| `name`    | Yes       | dataset name |         |

This action clears the content of the dataset with the given name. Specifically, it replaces it with a single anonymous root element (`<_/>`). Example:

Copy

```
<fr:dataset-clear
    name="my-dataset"/>
```

#### Calling a process <a href="#calling-a-process" id="calling-a-process"></a>

\[SINCE Orbeon Forms 2019.1]

Copy

```
<fr:process-call
    scope="oxf.fr.detail.process"
    name="send"/>
```

| Attribute | Mandatory | Value          | Comment |
| --------- | --------- | -------------- | ------- |
| `scope`   | Yes       | property scope |         |
| `name`    | Yes       | process name   |         |

#### Navigating to a page or URL <a href="#navigating-to-a-page-or-url" id="navigating-to-a-page-or-url"></a>

\[SINCE Orbeon Forms 2019.1]

Copy

```
<fr:navigate
    location="https://www.bbc.com/news"/>
```

| Attribute  | Mandatory | Value                                               | Comment                       |
| ---------- | --------- | --------------------------------------------------- | ----------------------------- |
| `location` | Yes       | path or URL                                         |                               |
| `target`   | No        | `_self` or `_blank` or name of the browsing context | where to display the location |

#### Setting the value of an attachment control <a href="#setting-the-value-of-an-attachment-control" id="setting-the-value-of-an-attachment-control"></a>

\[SINCE Orbeon Forms 2019.1]

Copy

```
<fr:control-setattachment
    control="..."/>
```

| Attribute | Mandatory | Value        | Comment |
| --------- | --------- | ------------ | ------- |
| `control` | Yes       | control name |         |

When the response of a service is binary, this action allows setting the value of an attachment control to the content of the service response:

Copy

```
<fr:control-setattachment
    control="my-attachment"/>
```

This supports the following controls:

* `<fr:attachment>`
* `<fr:image-attachment>`

The mediatype and received by the service and the actual size of the attachment are automatically set. However, the filename is not set automatically.

See also the following actions:

* `<fr:control-setfilename>`
* `<fr:control-setmediatype>`
* `<fr:control-setsize>`

#### Setting the filename of an attachment control <a href="#setting-the-filename-of-an-attachment-control" id="setting-the-filename-of-an-attachment-control"></a>

\[SINCE Orbeon Forms 2019.1]

Copy

```
<fr:control-setfilename
    control="..."
    value="..."/>
```

| Attribute | Mandatory | Value            | Comment                      |
| --------- | --------- | ---------------- | ---------------------------- |
| `control` | Yes       | control name     |                              |
| `value`   | Yes       | value expression | value of the filename to set |

This action allows setting the filename of an attachment control. Example:

Copy

```
<fr:control-setfilename
    control="my-attachment"
    value="'My Image.png'"/>
```

This supports the following controls:

* `<fr:attachment>`
* `<fr:image-attachment>`

#### Setting the mediatype of an attachment control <a href="#setting-the-mediatype-of-an-attachment-control" id="setting-the-mediatype-of-an-attachment-control"></a>

\[SINCE Orbeon Forms 2019.1]

Copy

```
<fr:control-setmediatype
    control="..."
    value="..."/>
```

| Attribute | Mandatory | Value            | Comment                       |
| --------- | --------- | ---------------- | ----------------------------- |
| `control` | Yes       | control name     |                               |
| `value`   | Yes       | value expression | value of the mediatype to set |

This action allows setting the mediatype of an attachment control. Example:

Copy

```
<fr:control-setmediatype
    control="my-attachment"
    value="'image/png'"/>
```

This supports the following controls:

* `<fr:attachment>`
* `<fr:image-attachment>`

Note that the `<fr:control-setattachment>` action automatically sets a mediatype.

#### Setting the size of an attachment control <a href="#setting-the-size-of-an-attachment-control" id="setting-the-size-of-an-attachment-control"></a>

[\[SINCE Orbeon Forms 2023.1\]](https://doc.orbeon.com/release-notes/orbeon-forms-2023.1)

Copy

```
<fr:control-setsize
    control="..."
    value="..."/>
```

| Attribute | Mandatory | Value            | Comment                  |
| --------- | --------- | ---------------- | ------------------------ |
| `control` | Yes       | control name     |                          |
| `value`   | Yes       | value expression | value of the size to set |

This action allows setting the mediatype of an attachment control. Example:

Copy

```
<fr:control-setsize
    control="my-attachment"
    value="123"/>
```

This supports the following controls:

* `<fr:attachment>`
* `<fr:image-attachment>`

Note that the `<fr:control-setattachment>` action automatically sets a size.

#### Setting the focus on a form control <a href="#setting-the-focus-on-a-form-control" id="setting-the-focus-on-a-form-control"></a>

\[SINCE Orbeon Forms 2022.1.2]

Copy

```
<fr:control-setfocus
    control="..."/>
```

| Attribute | Mandatory | Value        | Comment |
| --------- | --------- | ------------ | ------- |
| `control` | Yes       | control name |         |

This action allows setting the focus on a form control. Example:

Copy

```
<fr:control-setfocus
    control="my-text-field"/>
```

#### Marking a form control visited or unvisited <a href="#marking-a-form-control-visited-or-unvisited" id="marking-a-form-control-visited-or-unvisited"></a>

\[SINCE Orbeon Forms 2022.1.2]

Copy

```
<fr:control-setvisited
    control="..."
    visited=""/>
```

| Attribute | Mandatory | Value             | Comment            |
| --------- | --------- | ----------------- | ------------------ |
| `control` | Yes       | control name      |                    |
| `visited` | No        | `true` or `false` | defaults to `true` |

This action allows setting whether a form control is visited or not. Example:

Copy

```
<fr:control-setvisited
    control="my-text-field"
    visited="false"/>
```

In Orbeon Forms, a form control can be _visited_ or not. Visited controls have been visited by the user, which means that the user navigated through the form control, possibly without changing its value. One way to visit form controls is to navigate using the "Tab" key, or to click on the form control and then click outside of it. Another way is to use the default "Save" or "Send" buttons, which by default visit all the form controls before proceeding. The notion is used to determine whether to show validation errors associated with that form control. [\[SINCE Orbeon Forms 2023.1\]](https://doc.orbeon.com/release-notes/orbeon-forms-2023.1) Form controls can also be visited when they are calculated, visible, and their value changes.

When working with actions, some form controls can benefit from having their "visited" status reset so that the user doesn't see extra errors appear. The `<fr:control-setvisited>` action allows doing that.

#### Alert for debugging <a href="#alert-for-debugging" id="alert-for-debugging"></a>

\[SINCE Orbeon Forms 2022.1, 2021.1.2, 2020.1.7] The `<fr:alert>` action is intended to be used for debugging, allowing you to get some visibility on the value of intermediary results, or more generally the value of any expression is at a given point. The value of the `message` attribute uses the [AVT syntax](https://doc.orbeon.com/xforms/core/attribute-value-templates), so the example below would show a dialog with the message "The answer is 42".

Copy

```
<fr:alert message="The answer is {40+2}"/>
```

#### Copying control content <a href="#copying-control-content" id="copying-control-content"></a>

[\[SINCE Orbeon Forms 2023.1\]](https://doc.orbeon.com/release-notes/orbeon-forms-2023.1)

**Description**

The `<fr:copy-content>` action allows you to copy form content:

* from non-repeated form controls to other non-repeated form controls
* from non-repeated form controls to repeated form controls
* from repeated form controls to other repeated form controls

See also [Synchronizing repeated content](https://doc.orbeon.com/form-builder/advanced/services-and-actions/synchronize-repeated-content).

**Example**

Assuming that:

* `control-1`, `control-2`, `control-3`, and `control-4` are all within repeated grids or repeated sections
* `non-repeated-control` is non-repeated, and `control-5` is repeated

Then, in the following example, each `<fr:map>` expresses that all repeated controls denoted by the `left` attribute (source) are copied to the corresponding repeated controls denoted by the `right` attribute (destination).

Copy

```
<fr:copy-content warn="true">
    <fr:map left="control-1"            right="control-3"/>
    <fr:map left="control-2"            right="control-4"/>
    <fr:map left="non-repeated-control" right="control-5" right-at="all"/>
</fr:copy-content>
```

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-f61b08c2a47aa981ff4e1277bcfcef57c5c1a36a%252Fcopy-content.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=16cf24074216f3a7858831467fb7a64850ca10f1b415b76a4bee19e790ffa8af)

Example form showing copied content

**Repeated source and destination**

When both the source and destination are repeated, the destination repeat's iterations are enforced, which means:

* Extra iterations in the destination are removed from the end if needed, to match the number of iterations in the source.
* New iterations in the destination are added at the end if needed, to match the number of iterations in the source.
* For each iteration, the values of controls specified with `<fr:map>` are copied over.

Each `<fr:map>` element can individually refer to separate repeated grids and sections. In other words, not all `left` need to be in the same repeated grid or section, and not all `right` need to be in the same repeated grid or section.

**Non-repeated source and destination**

When the source is non-repeated and the destination is also non-repeated, the value of the control is simply copied.

**Non-repeated source and repeated destination**

When the source is non-repeated and the destination is repeated, the value of the control is copied to one or more of the destination controls. The `right-at` attribute can be used to control this behavior:

* `start`: first control only
* `end`: last control only
* strictly positive integer: specific position
* `all`: all controls

**Repeated source and non-repeated destination**

This scenario is not supported yet.

**Warning the user**

When the `warn` attribute is set to `true`, the action will show a warning dialog if the action will overwrite existing content in the destination. An overwrite is considered when:

* The destination has more iterations than the source.
* The destination has the same number of iterations as the source or less, but the action would overwrite a non-blank destination value.

The default is `false`.

### Error handling <a href="#error-handling" id="error-handling"></a>

With Orbeon Forms 2018.2, errors when running services are silently ignored and the action continues.

With Orbeon Forms 2019.1, errors:

1. cause the entire action to stop.
2. run the `oxf.fr.detail.process.action-service-error` process

The default implementation of the service error process is as follows:

Copy

```
<property as="xs:string"  name="oxf.fr.detail.process.action-service-error.*.*">
    error-message("service-error")
</property>
```

You can provide your own service error process in properties-local.xml.

### Evaluation context of XPath expressions <a href="#evaluation-context-of-xpath-expressions" id="evaluation-context-of-xpath-expressions"></a>

The context item used for XPath evaluations is set as follows:

* At the beginning of an action, it is the root element of the form data.
* Within an `<fr:data-iterate>`, and until a service response is available, it is the current iteration item.
* After a service call, whether at the top-level or within an `<fr:data-iterate>`, it is the root element of the preceding action response.

\[SINCE Orbeon Forms 2019.1]

You can explicitly set the XPath evaluation context to the current iteration item within a `<fr:data-iterate>` with the `expression-context` attribute set to `current-iteration`.

In the following example, without the `expression-context="current-iteration"` attribute, the second `<fr:value>` would evaluate within the context of the first service call's response.

Copy

```
<fr:data-iterate ref="/*/row">
    <fr:service-call service="my-first-service">
        <fr:value
            value="foo"
            ref="/some/path"/>
    </fr:service-call>
    <fr:service-call service="my-second-service">
        <fr:value
            expression-context="current-iteration"
            value="bar"
            ref="/some/other/path"/>
    </fr:service-call>
</fr:data-iterate>
```

To be more explicit, the attribute can also be set on the first `<fr:value>`:

Copy

```
<fr:data-iterate ref="/*/row">
    <fr:service-call service="my-first-service">
        <fr:value
            expression-context="current-iteration"
            value="foo"
            ref="/some/path"/>
    </fr:service-call>
    <fr:service-call service="my-second-service">
        <fr:value
            expression-context="current-iteration"
            value="bar"
            ref="/some/other/path"/>
    </fr:service-call>
</fr:data-iterate>
```
