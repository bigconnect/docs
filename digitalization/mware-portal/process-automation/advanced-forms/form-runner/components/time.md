# Time

### Appearance <a href="#appearance" id="appearance"></a>

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-45c8a2f736f1ad2975940af3421370e25435fd1e%252Fxbl-time-controls.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=187880748655c36510d007f209d89fd500727c6adc34460d5b0d3377f583fc3a)

Regular appearance

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-1e8f1151d7196e5cc550f1eff7ade71845e492b0%252Fxbl-time-controls-readonly.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=5a100a34df37a77ecca5fd505a3f09b3b0dbf4196d15fe12ea48fffc05491ec8)

Readonly modes appearance

### What it does <a href="#what-it-does" id="what-it-does"></a>

The Time component allows the user to enter a time of day. The component has two major features:

* **Smart input:** lets the user enter a time in a variety of formats that are recognized, parsed, and then formatted
* **Formatting:** formats the time according to a specified format

The time is a local time, which does not contain a timezone, and stored in the data in ISO Format (`HH:mm:ss`).

With smart input, when the field loses the focus, the value you entered is parsed and, if recognized, replaced with a value in a canonical format.

Here are examples of supported smart time formats:

| Example      | Note                       |
| ------------ | -------------------------- |
| now          | current local time         |
| 21           | equivalent to 9:00:00 pm   |
| 3:34:56 p.m. | full time with seconds     |
| 3:34:56 pm   | same as above              |
| 3:34 p.m.    | equivalent to 3:34:00 p.m. |
| 3:34 pm      | same as above              |
| 3 p.m.       | equivalent to 3:00:00 p.m. |
| 3 pm         | same as above              |
| 3p           | equivalent to 3:00:00 pm   |
| 3:34:56 a.m. | full time with seconds     |
| 3:34:56 am   | same as above              |
| 3:34 a.m.    | equivalent to 3:34:00 a.m. |
| 3:34 am      | same as above              |
| 3 a.m.       | equivalent to 3:00:00 a.m. |
| 3 am         | same as above              |
| 3a           | same as above              |
| 12 a.m.      | midnight                   |
| 12 p.m.      | noon                       |

### Form Builder support <a href="#form-builder-support" id="form-builder-support"></a>

The Time component is directly accessible from the Form Builder toolbox.

The Control Settings dialog features a user interface for configuring the control width and output format.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-cafa5575872b27d711d15879bfc8d2d8944f56a8%252Fxbl-time-editor-custom-cropped.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=a76306d74bb914d879e1eb39e36d062105a1c4a43e485df976f86f09c8435e7f)

Configuring the time format

### Configuration <a href="#configuration" id="configuration"></a>

#### Field width <a href="#field-width" id="field-width"></a>

The `field-width` parameter can have two values:

* `natural`: takes a width defined by the component
* `full`: takes the entire width of the grid cell

This parameter can be specified at the control level, form level, or in properties-local.xml. The default is:

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.time.field-width"
    value="{if (fr:created-with-or-newer('2018.2')) then 'full' else 'natural'}"/>
```

#### Output format <a href="#output-format" id="output-format"></a>

The `oxf.xforms.format.input.time` global property configures the default output format for all instances of the Time component. The default value is:

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.format.input.time" 
    value="[h]:[m]:[s] [P,*-2]"/>
```

The format is a "picture string". These are examples of values supported:

| Format                 | Example      | Description                                  | Since    |
| ---------------------- | ------------ | -------------------------------------------- | -------- |
| `[h]:[m]:[s] [P]`      | 2:05:12 p.m. | with dots in a.m. and p.m.                   |          |
| `[h]:[m] [P]`          | 2:05 p.m.    | with dots in a.m. and p.m., no seconds       | 2020.1   |
| `[h]:[m]:[s] [P,*-2]`  | 2:05:12 pm   | without dots in am and pm                    |          |
| `[h]:[m] [P,*-2]`      | 2:05 pm      | without dots in am and pm, no seconds        | 2020.1   |
| `[h]:[m]:[s] [PN]`     | 2:05:12 P.M  | uppercase A.M. and P.M.                      | 2022.1.1 |
| `[h]:[m] [PN]`         | 2:05 P.M.    | uppercase A.M. and P.M.                      | 2022.1.1 |
| `[h]:[m]:[s] [PN,*-2]` | 2:05:12 PM   | uppercase AM and PM                          | 2022.1.1 |
| `[h]:[m] [PN,*-2]`     | 2:05 PM      | uppercase AM and PM                          | 2022.1.1 |
| `[H]:[m]:[s]`          | 14:05:12     | 24-hour time                                 |          |
| `[H]:[m]`              | 14:05        | 24-hour time                                 |          |
| `[H01]:[m]:[s]`        | 03:05:12     | 24-hour time, 2-digit hour                   | 2022.1.1 |
| `[H01]:[m]`            | 03:05        | 24-hour time, 2-digit hour (without seconds) | 2022.1.1 |

Important:

* The order of components (hours, minutes, etc.) cannot be changed.
* The separator is always `:`.
* Milliseconds are not supported.

When seconds are omitted, the user is still allowed to enter non-zero seconds. However, they will not be shown by the control when the user leaves the control.

\[SINCE Orbeon Forms 2022.1.1]

The Time component adds an `output-format` parameter. This parameter can be used to override the global property at the control level, the form level, or via properties-local.xml with the following new property:

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.time.output-format"
    value=""/>
```

As usual, the property can use an app name and form name (with possible wildcards) to specify a default value for all controls in a given app/form:

Copy

```
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.time.output-format.acme.*"
    value="[H01]:[m]"/>
```

The value is in the same format as the global `oxf.xforms.format.input.time` property.

By default, the `output-format` parameter is not set, and the global `oxf.xforms.format.input.time` property is used for backward compatibility.

### Output format in readonly modes <a href="#output-format-in-readonly-modes" id="output-format-in-readonly-modes"></a>

The Time control uses the `oxf.xforms.format.output.time` property to format the time value.

The default value is:

Copy

```
<property as="xs:string"  name="oxf.xforms.format.output.time">
    if (. castable as xs:time) then format-time(xs:time(.), '[h]:[m]:[s] [P,*-2]', xxf:lang(), (), ()) else .
</property>
```

\[SINCE Orbeon Forms 2022.1.1]

The Time components uses the `output-format` parameter to format the time in readonly modes, including `view` and `pdf`.

This means that the `oxf.xforms.format.output.time` property is no longer used in readonly modes.

### Example usage <a href="#example-usage" id="example-usage"></a>

Copy

```
<fr:time 
    bind="time-24hour-bind" 
    id="time-24hour-control" 
    field-width="natural"
    output-format="[H]:[m]:[s]">
    <xf:label ref="$form-resources/time-24hour/label"/>
    <xf:hint ref="$form-resources/time-24hour/hint"/>
</fr:time>
```
