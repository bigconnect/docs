# Source code editor

The source code editor component uses the excellent CodeMirror library, hence the name of the XBL element name: `<fr:code-mirror>`. You can use it in situations where you want to users to be able to view or edit source code. You bind it to the node that contains the text to view or edit, just like you would with an `<xforms:textarea>`. If the node you bind it to is readonly, then users will be able to view the source but not edit it.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC32TG\_mGXjfbwBUs%252Fxbl-code-mirror.png%3Fgeneration=1528743353246128%26alt=media\&width=768\&dpr=4\&quality=100\&sign=16b33bb0b4a0251d21dfa99563815995a43d03acbb8c0995bbd144d271a5fd91)

\[LIMITATION] At this point, the source editor can't be configured, and it always does syntax coloring interpreting the source as XML.

### Styling <a href="#styling" id="styling"></a>

By default, the editor takes 100% of the available width, and has a fixed height of 300 pixels. You can set to use a width and height of your choosing with CSS, as follows:

Copy

```
.xbl-fr-code-mirror .CodeMirror {
    height: 20em;
    width: 50em;
}
```
