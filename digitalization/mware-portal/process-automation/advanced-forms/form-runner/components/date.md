# Date

The date component is designed to enter a date, without time. On the desktop, the component shows as a field with a linked date picker.

### Basic usage <a href="#basic-usage" id="basic-usage"></a>

You use the date component like a regular input field, for example:

```
<fr:date ref="birth-date">
  <xf:label>Birth Date</xf:label>
</fr:date>
```

### Datatype <a href="#datatype" id="datatype"></a>

`fr:date` must be bound to `xs:date`.

### Parameters <a href="#parameters" id="parameters"></a>

`fr:date` supports parameters, which you can set via properties, Form Builder settings, or directly on `fr:date`:

* `week-start-day`
  * first day of the week
  * values: `sunday` or `monday`
  * default: when not specified, the start day depends on the language (for example Sunday for English, Monday for French)

These are the default values of the properties:

Copy

```
<property 
    as="xs:string" 
    name="oxf.xforms.xbl.fr.date.week-start-day.*.*" 
    value=""/>
```

### Native date picker <a href="#native-date-picker" id="native-date-picker"></a>

On iOS, the native iOS date picker is used by default, unless users enabled Request Desktop Website in Safari, in which case they will see the same behavior as they would on the desktop.

As browser support for the native date picker on desktop has improved over the year, and since it is better supported by screen readers, you may want to use the native date picker not only on mobile but also on desktop. You can achieve this by setting the property mentioned below. When doing so, the date format is determined by the browser based on the user's locale. Consequently, the properties `oxf.xforms.format.input.date` and `oxf.xforms.format.output.date` have no effect on the format of the native date picker.

```
<property 
    as="xs:string"  
    name="oxf.xforms.xbl.fr.date.native-picker.*.*"             
    value="always"/>
```

![Native date picker on Chrome desktop](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-13f7fada051b477e7717ea46112d031d86840067%252Fnative-date-picker-chrome.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=d8e00b4f887ee42a39bef64934c6b2a122620fe510a8ea160efcc71ae86b6887)

