# Including Default Resources in Fragments

You can include images (e.g. `.gif`, `.jpg`, `.jpeg`, or `.png`) in your Fragment Sets for your Fragments to use. Keeping images with your Fragments, rather than in other applications, like Documents and Media, is a convenience. Here you’ll learn how to include image resources in your Fragment Sets and use image resources in your Fragments.

NOTE

For version 7.4+, Fragment Collections are called Fragment Sets in the portal UI.

### [Import a Fragment Set with Resources](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/including-default-resources-with-fragments#import-a-fragment-set-with-resources) <a href="#import-a-fragment-set-with-resources" id="import-a-fragment-set-with-resources"></a>

Start a new portal instance by running

```bash
docker run -it -m 8g -p 8080:8080 liferay/portal:7.4.3.102-ga102
```

Sign in to the portal at http://localhost:8080. Use the email address _test@liferay.com_ and the password _test_. When prompted, change the password to _learn_.

Then, follow these steps to import an example Fragment Set to see how Fragment resources work:

1.  Download and unzip the example resources Fragment Set:

    ```bash
    curl https://resources.learn.liferay.com/dxp/latest/en/site-building/developer-guide/developing-page-fragments/liferay-i6r3.zip -O
    ```

    ```bash
    unzip liferay-i6r3.zip
    ```
2.  Setup up the Fragments Toolkit:

    ```bash
    cd liferay-i6r3
    ```

    ```bash
    ./setup_tutorial.sh
    ```

    Resolve all unmet requirements reported by the script and rerun the script until it reports that your environment is ready.
3.  Import the Fragment Set in the Docker container with the Fragments Toolkit using the `yarn run import` command below.

    ```bash
    yarn run import

    ...
    ? Liferay host & port http://localhost:8080
    ? Username test@liferay.com
    ? Password [hidden]

    Checking connection...
    Connection successful

    ? Company ID liferay.com
    ? Group ID Liferay DXP

    Building project...
    Importing project...

    ✔ Fragment I6R3 Card imported

    Project Imported
    ```
4. Verify that the Fragment Set is available. Point your browser to `https://localhost:8080`, and under the Site Menu on the left side of the screen, go to _Design_ → _Fragments_. The I6R3 Set should appear with the other Fragment Sets.
5. Click on the _I6R3 Set_.
6.  Click the _Resources_ tab. The `books.png` image appears in the resource list.

    ![The resource is available in the Fragment Set.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/including-default-resources-with-fragments/images/01.png)

### Fragment Set Structure with Resources <a href="#fragment-set-structure-with-resources" id="fragment-set-structure-with-resources"></a>

Fragment Sets with resources use the following structure:

* `collection.json`: a text file which describes your Set with the format `{"name":"Set Name","description":"Set description"}`.
* `[fragment-name]/`: contains all of a Fragment’s files.
* `resources/`: contains files available to all the Set’s Fragments.

TIP

Alternatively, you can upload the image through the _Resources_ tab in the Fragments Editor.

Image files are referenced in a Fragment’s HTML with the syntax `[resources:image-name.extension]`. The example Fragment HTML has this `img` element:

```html
<img
    class="card-img-top"
    src="[resources:books.png]"
/>
```

![Embed image resources in fragments.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/including-default-resources-with-fragments/images/02.png)

NOTE

The resource name is case-sensitive and must match the name and case exactly in the reference.

TIP

You can style the image resource in your CSS with the syntax `img[src="[resources:image-name.extension]"]`.

### Include a New Resource <a href="#include-a-new-resource" id="include-a-new-resource"></a>

Now that you know how to include a resource and reference it in a Fragment, you can use a new resource in the example Fragment Set.

1. Copy an image, such as an image from https://www.freeimages.com/, to the `liferay-i6r3.zip/src/i6r3-set/resources/` folder.
2.  Import the Fragment Set in the Docker container again as you did above:

    ```bash
    yarn run import
    ```
3.  Verify that the Fragment Set includes the resource. In the _Fragments_ page in the UI, select the _I6R3 Set_ and click the _Resources_ tab. The new image should appear in the resource listing.

    ![Verify the new image appears in the Set's resource list.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/including-default-resources-with-fragments/images/03.png)
4. Open the _I6R3 Card_ Fragment by selecting the Set’s _Fragments_ tab and clicking the _I6R3 Card_ Fragment. The Fragment source appears in the Fragments Editor.
5.  Update the Fragment to use the new image resource. Delete `books.png` from the `img` element’s `src="[resources:books.png]"` attribute and start typing the new image file’s name after `src="[resources:`. An attribute value with the image file name appears under the cursor. Select that attribute value.

    ![The Fragments Editor lists the matching resources.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/including-default-resources-with-fragments/images/04.png)
6.  Verify that the Fragment renders the new image.

    ![The Fragment should use the new resource image.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/developing-page-fragments/including-default-resources-with-fragments/images/05.png)

NOTE

Any Fragment in a Fragment Set has access to the Set’s resources.

Great! Now you know how to use image resources Fragment Sets.

\
