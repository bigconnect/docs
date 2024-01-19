# Expression Language

The Data Pipelines expression language enables you to create expressions that evaluate or modify data. The expression language is based on the JSP 2.0 expression language.

Use the expression language to configure expressions and conditions in processors, such as the Expression Evaluator or Stream Selector. Some destination properties also allow the expression language, such as the directory template for the Hadoop FS destination.

You can use the expression language to define any stage or pipeline property that represents a numeric or string value. You can also use field path expressions to select the fields to use in some processors.

Use expression completion to determine where you can use an expression and the expression elements that you can use in that location.

You can use the following elements in an expression:

* Constants
* Datetime variables
* Field names
* Functions
* Literals
* Operators
* Runtime parameters
* Runtime properties
* Runtime resources

For more information about using expressions in pipelines, see Expression Configuration.

### Expression Completion in Properties <a href="#ariaid-title2" id="ariaid-title2"></a>

Expression completion provides a list of functions and other elements of the StreamSets expression language that are valid for use at that location. The list includes any runtime parameters that are defined for the pipeline, and available fields when the pipeline is valid for data preview.

When an element does not display in the list, it is not a valid element at the specified location.

#### Tips for Expression Completion <a href="#ariaid-title3" id="ariaid-title3"></a>

Use the following information and tips when you invoke expression completion:

1.  To invoke expression completion, place the cursor where you want to create an expression and click Ctrl + Space Bar.

    A list of valid expression elements displays. Scroll to view the entire list.

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/ExComp-List.png)

    You can use expression completion anywhere where you can use an expression.
2.  When available, expression completion lists the following elements using the following icons:

    * Blue F for fields.
    * Red C for constants
    * Green M for functions.
    * Purple P for runtime parameters defined in the pipeline.

    In the following example, `DirectoryRoot` is a runtime parameter:

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/ExComp-FieldsConstants.png)
3.  To view more information about an element, down arrow key to navigate to and highlight the element name:

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/ExComp-ListInfo.png)
4. To add an element to an expression, double-click the element name or type `Enter`.
5. You can filter the element list by typing the first few letters of the element name.
6.  To view the syntax of a function, after you add the function, click within the parentheses.

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/ExComp-Syntax.png)

### Expression Examples <a href="#ariaid-title4" id="ariaid-title4"></a>

The following tables include some examples of how you might use expressions in the Data Collector.

**Conditions**

The following table includes some examples of conditions:

| Sample Condition Expressions               | Description                                                                                                                                                   |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ${record:value('/payment\_type') == 'CRD'} | In a Stream Selector, this condition routes records where the payment type is credit card to the stream.                                                      |
| ${record:value('\[3]/State') == "MD"}      | The same as above, but for a record in list format.                                                                                                           |
| ${record:errorCode()=="FIELD\_MERGER\_02"} | Used in a Stream Selector in an error pipeline, this condition routes records with the FIELD\_MERGER\_02 error to a stream that corrects the error.           |
| ${record:value("\[20]/value") == ""}       | When used in an alert, triggers an alert when the specified field contains no data.                                                                           |
| ${record:type('/ID')=STRING}               | In the Stream Selector, routes records where the ID is a String to a branch that includes a Field Type Converter to convert the field to a numeric data type. |

**if-then-else**

The following table includes some examples of if-then-else expressions:

| Sample if-then-else Expressions                                                                                                                                                                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ${record:value('/gender')=='1'?'M':(record:value('/gender')=='2'?'F':'U')}                                                                                                                              | Replaces 1 with "M", 2 with "F", and any other value with "U" for unknown.                                                                                                                                                                                                                                                                                                                                                               |
| ${record:value('/phone\_home')!=""?record:value('/phone\_home'):(record:value('/phone\_mobile')!=""?record:value('/phone\_mobile'):(record:value('/phone\_work')!=""?record:value('/phone\_work'):''))} | <p>The phone number to use for a primary_phone field based on phone numbers in the home_phone, mobile_phone and work_phone fields:</p><ul><li>If there's a home phone number, use the home phone.</li><li>If there's no home phone number, use the mobile phone number.</li><li>If there's no home or mobile phone number, use the work phone number.</li><li>If there are no home, mobile, or work numbers, use a null value.</li></ul> |

**String manipulation**

The following table includes some string manipulation examples:

| Sample String Manipulation Expressions          | Description                                                          |
| ----------------------------------------------- | -------------------------------------------------------------------- |
| ${record:valueOrDefault('/Payment', 'unknown')} | Replaces missing or null values in the Payment field with "unknown". |
| ${str:toUpper(record:value('/STATE')}           | Capitalizes all strings in the STATE field.                          |

**Numeric operation**

The following table includes an example of a numeric operation:

| Sample Numeric Operations                                                                                               |                                                                                                                |
| ----------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| ${record:value('/total\_amount') - (record:value('/tip\_amount') + record:value('/tolls') + record:value('/mta\_tax'))} | An expression that calculates trip revenue by subtracting the tip, taxes, and tolls from the total fare amount |
