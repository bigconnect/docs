# Dropdown date

This component allows users to select a date with 3 drop-downs, for the month, day, and year.

![Dropdown date component](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-M5---TrLlGIpE4k3iBt%252F-LEkC38F71KGcqPMxX8u%252Fxbl-dropdown-date.png%3Fgeneration=1586990424569762%26alt=media\&width=768\&dpr=4\&quality=100\&sign=873eca96f800936b366c71b3bcb73feff694044bab36ff47181edc20e52cac41)

### Default <a href="#default" id="default"></a>

By default, the years show from 1970 to the current year.

### Configuration <a href="#configuration" id="configuration"></a>



The values for the start and end year are determined as with the `fr:component-param-value()` function, in the following order:

1. `start-year` and `end-year` attributes on the control
2. Form-level settings (in the `fr-form-metadata` instance)
3. `oxf.xforms.xbl.fr.dropdown-date.start-year.*.*` property with app/form wildcards
4. `oxf.xforms.xbl.fr.dropdown-date.start-year` property

All values are evaluated as AVTs.

For backward compatibility, a value can be an XPath expression, as follows:

* The value is evaluated as an AVT. If the result is castable as an integer, the value is used as an integer.
* The value is evaluated as an XPath expression.

Dynamic values should be written as AVTs and not plain XPath expressions.

Form Builder offers an option in the "Control Settings" dialog. The following example shows how to set the first available year as the current year, and the last available year as 10 years from the current year.

![Start year and End year value templates](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252Faa0cd7f844b64f676528cb1294212d34a96aa53a.png%3Fgeneration=1594317518961922%26alt=media\&width=768\&dpr=4\&quality=100\&sign=3aadad64368de113f7c801707d14caad1eee3ba247bb4882c8dd022105388e34)
