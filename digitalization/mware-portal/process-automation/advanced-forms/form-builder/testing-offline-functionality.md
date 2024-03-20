# Testing offline functionality

Since its inception, Advanced Forms has had a hybrid architecture for forms:

* the user interface runs in the browser, implemented in JavaScript (and more recently Scala.js)
* the form's logic and validations runs on the server

This architecture has benefits, such as protecting the confidentiality of internal data that never leaves the server.

### Preview <a href="#preview" id="preview"></a>

Advanced Forms allows testing that most aspects of a form work in . From Form Builder, simply use the new "Test Offline" button to see whether your form operates and renders properly in this new mode.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-e47062bb373dbcaebecd69254986970d99ae9947%252Ftest-offline-button.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=dd1bc43f2c25fb0a501912d40b6dd3a766344652083d6aedd4d09848fe985dcf)

The "Test Offline" button

When you do this:

* The form definition edited in Form Builder is compiled to a serialized representation.
* The JavaScript-based form runtime is loaded in the Form Builder test window, loads the compiled form, and renders it.

From the user's perspective, this works almost exactly like the "Test" button which has always been present in Form Builder.
