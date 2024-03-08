# Adding Configuration Options to Fragments

Configurable options help make your Fragments flexible, so you don’t have to maintain many similar Fragments. For example, instead of having one Fragment that has a heading with style A and another Fragment that has a heading with style B, you can create one Fragment that has a configurable style for the heading with options for style A and B. Here you’ll learn how to add configuration options to a Fragment:

* [Deploy a Configurable Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments#deploy-a-configurable-fragment)
* [Examine the Configuration](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments#examine-the-configuration)
* [Modify the Configuration](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments#modify-the-configuration)
* [Propagate the Changes and Test](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments#propagate-the-changes-and-test)
* [Related Information](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments#related-information)

NOTE

For Liferay DXP 7.4+, Fragment Collections are called Fragment Sets in the Liferay UI.

### [Deploy a Configurable Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments#deploy-a-configurable-fragment) <a href="#deploy-a-configurable-fragment" id="deploy-a-configurable-fragment"></a>

Start a new Liferay instance by running

```bash
docker run -it -m 8g -p 8080:8080 liferay/portal:7.4.3.102-ga102
```

Sign in to Liferay at [http://localhost:8080](http://localhost:8080/). Use the email address _test@liferay.com_ and the password _test_. When prompted, change the password to _learn_.

Then, follow these steps to deploy an example to see how Fragment configuration options work:

1.  Download and unzip the [example Fragment Set](https://resources.learn.liferay.com/dxp/latest/en/site-building/developer-guide/developing-page-fragments/liferay-c7f8.zip):

    ```bash
    curl https://resources.learn.liferay.com/dxp/latest/en/site-building/developer-guide/developing-page-fragments/liferay-c7f8.zip -O
    ```

    ```bash
    unzip liferay-c7f8.zip
    ```
2.  Set up the Fragments Toolkit:

    ```bash
    cd liferay-c7f8
    ```

    ```bash
    ./setup_tutorial.sh
    ```
3.  Import the Fragment Collection to the Docker container using the Fragments Toolkit. Alternatively, you can [import the Fragment manually](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/managing-fragments).

    ```bash
    yarn run import

    > yo liferay-fragments:import

    ? Liferay host & port http://localhost:8080
    ? Username test@liferay.com
    ? Password [hidden]
    Checking connection...
    Connection successful

    ? Company ID liferay.com
    ? Group ID Liferay
    Building project...
    Importing project...
    ✔ Fragment C7F8 Card imported
    Project imported
    ```
4.  Verify the Fragment Set is available. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Design_ → _Fragments_. The Set should appear in the list.

    ![Verify the Set is available.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments/images/01.png)

    NOTE

    For Liferay DXP 7.1 and 7.2, instead navigate to _Site_ → _Site Builder_ → _Page Fragments_ under the Product Menu to get to the Fragments page.
5. Go to a Content Page and click the _Edit_ icon (![Edit icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit-pencil.png)) to begin editing.
6. Expand the _C7F8 Set_ heading in the [Fragments and Widgets panel](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages/content-page-editor-ui-reference#fragments-and-widgets) and drag the _C7F8 Card_ Fragment onto the page.
7.  Select the _C7F8 Card_ on the page to open the Fragment Configuration Menu. In the _General_ tab, set the component’s text style to dark or light.

    ![Configurable Fragments provide options to modify the Fragment's look and feel.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments/images/02.png)

Great! You successfully imported and configured a configurable Fragment.

### [Examine the Configuration](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments#examine-the-configuration) <a href="#examine-the-configuration" id="examine-the-configuration"></a>

You can edit a Fragment’s configuration options in Liferay’s built-in [Fragments Editor](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor) or in a text file.

**Using the Fragments Editor:** In the UI, edit the Fragment with the [Fragments Editor](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor) and click _Configuration_ tab. The Fragment’s configuration appears in the editor.

**Editing a Fragment Configuration File:** If you don’t have the Fragment files, export them from the Fragment by opening the Fragment’s actions menu and clicking _Export_. The `configurationPath` field (optional) in a Fragment’s `fragment.json` file specifies the configuration `.json` file. If it doesn’t have a configuration file, create one and set the `configurationPath` field to the configuration file name.

Open the example’s `fragment.json` file to determine the Fragment’s configuration file.

```json
{
    "configurationPath": "index.json",
    "cssPath": "index.css",
    "htmlPath": "index.html",
    "jsPath": "index.js",
    "name": "C7F8 Card",
    "type": "component"
}
```

The `"configurationPath": "index.json"` property indicates this Fragment’s configuration file is `index.json`.

Open the configuration file.

```json
{
    "fieldSets": [
        {
            "fields": [
                {
                    "dataType": "string",
                    "defaultValue": "dark",
                    "label": "Text Style",
                    "name": "c7f8TextStyle",
                    "type": "select",
                    "typeOptions": {
                        "validValues": [
                            {
                                "value": "dark"
                            },
                            {
                                "value": "light"
                            }
                        ]
                    }
                }
            ],
            "label": "C7F8"
        }
    ]
}
```

The configuration file above, specifies a selector for applying a _dark_ or _light_ text style to the Fragment. The configuration has a field named `c7f8TextStyle`. The field’s `type` is `select`, which makes it a selector component. See [Fragment Configuration Types](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-configuration-types-reference) for details. The selector’s valid values are `dark` or `light`. The `"dataType": "string"` property means that the selector operates on string data.

This selector is a part of a field set labeled _C7F8_ (the label is optional). In the UI, this selector is found in the Fragment’s configuration section labeled _C7F8_. The selector is labeled `Text Style` per the field’s `label` property. The figure below shows the selector.

![Here is the selector for the C7F8 Card Fragment](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments/images/03.png)

WARNING

The Fragments Editor won’t save the configuration until it’s valid. Make sure your JSON configuration is valid before previewing it.

The configuration values selected by the user are made available to the HTML through the FreeMarker context. They are referenced in the HTML with the notation `${configuration.fieldName}`. The example (`${configuration.textAppliedStyle}`) returns `dark` or `light` depending on the configuration value selected by the user, setting the CSS class to `text-light` or `text-dark`:

```html
<div class="component-c7f8-card">
    <div class="card">
        <img
            class="card-img-top"
            data-lfr-editable-id="01-image"
            data-lfr-editable-type="image"
            src="https://clayui.com/images/home/lexicon_symbol.svg"
        />

        <div class="card-body">
            <h5
                class="card-title text-${configuration.c7f8TextStyle}"
                data-lfr-editable-id="02-title"
                data-lfr-editable-type="rich-text"
            >
                Editable Card Title
            </h5>

            <p
                class="card-text text-${configuration.c7f8TextStyle}"
                data-lfr-editable-id="03-text"
                data-lfr-editable-type="rich-text"
            >
                Here is some editable text.
            </p>

            <a
                class="btn btn-primary"
                data-lfr-editable-id="04-label"
                data-lfr-editable-type="link"
                href="#"
            >
                Editable Link
            </a>
        </div>
    </div>
</div>
```

The example demonstrates a select configuration. See the [Configuration Types Reference](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-configuration-types-reference) for a complete list of the available Fragment configuration types.

### [Modify the Configuration](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments#modify-the-configuration) <a href="#modify-the-configuration" id="modify-the-configuration"></a>

Now that you know how the configuration works, you can modify it.

1.  Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Design_ → _Fragments_.

    NOTE

    For Liferay DXP 7.1 and 7.2, instead navigate to _Site_ → _Site Builder_ → _Page Fragments_ under the Product Menu to get to the Fragments page.
2. Select the _C7F8 Set_, click the _Actions_ button (![Actions Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the C7F8 Card and select _Edit_. This opens the Fragments Editor.
3.  Click the _Configuration_ tab and update the configuration with a checkbox field to hide/show the card’s description. Insert this code on a new line after the `c7f8TextStyle` field’s closing brace and comma (`},`)

    ```json
    {
        "name": "showDescription",
        "label": "Show Description",
        "description": "show-description",
        "type": "checkbox",
        "defaultValue": true
    }
    ```

    TIP

    You can add a `configurationRole` value to the field set (alongside the `fields` object in the JSON) to specify which tab the associated fields appear in. Set the value as `styles` to make the fields appear in the Styles tab, or set it to `advanced` to make them appear in the Advanced tab (_only in Liferay DXP versions U23+ or GA23+_). If no `configurationRole` is set, then they appear in the General tab by default.
4.  Go back to the _HTML_ pane in the _Code_ tab and wrap the paragraph element with a conditional statement to check for the checkbox’s value. Click _Publish_ to apply the changes.

    ```html
    [#if configuration.showDescription]
      <p data-lfr-editable-id="03-card-text" data-lfr-editable-type="rich-text"
      class="card-text text-${configuration.textAppliedStyle}">
        Here is some editable text.
      </p>
    [/#if]
    ```

NOTE

You can also access the configuration’s value through the JavaScript with the syntax `const configurationValue = configuration.textAppliedStyle;`.

### [Propagate the Changes and Test](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments#propagate-the-changes-and-test) <a href="#propagate-the-changes-and-test" id="propagate-the-changes-and-test"></a>

Now you can test the updates.

1. Propagate the changes so they’re reflected on the Content Page. Click _Actions_ (![Action Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the C7F8 Card and select _View Usages_.
2.  Check the box for the Content Page and click the (![propagate button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-propagate.png)) button.

    ![Configurable Fragments provide options to modify the Fragment's look and feel.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments/images/04.png)
3. Go back to the Content Page and once again click the (![Edit icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit-pencil.png)) icon to edit the Content Page.
4. Select the _C7F8 Card_ again to show Selection panel on the right.
5.  In the _General_ tab, check/uncheck the _Show Description_ checkbox to show/hide the card’s text.

    ![You can have as many configuration options as you want for your Fragments.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments/images/05.png)

Great! Now you know how to add configuration options to your Fragments. For more configuration examples, please see Please see [Fragment Configuration Types](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-configuration-types-reference).

\
