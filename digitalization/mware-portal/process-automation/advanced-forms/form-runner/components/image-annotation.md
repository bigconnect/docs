# Image annotation

The image annotation component allows users to select an image and annotate it. Both the original image users select and the image with the annotation are saved as part of the data.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkByNMBMK43CtT-2MQ%252Fxbl-image-annotation.png%3Fgeneration=1528743323834089%26alt=media\&width=768\&dpr=4\&quality=100\&sign=82150d8d3714c88620851b088d2d0d2354b8286bd966ca7bf84f1259bd86c433)

### Browser support <a href="#browser-support" id="browser-support"></a>

This component uses [wPaint](https://github.com/websanova/wPaint), which requires support for the [HTML5 canvas](http://en.wikipedia.org/wiki/Canvas\_element) in the web browser. Most browsers have support for the canvas, but note that Internet Explorer [IE9 or later is required](http://en.wikipedia.org/wiki/Canvas\_element#Support) (this feature will not work with IE7 and IE8).

### Configuration <a href="#configuration" id="configuration"></a>

You can change the start stroke color with the following property:

```
<property 
    as="xs:string"  
    name="oxf.xforms.xbl.fr.wpaint.start-stroke-color.*.*"      
    value="#FF8C00"/>
```

### In Form Builder <a href="#in-form-builder" id="in-form-builder"></a>

To add this component to your form, in the toolbar, in the _Attachments_ section, choose _Image annotation_.

### In XForms <a href="#in-xforms" id="in-xforms"></a>

When using this component in your own XForms (rather than through Form Builder), you must bind it to an element that is either empty, or contains two elements `<image>` and `<annotation>`. If the element is empty, the two elements `<image>` and `<annotation>` are automatically added by the component.
