# Reading and Processing XML Data

You can parse XML documents from an origin system with an origin enabled for the XML data format. You can also parse XML documents in a field in a Data Pipelines record with the XML Parser processor.

You can use the XML data format and the XML Parser to process well-formed XML documents. If you want to process invalid XML documents, you can try using the text data format with custom delimiters.&#x20;

BigConnect Pipelines uses a user-defined delimiter element to determine how it generates records. When processing XML data, you can generate a single record or multiple records from an XML document, as follows:

**Generate a single record**

To generate a single record from an XML document, do not specify a delimiter element.

When you generate a single record from an XML document, the entire document is written to the record as a map.

**Generate multiple records using an XML element**

You can generate multiple records from an XML document by specifying an XML element as the delimiter element.

You can use an XML element when the element resides directly under the root element.

**Generate multiple records using a simplified XPath expression**

You can generate multiple records from an XML document by specifying a simplified XPath expression as the delimiter element.

Use a simplified XPath expression to access data below the first level of elements in the XML document, to access namespaced elements, elements deeper in complex XML documents.

## Creating Multiple Records with an XML Element <a href="#ariaid-title2" id="ariaid-title2"></a>

You can generate records by specifying an XML element as a delimiter.

When the data you want to use is in an XML element directly under the root element, you can use the element as a delimiter. For example, in the following valid XML document, you can use the `msg` element as a delimiter element:

```
<?xml version="1.0" encoding="UTF-8"?>
<root>
 <msg>
    <time>8/12/2016 6:01:00</time>
    <request>GET /index.html 200</request>
 </msg>
 <msg>
    <time>8/12/2016 6:03:43</time>
    <request>GET /images/sponsored.gif 304</request>
 </msg>
</root>
```

Processing the document with the msg delimiter element results in two records.Note: When configuring the Delimiter Element property, enter just the XML element name, without surrounding carat characters. That is, use `msg` instead of `<msg>`.

### Using XML Elements with Namespaces <a href="#ariaid-title3" id="ariaid-title3"></a>

When you use an XML element as a delimiter, BigConnect Pipelines uses that exact element name that you specify to generate records.

If you include a namespace prefix in the XML element, you must also define the namespace in the stage. Then, BigConnect Pipelines can process the specified XML element with the prefix.

For example, you use the `a:msg` element as the delimiter element and define the Company A namespace in the stage. Then, BigConnect Pipelines processes only the `a:msg` element in the Company A namespace. It generates one record for the following document, ignoring data in the `c:msg` element:

```
<?xml version="1.0" encoding="UTF-8"?>
<root>
	<a:msg xmlns:a="http://www.companyA.com">
		<time>8/12/2016 6:01:00</time>
		<request>GET /index.html 200</request>
	</a:msg>
	<c:msg xmlns:c="http://www.companyC.com">
		<item>Shoes</item>
		<item>Magic wand</item>
		<item>Tires</item>
	</c:msg>
	<c:msg xmlns:c="http://www.companyC.com">
		<time>8/12/2016 6:03:43</time>
		<request>GET /images/sponsored.gif 304</request>
	</c:msg>
</root>
```

In the stage, you define the Namespace property using the prefix "a" and the namespace URI: http://www.companyA.com.

The following image shows a Directory origin configured to process this data:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/XML-Delim-Namespace.png)

## Creating Multiple Records with an XPath Expression <a href="#ariaid-title4" id="ariaid-title4"></a>

You can generate records from an XML document using a simplified XPath expression as the delimiter element.

Use a simplified XPath expression to access data below the first level of elements in the XML document. You can also use an XPath expression to access namespaced elements or elements deeper in complex XML documents.

For example, say an XML document has record data in a second level `msg` element, as follows:

```
<?xml version="1.0" encoding="UTF-8"?>
<root>
    <data>
        <msg>
            <time>8/12/2016 6:01:00</time>
            <request>GET /index.html 200</request>
        </msg>
    </data>
    <data>
        <msg>
            <time>8/12/2016 6:03:43</time>
            <request>GET /images/sponsored.gif 304</request>
        </msg>
    </data>
</root>
```

Since the `msg` element is not directly under the root element, you cannot use it as a delimiter element. But you can use the following simplified XPath expression to access the data:

```
/root/data/msg
```

Or, if the first level `data` element can sometimes be `info`, you can use the following XPath expression to access any data in the `msg` element that is two levels deep:

```
/root/*/msg
```

### Using XPath Expressions with Namespaces <a href="#ariaid-title5" id="ariaid-title5"></a>

When using an XPath expression to process XML documents, you can process data within a namespace. To access data in a namespace, define the XPath expression, then use the Namespace property to define the prefix and definition of the namespace.

For example, the following XML document includes two namespaces, one for Company A and one for Company C:

