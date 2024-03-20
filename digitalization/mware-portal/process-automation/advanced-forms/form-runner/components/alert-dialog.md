# Alert dialog

The alert dialog allows you to build simple dialogs for those cases where you need to inform users of something that happened, or ask a question which can be answered by a _yes_ or a _no_ .

### Example <a href="#example" id="example"></a>

Let's assume that you want to ask a yes/no question which produces a dialog as shown below, and that you want to react to users choosing _Yes_ .

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0X7izqx\_znmcp7J%252Fxbl-alert-dialog.png%3Fgeneration=1528743344023400%26alt=media\&width=768\&dpr=4\&quality=100\&sign=1cccae46bf5c7e1894beacc2b12f7da006e725baefcc5aa4728b1ada997a125f)

First declare the dialog, with:

```
<fr:alert-dialog id="overwrite-file-dialog">
    <fr:label>Copy</fr:label>
    <fr:message>
        An item with the same name already exists.
        Do you want to replace it with the one you're moving?
    </fr:message>
    <fr:negative-choice/>
    <fr:positive-choice>
        <xf:toggle event="DOMActivate" case="replaced-case"/>
    </fr:positive-choice>
</fr:alert-dialog>
```

1. You gave an id to the dialog (`overwrite-file-dialog`).
2. You defined what the dialog title and message are with `<fr:label>` and `<fr:message>`.
3. You use the `<fr:negative-choice>` and `<fr:positive-choice>` elements to indicate which buttons you want to have, and in which order they should be shown.
4. You placed an event handler inside the `<fr:positive-choice``>`, which will run when/if users press the _Yes_ button.

You open this dialog by dispatching the event `fr-show` to the dialog:

```
<xf:dispatch target="overwrite-file-dialog" name="fr-show"/>
```

### Buttons <a href="#buttons" id="buttons"></a>

The alert dialog supports 3 buttons, that correspond to a _positive choice_ , a _negative choice_ , and a _neutral choice_ . Each one has a default label and a default icon, as shown in the table below. For each button, you can:

*   Override the label by placing your own `<fr:label>` inside the button element. For instance, you would change the label for positive choice from _Yes_ to _OK_ and for the negative choice from _No_ to _Cancel_ with:

    Copy

    ```
      <fr:positive-choice><fr:label>OK</fr:label></fr:positive-choice>
      <fr:negative-choice><fr:label>Cancel</fr:label></fr:negative-choice>
    ```
*   Override the icon shown inside the button with CSS. You will find an `<xhtml:span>` with the class indicated in the table below around each button, which allows you to have a different style depending on the button type. For instance, if you can use the following CSS rule to remove the icon for the positive choice button:

    Copy

    ```
      .yui-skin-sam .xbl-fr-alert-dialog-positive .xbl-fr-button .yui-button button
          { background: none; padding: 0 10px 0 10px; }
    ```

| Element                | Default representation                                                                                                                                                                                                                                                                                                                                                                                     | CSS class                      |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| `<fr:positive-choice>` | ![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0XI6lWHU9ze-K7I%252Fxbl-alert-dialog-yes.png%3Fgeneration=1528743338278671%26alt=media\&width=300\&dpr=4\&quality=100\&sign=715f9bc8f622d006b239cbb119a599c991cff9b37fa57c419382b2e6585e1683) | `xbl-fr-alert-dialog-positive` |
| `<fr:negative-choice>` | ![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0XKVoJgYr9sgJth%252Fxbl-alert-dialog-no.png%3Fgeneration=1528743356602222%26alt=media\&width=300\&dpr=4\&quality=100\&sign=8e33f9486f869b4bd6b9455651b7932a0a51bae5560ee499d5fb3996c7ceb5c6)  | `xbl-fr-alert-dialog-negative` |
| `<fr:neutral-choice>`  | ![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0XMmgJBuLqmJRcf%252Fxbl-alert-dialog-ok.png%3Fgeneration=1528743344787991%26alt=media\&width=300\&dpr=4\&quality=100\&sign=5c874585f303c5681d286cd385ec5c8d61c2d608a6e77a044b54aeafe6a1d29c)  | `xbl-fr-alert-dialog-neutral`  |

