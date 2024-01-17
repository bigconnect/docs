# DP RPC Pipelines

Remote Protocol Call pipelines, called DP RPC pipelines, are a set of pipelines that pass data from one pipeline to another without writing to an intermediary system.

DP RPC pipelines can write to each other on the same machine or over a local network or the public internet. You might use DP RPC pipelines to send data securely between two data centers.

Typically, a pipeline uses a standard origin such as Directory and writes to a standard destination such as HBase. An DP RPC pipeline includes an DP RPC destination or an DP RPC origin to communicate with another DP RPC pipeline.

To use RPC pipelines, you create an origin pipeline and a destination pipeline. The origin pipeline uses an RPC destination to write directly to an RPC origin in the destination pipeline, as follows:

<figure><img src="https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/SDC_RPCpipelines.png" alt=""><figcaption></figcaption></figure>

The SDC RPC destination and SDC RPC origin enable you to pass data securely from one pipeline to another, effectively like creating a single pipeline that spans a network.

### Pipeline Types <a href="#ariaid-title3" id="ariaid-title3"></a>

You can create two kinds of RPC pipelines:

**origin pipeline**

Processes data from the origin system and passes it to the destination pipeline.

Uses an DP RPC destination to pass data to a destination pipeline. To provide redundancy and load-balancing, you can define connections to multiple destination pipelines.

**destination pipeline**

Processes data from the origin pipeline and passes it to the destination system.

Uses an DP RPC origin to process data from the origin pipeline.

### Deployment Architecture <a href="#ariaid-title4" id="ariaid-title4"></a>

When using DP RPC pipelines, consider your needs and environment carefully as you design the deployment architecture.

One key point to consider:Use multiple destination pipelines to improve downstream availability

To ensure downstream availability, you generally want to deploy multiple destination pipelines.

Note that the origin pipeline writes data to a single destination pipeline, but can round-robin through multiple pipelines. By using multiple destination pipelines, you can provide redundancy and avoid bottlenecks with a high-volume origin pipeline:

<figure><img src="https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/SDC_RPCarch-oneSet.png" alt=""><figcaption></figcaption></figure>

If you have multiple pipelines with similar data, you might deploy several sets of this model to provide load-balancing and redundancy:

<figure><img src="https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/SDC_RPCarch-manySets.png" alt=""><figcaption></figcaption></figure>

### Configuring the Delivery Guarantee <a href="#ariaid-title5" id="ariaid-title5"></a>

The delivery guarantee determines when a pipeline commits the offset. When configuring the delivery guarantee for RPC pipelines, use the same option in origin and destination pipelines.

A set of RPC pipelines process data like a single pipeline: The origin pipeline creates a batch, passes it through the pipeline, then passes it to the destination pipeline. Only when the destination pipeline writes the batch to its destination systems does the Data Pipelines commit the offset. As with standard pipelines, you can use the delivery guarantee property to define how the Data Pipelines commits the offset:

* Use At Least Once in both pipelines to ensure the pipelines process all data.
* Use At Most Once in both pipelines to avoid the possible duplication of data.

Note: If the RPC pipelines are configured to use different delivery guarantees, the resulting behavior is At Most Once.

For more information about delivery guarantee, see Delivery Guarantee.

### Defining the RPC ID <a href="#ariaid-title6" id="ariaid-title6"></a>

The RPC ID is a user-defined identifier that allows an RPC origin and RPC destination to recognize each other.

To enable the RPC destination to write directly to an RPC origin, use the same RPC ID in both stages.

To write to multiple pipelines, use the RPC ID in the RPC destination and all related RPC origins.

### Enabling Encryption <a href="#ariaid-title7" id="ariaid-title7"></a>

You can enable RPC pipelines to transfer data securely using SSL/TLS. To use SSL/TLS, enable TLS in both the RPC destination and the RPC origin.

You need the following information to enable TLS in both stages:

* A keystore file with the private keys of the TLS certificate, and the password for the file.
* For self-signed certificates, a truststore file with the public key of the TLS certificate of the RPC origin, and the password for the file.

To enable TLS:

1. Save the keystore and the truststore files in the Data Pipelines resources directory, $SDC\_RESOURCES.
2. In the RPC destination, enter the truststore file name and password when using a self-signed certificate.
3. In the RPC origin, enter the keystore file name and password.

### Configuration Guidelines for RPC Pipelines <a href="#ariaid-title8" id="ariaid-title8"></a>

To create a valid set of RPC pipelines, some configuration options must be aligned. Use the following guidelines to configure RPC pipelines:

origin pipelineIn the origin pipeline, configure the following elements:

1. In the pipeline properties, set the Delivery Guarantee to the option you want to use.
2. In the pipeline, use the RPC destination.
3.  When you configure the RPC destination, enter the RPC ID and RPC connection information.

    To use multiple destination pipelines, enter the RPC connection information for each RPC origin that you want to use.
4. To use encryption to transfer data between the pipelines, enable TLS and configure related properties.

destination pipelineFor each destination pipeline, configure the following elements:

1. In the pipeline properties, set the Delivery Guarantee to the same option as the origin pipeline. Using a different option causes the Data Pipelines to default to At Most Once behavior.
2. In the pipeline, use the RPC origin.
3. When you configure the RPC origin, define the properties as follows:
   * Use the same RPC ID used in the RPC destination.
   *   For the RPC Listening Port, enter the appropriate port number.

       It should be a port number specified in one of the RPC connections in the RPC destination.
   * To use encryption to transfer data between the pipelines, enable TLS and configure related properties.
