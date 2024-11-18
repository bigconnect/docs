# Embedded app

Websites will often have embedded content, for example videos from another platform such as YouTube.

This is done through an `iframe`, which is simply a HTML element that contains another HTML document inside.

You can embed an entire published application within your webpages.

> ### 📘Permissions
>
> Only application Screens with **Public** Access level can be viewed within an embedded app.
>
> You must also Manage access for any tables being used to have **Public** Read/Write access as required.

### Getting the iframe snippet

To get the embed code snippet, click on the `Settings` tab within your app, and then click `Embed`.

Here you can see and copy the iframe contents.

<figure><img src="https://files.readme.io/d8b063e-Screenshot_2023-06-28_at_09.58.17.png" alt="Note: Embeds will only work with a published app"><figcaption><p>Note: Embeds will only work with a published app</p></figcaption></figure>

**Publishing your app**

You can publish your app by clicking the drop-down in the top-right corner of the screen, and clicking `Publish`.

<figure><img src="https://files.readme.io/164138c-publish.png" alt="Published apps support embedding"><figcaption><p>Published apps support embedding</p></figcaption></figure>

Once you app is published, you will see a new button appear: `</> Embed`. Clicking this button will take you to the Embed settings section.

<figure><img src="https://files.readme.io/6ea4755-copy_code.png" alt="Click the &#x27;Copy code&#x27; to quick copy the iframe contents"><figcaption><p>Click the 'Copy code' button to quick copy the iframe contents</p></figcaption></figure>

### Settings

The iframe has a number of attributes that can be changed. We recommend that you only change the _width_, _height_ and _frameborder_ properties as changing the allow permissions could affect the working of your app.

> ### 🚧Building an embedded app for mobile
>
> If you are [Designing for mobile](https://docs.budibase.com/docs/designing-for-mobile) then be aware that the `{{ Device.Mobile }}` and `{{ Device.Tablet }}` Bindings will always be **false** regardless of the device the embedded app is being viewed on.\
> This is because the iframe has no context of the parent window.

\


### Creating widgets

You may find that you don't need to embed a fully fledged multi-screen app. Perhaps you want to embed a single Table or Chart for example, to display on your webpage.

In this case you will want to hide the navigation links, as well as the app title and logo.

To do this, click on your app screen and in the Settings Panel toggle off `Show nav`. Remember that the access level of the screen should also be public! Publish your app for changes to take effect.

<figure><img src="https://files.readme.io/5aa6655-Screenshot_2023-06-28_at_11.12.55.png" alt="Turning app navigation off"><figcaption><p>Turning app navigation off</p></figcaption></figure>

<figure><img src="https://files.readme.io/36ccbd9-Screenshot_2023-06-28_at_11.17.55.png" alt="The &#x27;Clients&#x27; table view can now be embedded as an inline widget"><figcaption><p>The 'Clients' table view can now be embedded as an inline widget</p></figcaption></figure>



### Embed component

The feature of embedding an app as an iframe should not be confused with the Embed Component within the **Design** section.

A common use-case for the embed component is to add Global styling to your app screens. You could however also use it to embed iframes, including another app!

<figure><img src="https://files.readme.io/bc1eada-adding_embed.png" alt="Adding an Embed component"><figcaption><p>Adding an Embed component</p></figcaption></figure>

<figure><img src="https://files.readme.io/0f307d9-Screenshot_2023-06-28_at_11.25.51.png" alt="Adding an iframe to the Embed component - in this case another Budibase app!"><figcaption><p>Adding an iframe to the Embed component - in this case another app!</p></figcaption></figure>
