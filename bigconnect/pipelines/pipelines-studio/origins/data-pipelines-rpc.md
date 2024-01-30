# Data Pipelines RPC

The Data Pipelines RPC origin enables connectivity between two Data Pipelines RPC pipelines. The Data Pipelines RPC origin reads data passed from an Data Pipelines RPC destination. Use the Data Pipelines RPC origin as part of an Data Pipelines RPC destination pipeline.

You can use Data Pipelines RPC pipelines across a single Data Pipelines instance on a single machine, or across a local network or the public internet to a remote Data Pipelines. You can also configure SSL/TLS properties, including default transport protocols and cipher suites.

When you configure the DP RPC origin, you define the port that the origin listens to for data. The port number must be specified in one of the RPC connections defined in the DP RPC destination.

Similarly, the RPC ID in the DP RPC origin must match the RPC ID used in the DP RPC destination.

### Configuring an DP RPC Origin <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure an DP RPC origin to process data from an DP RPC destination.&#x20;

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline.</li></ul> |
2.  On the RPC tab, configure the following properties:

    | RPC Property             | Description                                                                                                                                                                                                                                                                                   |
    | ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | RPC Listening Port       | <p>Port number to listen to for data. Must match one of the port numbers associated with the DP RPC destination that provides the data.</p><p>Note: No other pipelines or processes can already be bound to the listening port. The listening port can be used only by a single pipeline.</p> |
    | RPC ID                   | User-defined ID. Must match the RPC ID defined in the DP RPC destination.                                                                                                                                                                                                                     |
    | Max Batch Wait Time (ms) | Number of milliseconds to wait before sending a partial or empty batch.                                                                                                                                                                                                                       |
3.  Optionally, click the Advanced tab to configure advanced properties:

    | Advanced Property    | Description                                                                                                          |
    | -------------------- | -------------------------------------------------------------------------------------------------------------------- |
    | Max Record Size (MB) | <p>Maximum size for a record. Larger records are routed to the pipeline for error handling.</p><p>Default is 10.</p> |
4.  To use SSL/TLS, on the TLS tab, configure the following properties:

    | TLS Property              | Description                                                                                                                                                                                                                                                                                                                     |
    | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Use TLS                   | Enables the use of TLS.                                                                                                                                                                                                                                                                                                         |
    | Use Remote Keystore       | Enables loading the contents of the keystore from a remote credential store or from values entered in the stage properties. For more information, see Remote Keystore and Truststore.                                                                                                                                           |
    | Private Key               | Private key used in the remote keystore. Enter a credential function that returns the key or enter the contents of the key.                                                                                                                                                                                                     |
    | Certificate Chain         | <p>Each PEM certificate used in the remote keystore. Enter a credential function that returns the certificate or enter the contents of the certificate.</p><p>Using simple or bulk edit mode, click the <strong>Add</strong> icon to add additional certificates.</p>                                                           |
    | Keystore File             | <p>Path to the local keystore file. Enter an absolute path to the file or a path relative to the Data Pipelines resources directory: $SDC_RESOURCES.</p><p>By default, no keystore is used.</p>                                                                                                                                 |
    | Keystore Type             | <p>Type of keystore to use. Use one of the following types:</p><ul><li>Java Keystore File (JKS)</li><li>PKCS #12 (p12 file)</li></ul><p>Default is Java Keystore File (JKS).</p>                                                                                                                                                |
    | Keystore Password         | <p>Password to the keystore file. A password is optional, but recommended.</p><p></p><p>Tip: To secure sensitive information such as passwords, you can use runtime resources or credential stores.</p>                                                                                                                         |
    | Keystore Key Algorithm    | <p>Algorithm to manage the keystore.</p><p>Default is SunX509.</p>                                                                                                                                                                                                                                                              |
    | Use Default Protocols     | Uses the default TLSv1.2 transport layer security (TLS) protocol. To use a different protocol, clear this option.                                                                                                                                                                                                               |
    | Transport Protocols       | <p>TLS protocols to use. To use a protocol other than the default TLSv1.2, click the Add icon and enter the protocol name. You can use simple or bulk edit mode to add protocols.</p><p></p><p>Note: Older protocols are not as secure as TLSv1.2.</p>                                                                          |
    | Use Default Cipher Suites | Uses a default cipher suite for the SSL/TLS handshake. To use a different cipher suite, clear this option.                                                                                                                                                                                                                      |
    | Cipher Suites             | <p>Cipher suites to use. To use a cipher suite that is not a part of the default set, click the Add icon and enter the name of the cipher suite. You can use simple or bulk edit mode to add cipher suites.</p><p>Enter the Java Secure Socket Extension (JSSE) name for the additional cipher suites that you want to use.</p> |
