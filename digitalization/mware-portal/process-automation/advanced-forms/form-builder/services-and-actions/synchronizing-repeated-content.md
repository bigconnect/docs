# Synchronizing repeated content

Often, forms require some kind of master-detail interface, where you first enter a list of initial information, and then complete the information for each item of the list in more details on a separate screen.

A way of doing this with Advanced Forms consists in:

* using a repeated grid for the basic information
* using a repeated section, later in the form, for the details

The difficulty is how to synchronize the two. This is doable with custom XForms code, but it is difficult and error-prone. So Advanced Forms includes a component which does this synchronization automatically for you.

### Configuration <a href="#configuration" id="configuration"></a>

The synchronization component looks like this:

```
<fr:synchronize-repeated-content>
```

It takes the following required XML attributes:

* `left`: the name of the source (or "left") repeated grid or repeated section
* `right`: the name of the destination (or "right") repeated grid or repeated section
* `sync-on-form-load` (optional): can be set to `none` to disable right grid synchronization based on left grid on form load; disabling synchronization on load is a way to support having multiple lefts defined to synchronize with a single right, but only one left doing so at any given time, e.g. because the other lefts are irrelevant

Names are as you see them in the Form Builder user interface with the "Grid Settings" or "Section Settings" dialogs.

In addition, it can automatically copy the value of controls from the source to the destination. You specify this with nested `<fr:map>` elements. They take the following required XML attributes:

* `left`: the name of the source (or "left") control
* `right`: the name of the destination (or "right") control

Names are as you see them in the Form Builder user interface with the "Control Settings" dialog.

### How synchronization works <a href="#how-synchronization-works" id="how-synchronization-works"></a>

The synchronization takes place from "left" to "right":

* Upon form initialization, unless you specified `sync-on-form-load="none"` (see above), the iterations and values are enforced, which means:
  * Extra iterations in the destination are removed from the end if needed, to match the number of iterations in the source.
  * New iterations in the destination are added at the end if needed, to match the number of iterations in the source.
  * For each iteration, the values of controls specified with `<fr:map>` are copied over.
* When iterations are added in the source:
  * Corresponding iterations are added in the destination, at the same position.
* When iterations are moved in the source:
  * Corresponding iterations are moved in the destination.
* When iterations are removed in the source:
  * Corresponding iterations are removed in the destination.
* When values of controls specified by `<fr:map>` change in the source:
  * The values of the matching controls in the destination are updated.

### Full example <a href="#full-example" id="full-example"></a>

The following example does the following:

* Synchronize repeated iterations between repeated grid `master-grid` and repeated section `detail-section`.
* For each iteration, copy over the value of the `traveler-name` and `traveler-birth-date` form controls.

```
    <!-- other Form Builder code here -->

    <fr:synchronize-repeated-content left="master-grid" right="detail-section" version="2018.2">
        <fr:map left="traveler-name"       right="traveler-name-readonly"/>
        <fr:map left="traveler-birth-date" right="traveler-birth-date-readonly"/>
    </fr:synchronize-repeated-content>
    
    <!-- Put `<fr:synchronize-repeated-content>` just above this. -->
</xf:model>
```

The following screenshots show how the form looks like when using the wizard view:

![Master view](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-53af089f0e36e43d054ffaa2f0f4d8ab1f4cfc28%252Fsync-master.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=005715b9094e0fc0081a162db1867c6aa667308735d9974c73b665c5a2f82820)



![Detail view](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-ea3a35108e751fffcf9f44138d9850e9ba2a30e0%252Fsync-detail.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=e5b2a6bc2ccc55be0a93ba950b709f09302b6fb9665b28dfb4bb52f427d52005)



### Paging large repeated sections <a href="#paging-large-repeated-sections" id="paging-large-repeated-sections"></a>

When paging of large repeated sections is enabled, optionally with a repetition label, each master repetition appears as its own wizard page. This reduces the size of wizard pages and makes it easier to fill out a form.

![Detail view](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-952a3dda32698896d29a569b9a33d5284e886d9a%252Fsync-detail-paging-alice.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=6f64f203ca211d008203d77e17dfb58fac909848580ce227e9fffe18aaa28e79)



![Detail view](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-4c86ccc8a57d6c93178e156181905573e877c19c%252Fsync-detail-paging-bob.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=ce0932880e3aa79fe4f656ddf6cc93db95b6a13adb6c3548e3c0b5f65ea3b98a)

