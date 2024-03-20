# Static and dynamic dropdown

### Possible combinations <a href="#possible-combinations" id="possible-combinations"></a>

Combinations of static vs. dynamic, with vs. without search:

|                | Static                         | Dynamic                         |
| -------------- | ------------------------------ | ------------------------------- |
| Without search | Static dropdown without search | Dynamic dropdown without search |
| With search    | Static dropdown with search    | Dynamic dropdown with search    |

#### Static vs. Dynamic <a href="#static-vs.-dynamic" id="static-vs.-dynamic"></a>

In the Form Builder Toolbox you'll find 2 dropdown components, as shown in the screenshot below. The _Static Dropdown_ is for cases when you, as a form author, want to provide, ahead of time, in Form Builder, the list of all the different choices users will be able to choose from. This is in contrast with the _Dynamic Dropdown_ where the list of choices will be loaded at runtime from a service you provide.

Typically, you'll want to use the Static Dropdown for cases where the number of choices is limited, and doesn't change much over time, and the Dynamic Dropdown for cases where you have a long list of choices and/or those choices can change over time.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LqIrWnNGY7QUbpjMPoV%252F-LqIrhJJlmkA4WhvjVtg%252Fdynamic-data-dropdown-components.png%3Fgeneration=1570143923526373%26alt=media\&width=768\&dpr=4\&quality=100\&sign=16f635eddfa77f64471d70c2957ca0857fcb1f488caa0ca2733918e85847b330)

Static Dropdown and Dynamic Dropdown in the Toolbox

#### With vs. without search <a href="#with-vs.-without-search" id="with-vs.-without-search"></a>

You can choose to have each one of your Static or Dynamic Dropdown show as a regular dropdown native to the browser, or as a dropdown with search. You make that choice in the Control Settings dialog, as highlighted in the screenshot below.

![Choosing between a regular dropdown and a dropdown with search](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LqIrWnNGY7QUbpjMPoV%252F-LqIrhJMZorvMw9OEU21%252Fdynamic-data-dropdown-with-without-search.png%3Fgeneration=1570143918683771%26alt=media\&width=768\&dpr=4\&quality=100\&sign=5ce6afd166fa4ca62fa5035de464c28650adc91943e3e7384fc63ab522e6e74c)

The dropdown "with search" doesn't use the native browser dropdown, but instead uses a dropdown implemented in JavaScript that allows users to search for the choice they want to select by typing part of the label, which is typically useful when the list of choices can be pretty long, and it is thus hard for users to visually just pick one of the choices offered when the dropdown opens.

#### Example with search <a href="#example-with-search" id="example-with-search"></a>

![Doing a search to select a country](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LqIrWnNGY7QUbpjMPoV%252F-LqIrhJOzpHalo\_9yqHe%252Fdynamic-data-dropdown-search-country.gif%3Fgeneration=1570143918467616%26alt=media\&width=768\&dpr=4\&quality=100\&sign=e3586c0ddfb0091da17b28a90b44fe7a0c56c917f29899384f567dff454400cf)

### Dynamic dropdowns <a href="#dynamic-dropdowns" id="dynamic-dropdowns"></a>

#### Features combinations <a href="#features-combinations" id="features-combinations"></a>

The Venn diagram below illustrates the various ways in which the features of dynamic dropdowns can be combined:

* Areas marked with an "x" indicate combinations that are not feasible. Specifically, enabling both the service to perform the search and auto-selection simultaneously is impossible. This is because auto-selection requires knowing all possible choices beforehand, necessitating the service to return all possible options. However, when the service conducts a search, it only returns a subset of the possible choices based on the user's input.
* The remaining areas depict the six feasible combinations of features.
* Among those areas, caution is advised regarding #6, i.e., combining open selection with auto-selection. Consider a dropdown for a street number and a service that returns all known numbers on a given street. After the user selects a street, if the service only knows about one number on that street, that number will be auto-selected. If open selection is enabled, the user will be able to change the number, but the experience of having an unwanted number selected by default might be confusing. Therefore, it might be wise to be cautious about enabling both features simultaneously.

![Different combinations of features](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-9365d1e22f58ee8e9f6578f2785d5ce123fd0d04%252Fdynamic-data-dropdown-search-combinations.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=7f3dfb2685ffb8eabca7669a96a233f5026d06ba33f2c5ee709502e386f28483)

#### Extracting choices from your service response <a href="#extracting-choices-from-your-service-response" id="extracting-choices-from-your-service-response"></a>

Your service must either return XML or JSON. (If it returns JSON the result will first be converted to XML, so Orbeon Forms can run XPath expressions on it.) Say your service returns a list of countries as follows, here including just the first 3 countries:

```
<countries>
    <country>
        <name>Afghanistan</name>
        <us-code>af</us-code>
    </country>
    <country>
        <name>Akrotiri</name>
        <us-code>ax</us-code>
    </country>
    <country>
        <name>Albania</name>
        <us-code>al</us-code>
    </country>
</countries>
```