```
<?xml version="1.0" encoding="UTF-8"?>
<root>
	<a:data xmlns:a="http://www.companyA.com">
		<msg>
			<time>8/12/2016 6:01:00</time>
			<request>GET /index.html 200</request>
		</msg>
	</a:data>
	<c:data xmlns:c="http://www.companyC.com">
		<sale>
			<item>Shoes</item>
			<item>Magic wand</item>
			<item>Tires</item>
		</sale>
	</c:data>
	<a:data xmlns:a="http://www.companyA.com">
		<msg>
			<time>8/12/2016 6:03:43</time>
			<request>GET /images/sponsored.gif 304</request>
		</msg>
	</a:data>
</root>
```

To create records from data in the msg element in the Company A namespace, you can use either of the following XPath expressions:

```
/root/a:data/msg
/root/*/msg
```

Then define the Namespace property using the prefix "a" and the namespace URI: http://www.companyA.com.

The following image shows a Directory origin configured to process this data:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/XML-Namespace.png)

### Simplified XPath Syntax <a href="#ariaid-title6" id="ariaid-title6"></a>

When using an XPath expression to generate records from an XML document, use a simplified version of the [abbreviated XPath syntax](https://en.wikipedia.org/wiki/XPath#Abbreviated\_syntax).

Use the abbreviated XPath syntax with the following restrictions:Operators and XPath functionsDo not use operators or XPath functions in the XPath expression.Axis selectorsUse only the single slash ( / ) child selector. The descendant-or-self double slash selector ( // ) is not supported.Node testsOnly node name tests are supported. Note the following details:

* You can use namespaces with node names, defined with an XPath namespace prefix. For more information, see [Using XPath Expressions with Namespaces](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Data\_Formats/XMLDFormat.html#concept\_mkk\_3zj\_dy).
* Do not use namespaces for attributes.
* Elements can include predicates.

Predicates

* You can use the position predicate or attribute value predicate with elements, not both.
*   Use the following syntax to specify a position predicate:

    ```
    /<element>[<position number>]
    ```
*   Use the following syntax to specify an attribute value predicate:

    ```
    /<element>[@<attribute name>='<attribute value>']
    ```
* You can use the asterisk wildcard as the attribute value. Surround the value in single quotation marks.

For more information, see [Predicates in XPath Expressions](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Data\_Formats/XMLDFormat.html#concept\_bnl\_4fh\_2y).Wildcard characterYou can use the asterisk ( \* ) to represent a single element, as follows:

```
/root/*/msg
```

You can also use the asterisk to represent any attribute value. Use the asterisk to represent the entire value, as follows:

```
/root/info[@attribute='*']/msg
```

### Sample XPath Expressions <a href="#ariaid-title7" id="ariaid-title7"></a>

Here are some examples of valid and invalid XPath expressions:

Valid expressionsThe following expression selects every element beneath the first top-level element.

```
/*[1]/*
```

The following expression selects every value element under an allvalues element with a source attribute set to "XYZ". The allvalues element below a top-level element named root. Each element is in the abc namespace:

```
/abc:root/abc:allvalues[@source='XYZ']/xyz:value
```

Invalid expressionsThe following expressions are not valid:

* `/root//value` - Invalid because the descendent-or-self axis (“//”) is not supported.
* `/root/collections[last()]/value` - Invalid because functions, e.g. last, are not supported.
* `/root/collections[@source='XYZ'][@sequence='2']` - Invalid because multiple predicates for an element are not supported.
* `/root/collections[@source="ABC"]` - Invalid because attribute the attribute value should be in single quotation marks.
* `/root/collections[@source]` - Invalid because the expression uses an attribute without defining the attribute value.

### Predicates in XPath Expressions <a href="#ariaid-title8" id="ariaid-title8"></a>

You can use predicates in XPath expressions to process a subset of element instances. You can use a position predicate or attribute values predicate with an element, but not both. You can also use a wildcard to define the attribute value.Position predicateThe position predicate indicates the instance of the element to use in the file. Use a position predicate when the element appears multiple times in a file, and you want to use a particular instance based on the position of the instances in the file, e.g. the first, second, or third time the element appears in the file.Use the following syntax to specify a position predicate:

```
/<element>[<position number>]
```

For example, say the `contact` element appears multiple times in the file, but you only care about the address data in the first instance in the file. Then you can use a predicate for the element as follows:

```
/root/contact[1]/address
```

Attribute value predicateThe attribute value predicate limits the data to elements with the specified attribute value. Use the attribute value predicate when you want to specify an element with a particular attribute values or an element that simply has an attribute value defined.Use the following syntax to specify an attribute value predicate:

```
/<element>[@<attribute name>='<attribute value>']
```

You can use the asterisk wildcard as the attribute value. Surround the value in single quotation marks.For example, if you only wanted `server` data with a `region` attribute set to "west", you can add the region attribute as follows:

```
/*/server[@region='west']
```

### Predicate Examples <a href="#ariaid-title9" id="ariaid-title9"></a>

To process all data in a `collections` element under the `apps` second-level element, you would use the following simplified XPath expression:

```
/*/apps/collections
```

If you only wanted the data under the first instance of `apps` in the XML document, you would add a position predicate as follows:

```
/*/apps[1]/collections
```

To only process data from all `app` elements in the document where the `collections` element has a version attribute set to 3, add the version attribute and value as follows:

```
/*/apps/collections[@version='3']
```

If you don't care what the value of the attribute is, you can use a wildcard for the attribute value, as follows:

```
/root/apps/collections[@version='*']
```

## Preserving the Root Element <a href="#ariaid-title10" id="ariaid-title10"></a>

You can include the root element in the generated record by enabling the Preserve Root Element property.

The root element depends on whether you omit or specify a delimiter:Omit a delimiterWhen you omit a delimiter to generate a single record, the root element is the root element of the XML document.Specify a delimiterWhen you specify a delimiter to generate multiple records, the root element is the XML element specified as the delimiter element or is the last XML element in the simplified XPath expression specified as the delimiter element.For example, let's look at how BigConnect Pipelines treats the root element when processing the following XML document:

```
<?xml version="1.0" encoding="UTF-8"?>
<bookstore xmlns:prc="http://books.com/price">
   <b:book xmlns:b="http://books.com/book">
      <title lang="en">Harry Potter</title>
      <prc:price>29.99</prc:price>
   </b:book>
   <b:book xmlns:b="http://books.com/book">
      <title lang="en_us">Learning XML</title>
      <prc:price>39.95</prc:price>
   </b:book>
</bookstore>
```

### Root Element when Omitting a Delimiter

If you omit a delimiter to generate a single record, the root element is `bookstore`, the root element of the XML document.

When you enable the Preserve Root Element property, BigConnect Pipelines generates the following record, including `bookstore` as the root element:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/XML-PreserveRootElement.png)

When you disable the Preserve Root Element property, BigConnect Pipelines generates the following record, removing the `bookstore` root element:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/XML-NoRootElement.png)

### Root Element when Specifying a Delimiter

If you specify `book` as the delimiter element to generate multiple records, the root element is `book`.

When you enable the Preserve Root Element property, BigConnect Pipelines generates the following records, including `book` as the root element of both records:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/XML-PreserveRootElementDelimiter.png)

When you disable the Preserve Root Element property, BigConnect Pipelines generates the following records, removing `book` as the root element from both records:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/XML-NoRootElementDelimiter.png)

Similarly, if you specify the XPath expression `/bookstore/book/author` as the delimiter element to generate multiple records, the root element is `author`. BigConnect Pipelines includes `author` in the generated records based on the Preserve Root Element property.

## Including Field XPaths and Namespaces <a href="#ariaid-title11" id="ariaid-title11"></a>

You can include field XPath expressions and namespaces in the record by enabling the Include Field XPaths property.

When enabled, the record includes the XPath expression for each field as a field attribute and includes each namespace in an xmlns record header attribute. By default, this information is not included in the record.

For example, say you have the following XML document:

```
<?xml version="1.0" encoding="UTF-8"?>
<bookstore xmlns:prc="http://books.com/price">
   <b:book xmlns:b="http://books.com/book">
      <title lang="en">Harry Potter</title>
      <prc:price>29.99</prc:price>
   </b:book>
   <b:book xmlns:b="http://books.com/book">
      <title lang="en_us">Learning XML</title>
      <prc:price>39.95</prc:price>
   </b:book>
</bookstore>
```

When you use `/*[1]/*` as the delimiter element and enable the Include Field XPaths property, BigConnect Pipelines generates the following records with the highlighted field XPath expressions and namespace record header attributes:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/XML-FieldXPath.png)

