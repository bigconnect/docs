# Reading and Processing XML Data

You can parse XML documents from an origin system with an origin enabled for the XML data format. You can also parse XML documents in a field in a Data Collector record with the XML Parser processor.

You can use the XML data format and the XML Parser to process well-formed XML documents. If you want to process invalid XML documents, you can try using the text data format with custom delimiters. 

Data Collector uses a user-defined delimiter element to determine how it generates records. When processing XML data, you can generate a single record or multiple records from an XML document, as follows:

**Generate a single record**

To generate a single record from an XML document, do not specify a delimiter element.

When you generate a single record from an XML document, the entire document is written to the record as a map.

**Generate multiple records using an XML element**

You can generate multiple records from an XML document by specifying an XML element as the delimiter element.

You can use an XML element when the element resides directly under the root element.

**Generate multiple records using a simplified XPath expression**

You can generate multiple records from an XML document by specifying a simplified XPath expression as the delimiter element.

Use a simplified XPath expression to access data below the first level of elements in the XML document, to access namespaced elements, elements deeper in complex XML documents.

