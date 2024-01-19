# Aerospike

The Aerospike destination writes data to Aerospike.&#x20;

The destination can write to a single Aerospike node or to a cluster of Aerospike nodes. When you configure the destination, you define the Aerospike nodes to write to and configure the number of times the destination attempts to retry connections to Aerospike.

You specify the Aerospike namespace and optional set to use. You also specify the key, or unique identifier, of the Aerospike record to write to. You then map fields in the Data Pipelines record to bins in the Aerospike record.

### Configuring an Aerospike Destination <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure an Aerospike destination to write data to Aerospike.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline.</li></ul>                                                 |
2.  On the Aerospike tab, configure the following properties:

    | Aerospike Property | Description                                                                                                                                                                                                                                  |
    | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Aerospike Nodes    | <p>Aerospike nodes to use. Use the following format:</p><pre><code>&#x3C;host name>:&#x3C;port number>
    </code></pre><p>For example: <code>localhost:3000</code></p><p>If writing to an Aerospike cluster, add each node in the cluster. </p> |
    | Retry Attempts     | <p>Maximum number of times to attempt to connect to the Aerospike database.</p><p>Default is 0, which means that the destination does not attempt any retries.</p>                                                                           |
3.  On the Mapping tab, configure the following properties:

    | Mapping Property | Description                                                                                                                                                                                                                                                                                                                                                                                            |
    | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | Namespace        | Aerospike namespace to use. Enter a namespace name or enter an expression that defines the namespace.                                                                                                                                                                                                                                                                                                  |
    | Set              | Optional Aerospike set within the namespace to use. Enter a set name or enter an expression that defines the set.                                                                                                                                                                                                                                                                                      |
    | Key              | Key, or unique identifier, of the Aerospike record to use. Enter a key name or enter an expression that defines the key.                                                                                                                                                                                                                                                                               |
    | Bins to Update   | <p>Map fields in the Data Pipelines record to bins in the Aerospike record. Enter the following:</p><ul><li>Bin Name Expression - Expression that defines the name of the Aerospike bin to write to.</li><li>Bin Value Expression - Expression that defines the value to write to the Aerospike bin.</li><li>Bin Value Type - Data type of the specified bin value: String, Long, or Double.</li></ul> |