Note: Field attributes and record header attributes are written to destination systems automatically only when you use the SDC RPC data format in destinations. For more information about working with field attributes and record header attributes, and how to include them in records, see Field Attributes and Record Header Attributes.

## XML Attributes and Namespace Declarations <a href="#ariaid-title12" id="ariaid-title12"></a>

Parsed XML includes XML attributes and namespace declarations in the record as individual fields by default. You can use the Output Field Attributes property to place the information in field attributes instead.

Place the information in field attributes to avoid adding unnecessary information in the record fields.

Note: Field attributes are automatically included in records written to destination systems only when you use the RPC data format in the destination. For more information about working with field attributes, see Field Attributes.

## Parsed XML <a href="#ariaid-title13" id="ariaid-title13"></a>

When parsing XML documents with the XML data format or the XML Parser processor, BigConnect Pipelines generates a field that is a map of fields based on nested elements, text nodes, and attributes. Comment elements are ignored.

For example, say you have the following XML document:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<root>
 <a:info xmlns:a="http://www.companyA.com">  
  <sale>  
    <item>Apples</item>  
    <item>Bananas</item> 
  </sale>
</a:info>
<c:info xmlns:c="http://www.companyC.com"> 
  <sale>
    <item>Shoes</item>
    <item>Magic wand</item>  
    <item>Tires</item>
  </sale>
 </c:info>
</root>
```

To create records for the data in the `sale` element in both namespaces, you can use a wildcard to represent the second level `info` element, as follows:

```
/root/*/sale
```

Then, you define both namespaces in the origin.

When processing the XML document using default XML properties, BigConnect Pipelines produces two records, as shown in the following data preview of the origin:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/XML-records.png)
