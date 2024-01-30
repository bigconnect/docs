# Salesforce

The Salesforce destination writes data to Salesforce objects.&#x20;

When you configure the Salesforce destination, you define connection information, including the API type and version that the destination uses to connect to Salesforce. You specify the Salesforce object to write to by entering the object name or by defining an expression that evaluates to the object name.

You can write Salesforce platform events as you write to any Salesforce object by using the platform event API name, for example, Notification\_\_e, rather than a Salesforce object type API name such as Account or Widget\_\_c.

The Salesforce destination can use CRUD operations defined in the `sdc.operation.type` record header attribute to write data. You can define a default operation for records without the header attribute or value. You can also configure how to handle records with unsupported operations.&#x20;

By default, the Salesforce destination writes data to a Salesforce object by matching case-sensitive field names. You can override the default field mappings by defining specific mappings that the CRUD operation requires. For update and delete operations, the incoming record must contain the record ID.

You can optionally use an HTTP proxy to connect to Salesforce. When enabled in Salesforce, you can configure the destination to use mutual authentication to connect to Salesforce.

### Changing the API Version <a href="#ariaid-title2" id="ariaid-title2"></a>

Data Collector ships with version 43.0 of the Salesforce Web Services Connector libraries. You can use a different Salesforce API version if you need to access functionality not present in version 43.0.

1. On the Salesforce tab, set the API Version property to the version that you want to use, for example 39.0.
2.  Download the relevant version of the following JAR files from Salesforce Web Services Connector (WSC):

    * WSC JAR file - force-wsc-\<version>.0.0.jar
    * Partner API JAR file - force-partner-api-\<version>.0.0.jar

    Where `<version>` is the API version number, for example, 39.

    For information about downloading libraries from Salesforce WSC, see the Salesforce Developer documentation.
3.  In the following Data Collector directory, replace the default force-wsc-43.0.0.jar and force-partner-api-43.0.0.jar files with the versioned JAR files that you downloaded:

    ```
    $SDC_DIST/streamsets-libs/streamsets-datacollector-salesforce-lib/lib/
    ```
4. Restart Data Collector for the changes to take effect.

### Define the CRUD Operation <a href="#ariaid-title3" id="ariaid-title3"></a>

When using the SOAP API, the Salesforce destination can insert, update, upsert, delete, or undelete data. When using the Bulk API, the destination can insert, update, upsert, or delete data.

The destination writes the records based on the CRUD operation defined in a CRUD operation header attribute or in operation-related stage properties:CRUD record header attributeYou can define the CRUD operation in a CRUD operation record header attribute. The destination looks for the CRUD operation to use in the `sdc.operation.type` record header attribute.The attribute can contain one of the following numeric values:

* 1 for INSERT
* 2 for DELETE
* 3 for UPDATE
* 4 for UPSERT
* 6 for UNDELETE (not supported by the Salesforce Bulk API)

If your pipeline includes a CRUD-enabled origin that processes changed data, the destination simply reads the operation type from the `sdc.operation.type` header attribute that the origin generates. If your pipeline uses a non-CDC origin, you can use the Expression Evaluator or a scripting processor to define the record header attribute.&#x20;

Operation stage propertiesYou define a default operation in the destination properties. The destination uses the default operation when the `sdc.operation.type` record header attribute is not set.You can also define how to handle records with unsupported operations defined in the `sdc.operation.type` header attribute. The destination can discard them, send them to error, or use the default operation.

### Field Mappings <a href="#ariaid-title4" id="ariaid-title4"></a>

When you configure the Salesforce destination, you can override the default mapping of case-sensitive field names by mapping specific fields in the record to existing fields in the Salesforce object.

To map a field, you enter the following:

* SDC Field - Name of the field in the record that contains the data to be written.
* Salesforce Field - API name of the existing field in the Salesforce object that receives the data. Enter a field name or enter an expression that defines the field.

Map the fields as required by the CRUD operation that the destination uses:Delete or undeleteTo delete or undelete data, map only the Salesforce record Id to delete or undelete. Create a single field mapping that maps the field in the record that contains the value of the Salesforce record Id to the Salesforce field named “Id”.Note: Undelete is not supported by the Salesforce Bulk API.Insert, update, or upsertTo insert, update, or upsert data, you can create multiple field mappings. When you define the Salesforce fields, use the field name syntax required by the configured API type:

* Bulk API - Use a colon (:) or period (.) as a field separator. For example, `Parent__r:External_Id__c` or `Parent__r.External_Id__c` are both valid Salesforce fields.
* SOAP API - Use a period (.) as a field separator. For example, `Parent__r.External_Id__c` is a valid Salesforce field.

To upsert data, you also must configure the External ID Field property that specifies the external ID field in the Salesforce object to use for the upsert operation.Note: You can also map fields to Salesforce polymorphic fields using the following Salesforce syntax: `<ObjectType>:<RelationshipName>.<IndexedFieldName>`. Because a polymorphic field can have more than one type of object as a parent, you must define the parent object type in the field name syntax. For example, a case field can have either a user or a queue as the parent. To set a case owner to a user through their username, specify the `User:Owner.Username` Salesforce field.
