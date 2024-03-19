# Client-side error handling

### Disabling the standard error dialog <a href="#disabling-the-standard-error-dialog" id="disabling-the-standard-error-dialog"></a>

By default, when an Ajax error happens, Advanced Forms shows users an error dialog.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC3H3NSL2hoPXaakE%252Ferror-dialog.png%3Fgeneration=1528743348818856%26alt=media\&width=768\&dpr=4\&quality=100\&sign=4389535bc3cf353fd414b07c012e9b03f5c35ffb19b860c317a69d111934aa1a)

You can disable this behavior by adding this property to your `properties-local.xml`:

```
<property
    as="xs:boolean"
    name="oxf.xforms.show-error-dialog"
    value="false"/>
```

### JavaScript event handler <a href="#javascript-event-handler" id="javascript-event-handler"></a>

Advanced Forms exposes a custom JavaScript event: `FORMS.xforms.Events.errorEvent`.

By default a dialog is shown to the user when an error is intercepted. If you prefer to show your own dialog or to implement some other behavior in case of error, most likely you will want to:

* Disable the default error dialog by setting the `oxf.xforms.show-error-dialog` property to `false`.
* Register your own listener on `FORMS.xforms.Events.errorEvent`.

You can register your own listener on that event, and when fired, send users to a page you choose, as done in the following snippet, which sends users to the Advanced home page:

```
FORMS.xforms.Events.errorEvent.subscribe(function(eventName, eventData) {
    // your code here
});
```

#### Example <a href="#example" id="example"></a>

In case the user session expires, or some other error happens, you would like to redirect them a page you created that will, for instance, tell users to log in and try again, and if the problem persists to contact customer support.

```
FORMS.xforms.Events.errorEvent.subscribe(function(eventName, eventData) {
    window.location.href = "http://www.example.org/";
});
```

### Providing your own dialog <a href="#providing-your-own-dialog" id="providing-your-own-dialog"></a>

#### File location <a href="#file-location" id="file-location"></a>

By default, the source markup for this dialog is available under:

```
oxf:/config/error-dialog.xml
```

This file is located inside the following jar file:

* `WEB-INF/lib/forms-core.jar`

You can override it in two ways:

* Globally, by placing your own `error-dialog.xml` file under `WEB-INF/resources/config`.
* Per application, by placing your own `error-dialog.xml` file under `WEB-INF/resources/apps/$app`, where `$app` stands for the name of your application.

#### Localization <a href="#localization" id="localization"></a>

Should you need to provide multiple versions of the error dialog in different languages, you can do so by having multiple `<div class="xforms-error-panel">` each with its own `lang` attribute (see below for the full structure of the HTML you need to provide). Advanced Forms will try to get the error panel with the `lang` attribute matching the current language, and if it can't find one it will pick the first error panel.

#### Example: default dialog <a href="#example-default-dialog" id="example-default-dialog"></a>

The default dialog provides classes allowing for opening/closing a details section:

```
<div xmlns="http://www.w3.org/1999/xhtml" class="xforms-error-dialogs">
    <div class="xforms-error-panel xforms-initially-hidden" role="dialog" aria-labelledby="error-dialog-title">
        <div class="hd" id="error-dialog-title">An error has occurred</div>
        <div class="bd">
            <p>
                You may want to try one of the following:
            </p>
            <ul>
                <li><a class="xforms-error-panel-close">Close this dialog</a> and continue to use this page.</li>
                <li><a class="xforms-error-panel-reload">Reload this page</a>. Note that you will lose any unsaved changes.</li>
                <li>
                    <p>
                        If the above does not work, try reloading the page yourself. Note that you will lose any unsaved changes:
                    </p>
                    <ul>
                        <li>
                            With Firefox: hold down the <code>shift</code> key and click the Reload button in your browser toolbar.
                        </li>
                        <li>
                            With Safari and Chrome: click the Reload button in your browser toolbar.
                        </li>
                        <li>
                            With Internet Explorer: hold down the <code>control</code> key and click the Reload button in your browser toolbar.
                        </li>
                    </ul>
                </li>
                <li>Return <a href="/">home</a>.</li>
            </ul>
            <div class="xforms-error-panel-details-hidden">
                <p>
                    <a class="xforms-error-panel-show-details">
                        <img src="/ops/images/xforms/section-closed.png" alt="Show Details"/>
                        <span>Show details</span>
                    </a>
                </p>
            </div>
            <div class="xforms-error-panel-details-shown xforms-disabled">
                <p>
                    <a class="xforms-error-panel-hide-details">
                        <img src="/ops/images/xforms/section-opened.png" alt="Hide Details"/>
                        <span>Hide details</span>
                    </a>
                </p>
                <div class="xforms-error-panel-details"/>
            </div>
        </div>
    </div>
    <div class="xforms-login-detected-dialog modal hide fade" tabindex="-1" role="dialog" aria-hidden="true">
        <div class="modal-header">
            <h4>Reloading form</h4>
        </div>
        <div class="modal-body">
            <p>
                This form has to be reloaded. This most likely happened because your session has expired, which might
                take to the login page. (If you think that you shouldn't see this message and that the problem persists,
                please contact support.)
            </p>
        </div>
        <div class="modal-footer">
            <button class="btn btn-primary">OK</button>
        </div>
    </div>
</div>
```

#### Example: minimal dialog <a href="#example-minimal-dialog" id="example-minimal-dialog"></a>

At a minimum, the file should contain the following structure:

```
<div xmlns="http://www.w3.org/1999/xhtml"
     class="xforms-error-panel xforms-initially-hidden">
    <div class="hd">An error has occurred</div>
    <div class="bd">
        <p>
            Sorry, a serious error has occurred!
        </p>
        <div class="xforms-error-panel-details-hidden xforms-disabled">
            <p>
                <a class="xforms-error-panel-show-details"/>
            </p>
        </div>
        <div class="xforms-error-panel-details-shown xforms-disabled">
            <p>
                <a class="xforms-error-panel-hide-details"/>
            </p>
            <div class="xforms-error-panel-details"/>
        </div>
    </div>
</div>
```

#### CSS configuration <a href="#css-configuration" id="css-configuration"></a>

Advanced developers can configure the appearance of the error dialog. In most cases overriding CSS definitions should be enough.
