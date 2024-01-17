# Geo IP

The Geo IP processor is a lookup processor that can return geolocation and IP intelligence information for a specified IP address.

The Geo IP processor uses MaxMind GeoIP2 database files for the lookup. You must provide your own copy of the database files.

Tip: MaxMind provides some free databases that you can use.

To use the Geo IP processor, you specify the location of the database files and the database types that you want to use. You enter one or more IP address input fields, name the corresponding output field, and specify the return information that you want. You can also configure the action that the processor takes if the database files do not have an IP address.

The input field must be an Integer or String data type that passes IPv4 or IPv6 addresses.

### Supported Databases <a href="#ariaid-title2" id="ariaid-title2"></a>

You can use most MaxMind GeoIP2 databases with the GeoIP processor. The processor supports the following GeoIP2 databases:

* Anonymous IP
* City
* Country
* Connection Type
* Domain
* ISP

### Database File Location <a href="#ariaid-title3" id="ariaid-title3"></a>

To use the Geo IP processor, save the MaxMind GeoIP2 database files that you want to use in a directory local to the Data Pipelines or in the Data Pipelines resources directory: $SDC\_RESOURCES.

Then, specify the location of the database file when you configure the processor.

### GeoIP Field Types <a href="#ariaid-title4" id="ariaid-title4"></a>

Each GeoIP2 database provides a different set of information that you can request. When you configure the processor, be sure to request information that exists in the databases that you are using.

For example, if you configure the processor to use the City and Country databases, do not request domain information. To return domain details, you need to use the Domain database.

In the processor, you request return values by defining the GeoIP Field Type property.

The following table lists the valid GeoIP field types for each database. For details about the information returned with each field type, see the MaxMind GeoIP2 documentation.

| Database        | Valid GeoIP Field Types                                                                                                                                             |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Anonymous IP    | <ul><li>Anonymous IP Full JSON</li><li>Is Anonymous</li><li>Is Anonymous VPN</li><li>Is Hosting Provider</li><li>Is Public Proxy</li><li>Is TOR Exit Node</li></ul> |
| City            | <ul><li>City Full JSON</li><li>City Name</li><li>Country</li><li>Country ISO Code</li><li>Latitude</li><li>Longitude</li></ul>                                      |
| Country         | <ul><li>Country</li><li>Country Full JSON</li><li>Country ISO Code</li></ul>                                                                                        |
| Connection Type | <ul><li>Connection Type</li><li>Connection Type Full JSON</li></ul>                                                                                                 |
| Domain          | <ul><li>Domain</li><li>Domain Full JSON</li></ul>                                                                                                                   |
| ISP             | <ul><li>Autonomous System Number</li><li>Autonomous System Org</li><li>ISP</li><li>ISP Full JSON</li><li>Organization</li></ul>                                     |

#### Full JSON Field Types <a href="#ariaid-title5" id="ariaid-title5"></a>

The GeoIP processor provides a Full JSON field type for each database. The Full JSON field type returns all available data in the dictionary for the specified IP address.

Use the Full JSON field type when the information that you want is in a database, but not available as one of the field types in the processor.

The Full JSON field type returns a JSON object with all available data. You can use a JSON Parser processor downstream to parse the object and extract the information that you need.

### Configuring a Geo IP Processor <a href="#ariaid-title6" id="ariaid-title6"></a>

Configure a Geo IP processor to return geolocation information based on IP addresses.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Geolocation tab, configure the following properties:

    | Geo IP Property         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | GeoIP2 Databases        | The MaxMind GeoIP databases that you want to use. Using simple or bulk edit mode, click the Add icon to add additional databases.                                                                                                                                                                                                                                                                                                                                  |
    | GeoIP2 Database File    | Directory where the GeoIP2 database file is located. Enter a fully-qualified location or the Data Pipelines resources directory, $SDC\_RESOURCES.                                                                                                                                                                                                                                                                                                                  |
    | GeoIP2 Database Type    | Database type.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Database Field Mappings | Mapping for each input field, output field, and the data that you want returned in the output field.                                                                                                                                                                                                                                                                                                                                                               |
    | Input Field Name        | Incoming field with the IP address to use. The field can be an Integer or String data type.                                                                                                                                                                                                                                                                                                                                                                        |
    | Output Field Name       | Name of the field to pass the selected geolocation data.                                                                                                                                                                                                                                                                                                                                                                                                           |
    | GeoIP2 Field            | Data from the available databases to be passed to the output field.                                                                                                                                                                                                                                                                                                                                                                                                |
    | Missing Address Action  | <p>Specify the action to take if an IP address is missing from the database file:</p><ul><li>Send to Error - Handles the record based on the error handling configured for the stage.</li><li>Replace with Nulls - Adds all of the specified output fields to the record, replacing the missing values with null.</li><li>Ignore - Ignores the missing data, and does not add the specified output fields to the record.</li></ul><p>Default is Send to Error.</p> |
3. To return additional geolocation data, click the Add icon.You can return geolocation data for the same input field or a different input field.