In most cases, you will be using either the positive and negative choice together, or the neutral choice alone. You can use the 3 buttons at the same time, to create a _Yes-No-Cancel_ type of dialog. In this case however, you should consider giving labels to buttons that more description than _Yes-No-Cancel_ . For instance, if you want to ask users whether they want to leave this form without saving, you could label the buttons _Save data_ , _Discard data_ , _Continue editing_ .

### Actions <a href="#actions" id="actions"></a>

You specify the actions running upon a button being pressed by either:

* **Use inline actions** – Placing the actions inside the `<fr:``positive-choice``>` or or , and run them on the `DOMActivate` event, as done in the example above.
*   **Use a "callback"** – When opening the alert dialog, passing the id of an element to which the dialog will dispatch an event upon a button being pressed. For instance you would open the dialog with:

    Copy

    ```
      <xf:trigger id="delete-trigger">
          <xf:label>Delete</xf:label>
          <xf:dispatch target="confirm-deletion-dialog" name="fr-show">
              <xf:property name="positive-targetid" value="'delete-trigger'"/>
          </xf:dispatch>
          <xf:action event="fr-positive">
              <!-- Operation to perform upon delete -->
          </xf:action>
      </xf:trigger>
    ```

_You pass the element id using the context property named_ `positive-targetid`_,_ `negative-targetid`_, or_ `neutral-targetid`_. Then, the alert dialog dispatches an event named_ `fr-positive`_, or_ `fr-negative`_, or_ `fr-neutral` _to that element. This second technique is particularly useful when you want to have one instance of an alert dialog that you want to reuse from the multiple places in your form._

_**Keeping context information** – When opening the dialog you can pass an optional context property named_ `context`_; the value of this property must be a string, and it will be passed back to your handler for_ `fr-positive`_, or_ `fr-negative`_, or_ `fr-neutral`_. This is particularly useful when you want to "forward" an event property to your custom handler. For instance you can open the dialog with:_

```
<xf:dispatch target="confirm-deletion-dialog" name="fr-show">
    <xf:property name="positive-targetid" value="'delete-trigger'"/>
    <xf:property name="context" value="event('patient-condition')"/>
</xf:dispatch>
```

_And then in your handler access the value you originally passed, as in:_

```
<xf:action event="fr-positive">
    <xf:delete ref="instance('patient-conditions')/condition[@id = event('context')]"/>
</xf:action>
```

### Dynamic message <a href="#dynamic-message" id="dynamic-message"></a>

Instead of specifying an inline message with `fr:message`, you can pass it dynamically when opening the dialog:

```
<xf:dispatch target="confirm-deletion-dialog" name="fr-show">
    <xf:property name="message" value="'Are you sure you want to proceed?'"/>
</xf:dispatch>
```

### Dialog icon <a href="#dialog-icon" id="dialog-icon"></a>

Depending on the number of buttons you place in your dialog, a different icon is used next to the message. Just like icons in buttons, you can change the icon by overriding the default CSS:

*   When you have 2 or 3 buttons, the dialog is considered to be a **question**. The CSS class is `xbl-fr-alert-dialog-question` and the dialog looks like:

    ![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0X7izqx\_znmcp7J%252Fxbl-alert-dialog.png%3Fgeneration=1528743344023400%26alt=media\&width=300\&dpr=4\&quality=100\&sign=1cccae46bf5c7e1894beacc2b12f7da006e725baefcc5aa4728b1ada997a125f)
*   When you have 1 button, the dialog is considered to be **informational**. The CSS class is `xbl-fr-alert-dialog-info` and the dialog looks like:

    ![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC0Y8RceDHbvHxHKT%252Fxbl-alert-dialog-info.png%3Fgeneration=1528743347567067%26alt=media\&width=300\&dpr=4\&quality=100\&sign=63831eb3e4e828a88ec3514fa99387684e569ce3816a3e1c4ab76a8bb57f47c1)
