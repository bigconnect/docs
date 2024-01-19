# Text Data Format with Custom Delimiters

By default, the text data format creates records based on line breaks, creating a record for each line of text. You can configure origins to create records based on custom delimiters.

Use custom delimiters when the origin system uses delimiters to separate logical sections of data that you want to use as records. A custom delimiter might be as simple as a semicolon or might be a set of characters. You can even use an XML tag as a custom delimiter to read XML data.

Note: When using a custom delimiter, the origin uses the delimiter characters to create records, ignoring new lines.

For most origins, you can include the custom delimiters in records or you can remove them. For the Hadoop FS and MapR FS origins, you cannot include the custom delimiters in records.

For example, say you configure the Directory origin to process a file with the following text, using a semicolon as a delimiter, and discarding the delimiter:

```
8/12/2016 6:01:00 unspecified error message;8/12/2016 
6:01:04 another error message;8/12/2016 6:01:09 just a warning message;
```

The origin generates the following records, with the data in a single text field:

| TEXT                                                 |
| ---------------------------------------------------- |
| 8/12/2016 6:01:00 unspecified error message          |
| <p>8/12/2016</p><p>6:01:04 another error message</p> |
| 8/12/2016 6:01:09 just a warning message             |

Note that the origin retains the line break, but does not use it to create a separate record.

### Processing XML Data with Custom Delimiters <a href="#ariaid-title2" id="ariaid-title2"></a>

You can use custom delimiters with the text data format to process XML data. You might use the text data format to process XML data with no root element, which cannot be processed with the XML data format.

When using the text data format in the origin to read XML data, you can use the XML Parser processor downstream to parse the XML data.

For example, the following XML document is valid and is best processed using the XML data format:

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

However, the following XML document does not include an XML prolog or root element, so it is invalid:

```
<msg>
    <time>8/12/2016 6:01:00</time>
    <request>GET /index.html 200</request>
</msg>
<msg>
    <time>8/12/2016 6:03:43</time>
    <request>GET /images/sponsored.gif 304</request>
</msg>
```

You can use the text data format with a custom delimiter to process the invalid XML document. To do so, use \</msg> as the custom delimiter to separate data into records, and make sure to include the delimiter in the record as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Text-CustomDelim-Dir.png)

When origins process text data, they write record data into a single text field named "text". When Directory processes the invalid XML document, it creates two records:

| TEXT                                                                                               |
| -------------------------------------------------------------------------------------------------- |
| \<msg> \<time>8/12/2016 6:01:00\</time> \<request>GET /index.html 200\</request> \</msg>           |
| \<msg> \<time>8/12/2016 6:03:43\</time> \<request>GET /images/sponsored.gif 304\</request> \</msg> |

You can configure the XML Parser to parse the XML data as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Text-CustomDelim-XML.png)

The XML Parser converts the time and request attributes to list fields within the text map field, as shown. The table displays data types in angle brackets ( < > ):

| TEXT \<MAP>                                                                                                                                                                                                                                    |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>- time &#x3C;list>:</p><ul><li><p>0 &#x3C;map>:</p><p>- value &#x3C;string>: 8/12/2016 6:01:00</p></li></ul><p>- request &#x3C;list>:</p><ul><li><p>0 &#x3C;map>:</p><p>- value &#x3C;string>: GET /index.html 200</p></li></ul>            |
| <p>- time &#x3C;list>:</p><ul><li><p>0 &#x3C;map>:</p><p>- value &#x3C;string>:: 8/12/2016 6:03:43</p></li></ul><p>- request &#x3C;list>:</p><ul><li><p>0 &#x3C;map>:</p><p>- value &#x3C;string>: GET /images/sponsored.gif 304</p></li></ul> |
