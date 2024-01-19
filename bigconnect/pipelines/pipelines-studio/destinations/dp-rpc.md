# DP RPC

The DP RPC destination enables connectivity between two DP RPC pipelines. The DP RPC destination passes data to one or more DP RPC origins. Use the DP RPC destination as part of an SDC RPC origin pipeline.

You can use DP RPC pipelines across a single Data Pipelines instance on a single machine, or across a local network or the public internet to a remote Data Pipelines.

When you configure the DP RPC destination, you specify the RPC ID and RPC connections to use. You can optionally enable encryption to pass data securely and define retry and timeout properties. You can also configure SSL/TLS properties, including default transport protocols and cipher suites.

### RPC Connections <a href="#ariaid-title2" id="ariaid-title2"></a>

In an SDC RPC destination, the RPC connections define where the destination passes data.

The RPC connection is the host and port number of the DP RPC origin to receive the data. Define multiple connections to allow the DP RPC destination to round-robin through multiple pipelines in case of bottlenecks.

### Disabling Compression <a href="#ariaid-title3" id="ariaid-title3"></a>

The DP RPC destination compresses data by default when passing data to an DP RPC origin. When necessary, you can disable compression in the destination.

The DP RPC destination compresses data to enhance performance. When the destination processes small records, compression might not enhance pipeline performance. When tuning pipeline performance, you might try disabling compression in the destination when working with small records.
