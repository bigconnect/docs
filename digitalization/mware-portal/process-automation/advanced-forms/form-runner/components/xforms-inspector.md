# XForms inspector

A lot can happen when you interact with an XForms page: events are dispatched, actions executed, submissions sent, and more. While you can have some visibility on those by enabling [XForms logging](https://doc.orbeon.com/configuration/advanced/xforms-logging) in Orbeon Forms, an important part of the state of an XForms page is simply contained in the various XForms instances that you define in your page.

The XForms Inspector allows you to:

1. See the "live" content of your instances as you interact with the form.
2. Type in XPath expression, and see what the result is when they are evaluated.
3. See what is logged by your form to the XForms inspector console (_experimental_).

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC2Nmdnl9QsVp4rT1%252Fxbl-xforms-inspector.png%3Fgeneration=1528743352493117%26alt=media\&width=768\&dpr=4\&quality=100\&sign=4418d6daa3432bdf5a38c0a71d48bea86309567f852c47a86d4c56e13ac5bec7)

### Configuration <a href="#configuration" id="configuration"></a>

You can enable the XForms Inspector in one of two ways:

*   For all your pages, by setting the following property to `true`:

    Copy

    ```
    <property as="xs:boolean" name="oxf.epilogue.xforms.inspector" value="true"/>
    ```
*   For a specific page, by adding the following code in your XForms: `<fr:xforms-inspector/>`. If you don't have the "fr" prefix in scope, instead of declaring that prefix globally, you can declare it locally:

    Copy

    ```
    <fr:xforms-inspector xmlns:fr="http://orbeon.org/oxf/xml/form-runner"/>
    ```

### The console <a href="#the-console" id="the-console"></a>

#### The fr-log event <a href="#the-fr-log-event" id="the-fr-log-event"></a>

The console is an experimental new feature, and is likely to be improved and changed in the future. When you select _View Console_ , the output area of the inspector shows what your XForms code logs to the console. You log something to the console by dispatching an event `fr-log` to the id of the XForms inspector. If you included the inspector by setting the `oxf.epilogue.xforms.inspector` property to `true`, that id is `orbeon-xforms-inspector`. Otherwise, it is the id you put on the `<fr:xforms-inspector>`. The event take one parameter: `fr-messages`. Its value is a sequence of values, either elements or atomic values (strings, numbers…).

Copy

```
<xf:dispatch name="fr-log" target="orbeon-xforms-inspector">
    <xxf:context
        name="fr-messages"
        select="instance('fr-xforms-inspector-input')"/>
</xf:dispatch>
```

#### Console input <a href="#console-input" id="console-input"></a>

In some cases, you'd like to evaluate an XPath expression you type in the console, but you want that XPath expression to be evaluated in a very particular context, for instance in a middle of a sequence of actions. For this, add the following instance to your form:

Copy

```
<xf:instance id="fr-xforms-inspector-input">
    <input/>
</xf:instance>
```

When the XForms inspector finds this instance, it shows an additional input field, and binds it to your `fr-xforms-inspector-input` instance. You can then use fr-log evaluating the expression you typed in the newly added input field with:

Copy

```
<xf:dispatch
    event="xforms-value-changed"
    name="fr-log"
    targetid="orbeon-xforms-inspector">
    <xxf:context
        name="fr-messages"
        select="saxon:evaluate(instance('fr-xforms-inspector-input'))"/>
</xf:dispatch>
```