You specify how to extract the necessary information to populate the dropdown through 3 XPath expressions, which you enter in the Control Settings dialog:

* The "Choices XPath expression" must return one item per choice in the dropdown, or in our example, per country, which is done with `/countries/country`.
* The "Label XPath expression" is relative to a given choice as returned by the first expression, and must return the label shown to users in the dropdown. In our example, this will point to the `name` element.
* The "Value XPath expression" is similar to the "Label XPath expression" but points to the value stored in the data when users make a selection.

![Extracting choices with XPath in Control Settings](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LqNWDoi1fywy7y-oksS%252F-LqNWSG7CP0tuzXA0dPh%252Fdynamic-data-dropdown-exact.png%3Fgeneration=1570221971670448%26alt=media\&width=768\&dpr=4\&quality=100\&sign=c00099921632a420cae7cf411f48c9d91d3ab84bab9edafbbc34d0a3f4edd0d1)

#### Services performing the search <a href="#services-performing-the-search" id="services-performing-the-search"></a>

In most cases, it will be simpler to have your service return all the possible values. But in some cases, the list of all possible values could be extremely large, and returning all them to Orbeon Forms would be prohibitively expensive, or outright impossible. In those cases you'll want the service to only return a subset of all possible choices, based on what users have typed so far. To do so:

* Make sure you've checked the "With Search" appearance.
* Select the "Yes" radio button under "Service performs search".
* Pass the value typed by users as a request parameter to your service by adding `{$fr-search-value}` as appropriate on the URL, for instance: `/fr/service/custom/orbeon/controls/countries?country-name={$fr-search-value}`.

With this, Orbeon Forms will automatically call your service repeatedly as users type in the field, this to retrieve a new list of possible choices, and update the dropdown.

![Service performing search](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LqNZffPLJ-6ZOS\_V\_rU%252F-LqNZyOPxjN56zMDyBLK%252Fdynamic-data-dropdown-service-search.png%3Fgeneration=1570222898114600%26alt=media\&width=768\&dpr=4\&quality=100\&sign=3654a9ff0562fe3cc1a795e5c8ca7dc094f8dd85650b37eb57ca48de92a2c500)

#### Paging <a href="#paging" id="paging"></a>

Services that implement their own search can also implement paging. This is useful for cases where the number of possible values is so large, that you don't want your service to return all the possible values even filtered down by what the user has typed so far, but instead want to return the first, say, 20 choices, and only show the following 20 after users scroll down in the dropdown past the first 20 choices. To do so:

* Setup your service as described in the previous section about services performing search.
* Add a request parameter `{$fr-search-page}` to the URL. The first time the service is called, the value will be `1`. If users scroll down in the dropdown, and reach the bottom, they will see a message informing them that more results will be loaded, and your service will be called with the parameter set to 2, and so on.
* At some point, users might reach the last page, as your service doesn't have more choices to list for the given search string. You'll want to include some information in your response as to whether the last page has been reached, and provide an XPath expression under "Is last page XPath expression" returning `true` when on the last page.

![Service performing search](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LqNde8DjFRBmSjPlZuZ%252F-LqNdrAguYJ1MP1buP0Z%252Fdynamic-data-dropdown-service-paging.png%3Fgeneration=1570224175158158%26alt=media\&width=768\&dpr=4\&quality=100\&sign=ba653b932a4d8bf4131bc8e827b292dd787b39dd289135df5a6700279fcb65dc)

#### Open selection <a href="#open-selection" id="open-selection"></a>

You can mark a dynamic search dropdown as _open_ in cases where you want users to be able to enter a value that is not returned by the service. For example, you may use a service that returns all known addresses in a given city, but the service may not be aware of new developments.

In the Control Settings dialog box, open selection can be disabled (using the No radio button, which is the default), enabled (using the Yes radio button), or conditionally enabled (using the Formula radio button). When set to Yes or Formula, the Label and Value formulas must be the same; therefore, the Label formula input field is disabled and its value is automatically set to the Value formula.

![Open selection inside the Control Settings](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-3225ce977691c91ca03ec03496dd49955a0173ae%252Fdynamic-data-dropdown-open.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=863d054f23660e055d5820f1d48418d2f2fd99db79a0e7d7919f748e60c031ca)

#### Minimum input length <a href="#minimum-input-length" id="minimum-input-length"></a>

You can specify a minimum input length, in characters, before the dropdown starts displaying choices. This is useful if you have a large number of choices, and the subset of choices returned is unlikely to be useful if users don't type at least a few characters. This is configured by setting the following property.

```
<property 
    as="xs:integer"
    name="oxf.xforms.xbl.fr.databound-select1-search.min-input-length.*.*"
    value="2"/>
```
