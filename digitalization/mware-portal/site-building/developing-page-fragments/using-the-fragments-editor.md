# Using the Fragments Editor

Liferay DXP includes a built-in [editor](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/page-fragment-editor-interface-reference) for developing content page fragments. To access the editor, open the _Site Menu_ ( ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Design_ → _Fragments_. From here, you can view, manage, and create fragments and fragment sets.

* [Creating a Fragment Set](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor#creating-a-fragment-set)
* [Creating a New Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor#creating-a-new-fragment)

NOTE

For Liferay DXP 7.4+, Fragment Collections are called Fragment Sets in the Liferay UI.

### [Creating a Fragment Set](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor#creating-a-fragment-set) <a href="#creating-a-fragment-set" id="creating-a-fragment-set"></a>

Before creating new fragments, you must create a new fragment set to contain them:

1.  Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Design_ → _Fragments_.

    In Liferay DXP 7.1 and 7.2, open the _Product Menu_, and go to _Site_ → _Site Builder_ → _Page Fragments_.
2.  Click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-plus.png) ) next to Fragment Sets.

    ![Click the add button next to Fragment Sets.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor/images/01.png)
3.  Enter a _name_ and _description_ for the new set.

    You can create sets that group fragments by functionality or by teams and departments, for example.
4. Click _Save_.

Once saved, you can add fragments to the set.

### [Creating a New Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor#creating-a-new-fragment) <a href="#creating-a-new-fragment" id="creating-a-new-fragment"></a>

IMPORTANT

To create a React fragment, use the [Fragments Toolkit](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/using-the-fragments-toolkit).

Follow these steps to create a fragment:

1.  Go to the desired fragment set and click _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to create a new fragment.

    NOTE

    Prior to Liferay DXP 7.3, a fragment could either be a Section or a Component. In Liferay DXP 7.3+, all page fragments are components.
2. Choose a fragment type, either Basic or [Form](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/creating-form-fragments), and click _Next_.
3. Enter a name for the fragment and click _Add_.
4.  In the Code tab, use the HTML, CSS, and JavaScript fields to add your fragment’s resources. Here, you can also see a live preview of your fragment’s appearance in different device contexts.

    Here are some HTML editing shortcuts:

    * Enter an open angle bracket (`<`) to access standard HTML tags and Liferay’s fragment-specific tags.
    * Start an element attribute with `data` to access Liferay’s [editable fragment attributes](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/reference/fragments/fragment-specific-tags-reference).

    The example below adds a card component with editable text:

    ```html
    <div class="marketing-card-fragment-01">
      <div class="card">
        <data-lfr-editable id="01-card-image" type="image">
          <img src="https://cdn.dribbble.com/users/1408464/screenshots/9323535/media/a5b9a76256562e878ecc6dc5cd0fadf0.png" class="card-img-top" alt="2020 - Try New Things">
        </data-lfr-editable>
        <div class="card-body">
          <data-lfr-editable id="02-card-title" type="rich-text">
            <h5 class="card-title">Editable Card title</h5>
          </data-lfr-editable>
          <data-lfr-editable id="03-card-text" type="rich-text">
            <p class="card-text">Here is some editable text.</p>
          </data-lfr-editable>
          <data-lfr-editable id="04-card-link" type="link">
            <a href="#" class="btn btn-primary">Editable link</a>
          </data-lfr-editable>
        </div>
      </div>
    </div>
    ```

    ```css
    .marketing-card-fragment-01 .card img {
      max-width: 100%;
    }
    ```

    ![Add HTML, CSS, and Javascript resources to the fragment and see a live preview.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor/images/02.png)
5.  In the Configuration tab, use the `JSON` field to add [configuration options](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/adding-configuration-options-to-fragments) to a page fragment.

    ![Add configuration options to a page fragment.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor/images/03.png)

    TIP

    You can add a `configurationRole` value to any field set (alongside the `fields` object in the JSON) to specify which tab the associated fields appear in. Set the value as `styles` to make the fields appear in the Styles tab, or set it to `advanced` to make them appear in the Advanced tab (_only in Liferay DXP versions U23+ or GA23+_). If no `configurationRole` is set, then they appear in the General tab by default.
6.  Click _Publish_ to save your fragment and make it available for use in [content pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/understanding-pages/understanding-pages#content-pages).

    ![The fragment can be used in content pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/using-the-fragments-editor/images/04.png)

While developing a fragment, changes are saved automatically as a draft until your fragment is published. Once added to a set, you can copy, export, edit, and remove a fragment at any time in the fragments editor. See [Managing Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/managing-fragments) for more information on available actions for page fragments.

NOTE

Since Liferay DXP 7.2 SP1+ and Liferay Portal 7.2 GA2+, you can create page fragments in the _Global_ site to make them available for all sites. To expose this feature in the initial releases of these versions, you must create a `.config` file named `com.liferay.fragment.web.internal.configuration.FragmentGlobalPanelAppConfiguration.config` and add the `enabled=B"true"` property. Then copy it to your Liferay DXP instance’s `osgi/configs` folder. Global page fragments are inherited by child sites and can only be edited from the Global site. Any resources the Global page fragments references from the Global site are copied to a site that leverages the page fragment.

\
