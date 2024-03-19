# Dropdown date

This component allows users to select a date with 3 drop-downs, for the month, day, and year.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-M5---TrLlGIpE4k3iBt%252F-LEkC38F71KGcqPMxX8u%252Fxbl-dropdown-date.png%3Fgeneration=1586990424569762%26alt=media\&width=768\&dpr=4\&quality=100\&sign=873eca96f800936b366c71b3bcb73feff694044bab36ff47181edc20e52cac41)

Dropdown date component

### Default <a href="#default" id="default"></a>

By default, the years show from 1970 to the current year.

### Configuration <a href="#configuration" id="configuration"></a>

#### From Orbeon Forms 2020.1 <a href="#from-orbeon-forms-2020.1" id="from-orbeon-forms-2020.1"></a>

The values for the start and end year are determined as with the [`fr:component-param-value()`](https://doc.orbeon.com/xforms/xpath/extension-functions/extension-form-runner#fr-component-param-value) function, in the following order:

1. `start-year` and `end-year` attributes on the control
2. Form-level settings (in the `fr-form-metadata` instance)
3. `oxf.xforms.xbl.fr.dropdown-date.start-year.*.*` property with app/form wildcards
4. `oxf.xforms.xbl.fr.dropdown-date.start-year` property

All values are evaluated as AVTs.

For backward compatibility, a value can be an XPath expression, as follows:

* The value is evaluated as an AVT. If the result is castable as an integer, the value is used as an integer.
* The value is evaluated as an XPath expression.

Starting Orbeon Forms 2020.1, dynamic values should be written as AVTs and not plain XPath expressions.

Form Builder offers an option in the "Control Settings" dialog. The following example shows how to set the first available year as the current year, and the last available year as 10 years from the current year.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252Faa0cd7f844b64f676528cb1294212d34a96aa53a.png%3Fgeneration=1594317518961922%26alt=media\&width=768\&dpr=4\&quality=100\&sign=3aadad64368de113f7c801707d14caad1eee3ba247bb4882c8dd022105388e34)

Start year and End year value templates

#### Until Orbeon Forms 2019.2 <a href="#until-orbeon-forms-2019.2" id="until-orbeon-forms-2019.2"></a>

The values for the start and end year are determined as follows:

1.  \[SINCE 2016.3] If you're on Orbeon Forms 2016.3 or newer, and your form was created with Form Builder, and you defined an app/form specific property as in the example below, then the values of these properties are used.

    Copy

    ```
    <property 
       as="xs:string" 
       name="oxf.xforms.xbl.fr.dropdown-date.start-year.*.*" 
       value="2010"/>
    <property 
       as="xs:string" 
       name="oxf.xforms.xbl.fr.dropdown-date.end-year.*.*"
       value="year-from-date(current-date()) + 10"/>
    ```
2.  \[SINCE 4.2] If the conditions in point 1 don't apply, but you're on Orbeon Forms 4.2 or newer, and you defined a global property setting the start or end date, as in the example below, then the value of this property is used.

    Copy

    ```
    <property 
       as="xs:string"
       name="oxf.xforms.xbl.fr.dropdown-date.start-year"
       value="1970"/>
    <property 
       as="xs:string"
       name="oxf.xforms.xbl.fr.dropdown-date.end-year"
       value="year-from-date(current-date())"/>
    ```
3. Finally, if none of the properties mentioned in points 1 and 2 above are usable, the start year is set to 1970, and the end year to the current year.

The values of all the properties mentioned earlier are XPath expressions. In particular, this allows you to set an end year which isn't hard coded and instead allows you to set it to, say, "this year".

### &#x20; <a href="#see-also" id="see-also"></a>
