# Record Deduplicator

The Record Deduplicator evaluates records for duplicate data and routes data to two streams - one for unique records and one for duplicate records. Use the Record Deduplicator to discard duplicate data or route duplicate data through different processing logic.

The Record Deduplicator can compare entire records or a subset of fields. Use a subset of fields to focus the comparison on fields of concern. For example, to discard purchases that are accidentally submitted more than once, you might compare information about the purchaser, selected items, and shipping address, but ignore the timestamp of the event.

To enhance pipeline performance, the Record Deduplicator hashes comparison fields and uses the hashed values to evaluate for duplicates. On rare occasions, hash functions can generate collisions that can cause records to be incorrectly treated as duplicates.

### Comparison Window <a href="#ariaid-title2" id="ariaid-title2"></a>

The Record Deduplicator caches record information for comparison until it reaches a specified number of records. Then, it discards the information in the cache and starts over.

You can configure a time limit to trigger a cache refresh at regular time intervals. When you configure a time limit, the time limit takes precedence over the record limit.

When you stop the pipeline, the Record Deduplicator discards all information in memory.

### Configuring a Record Deduplicator <a href="#ariaid-title3" id="ariaid-title3"></a>

Use a Record Deduplicator to route or remove records with duplicate data.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
2.  On the Deduplication tab, configure the following properties:

    | Record Deduplicator Property | Description                                                                                                                                                                                          |
    | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Compare                      | <p>Specifies the fields to compare. Use one of the following options:</p><ul><li>All Fields - Compares all fields in the record.</li><li>Specified Fields - Compares the specified fields.</li></ul> |
    | Fields to Compare            | Subset of fields to compare for duplicate data.                                                                                                                                                      |
    | Max Records to Compare       | The maximum number of records to compare. Upon reaching this number, the Record Deduplicator clears its cache.                                                                                       |
    | Time to Compare (secs)       | <p>Number of seconds to compare records. This property takes precedence over the Max Records to Compare.</p><p>Use 0 to opt out of this property.</p>                                                |
