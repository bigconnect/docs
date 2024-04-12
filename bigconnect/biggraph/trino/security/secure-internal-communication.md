# Secure internal communication

The Trino cluster can be configured to use secured communication with internal authentication of the nodes in the cluster, and to optionally use added security with TLS.

### Configure shared secret[#](https://trino.io/docs/current/security/internal-communication.html#configure-shared-secret) <a href="#configure-shared-secret" id="configure-shared-secret"></a>

Configure a shared secret to authenticate all communication between nodes of the cluster. Use this configuration under the following conditions:

* When opting to configure internal TLS encryption between nodes of the cluster
* When using any external authentication method between clients and the coordinator

Set the shared secret to the same value in config.properties on all nodes of the cluster:

```
internal-communication.shared-secret=<secret>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

A large random key is recommended, and can be generated with the following Linux command:

```
openssl rand 512 | base64
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Verify configuration[#](https://trino.io/docs/current/security/internal-communication.html#verify-configuration) <a href="#verify-configuration" id="verify-configuration"></a>

To verify shared secret configuration:

1. Start your Trino cluster with two or more nodes configured with a shared secret.
2. Connect to the [Web UI](https://trino.io/docs/current/admin/web-interface.html).
3. Confirm the number of `ACTIVE WORKERS` equals the number of nodes configured with your shared secret.
4. Change the value of the shared secret on one worker, and restart the worker.
5. Log in to the Web UI and confirm the number of `ACTIVE WORKERS` is one less. The worker with the invalid secret is not authenticated, and therefore not registered with the coordinator.
6. Stop your Trino cluster, revert the value change on the worker, and restart your cluster.
7. Confirm the number of `ACTIVE WORKERS` equals the number of nodes configured with your shared secret.

### Configure internal TLS[#](https://trino.io/docs/current/security/internal-communication.html#configure-internal-tls) <a href="#configure-internal-tls" id="configure-internal-tls"></a>

You can optionally add an extra layer of security by configuring the cluster to encrypt communication between nodes with [TLS](https://trino.io/docs/current/glossary.html#glosstls).

You can configure the coordinator and all workers to encrypt all communication with each other using TLS. Every node in the cluster must be configured. Nodes that have not been configured, or are configured incorrectly, are not able to communicate with other nodes in the cluster.

In typical deployments, you should enable TLS directly on the coordinator for fully encrypted access to the cluster by client tools.

Enable TLS for internal communication with the following configuration identical on all cluster nodes.

1. Configure a shared secret for internal communication as described in the preceding section.
2.  Enable automatic certificate creation and trust setup in `etc/config.properties`:

    ```
    internal-communication.https.required=true
    ```

    ![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)
3.  Change the URI for the discovery service to use HTTPS and point to the IP address of the coordinator in `etc/config.properties`:

    ```
    discovery.uri=https://<coordinator ip address>:<https port>
    ```

    ![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

    Note that using hostnames or fully qualified domain names for the URI is not supported. The automatic certificate creation for internal TLS only supports IP addresses.
4.  Enable the HTTPS endpoint on all workers.

    ```
    http-server.https.enabled=true
    http-server.https.port=<https port>
    ```

    ![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)
5. Restart all nodes.

Certificates are automatically created and used to ensure all communication inside the cluster is secured with TLS.

Warning

Older versions of Trino required you to manually manage all the certificates on the nodes. If you upgrade from this setup, you must remove the following configuration properties:

* `internal-communication.https.keystore.path`
* `internal-communication.https.truststore.path`
* `node.internal-address-source`

#### Performance with SSL/TLS enabled[#](https://trino.io/docs/current/security/internal-communication.html#performance-with-ssl-tls-enabled) <a href="#performance-with-ssl-tls-enabled" id="performance-with-ssl-tls-enabled"></a>

Enabling encryption impacts performance. The performance degradation can vary based on the environment, queries, and concurrency.

For queries that do not require transferring too much data between the Trino nodes e.g. `SELECT count(*) FROM table`, the performance impact is negligible.

However, for CPU intensive queries which require a considerable amount of data to be transferred between the nodes (for example, distributed joins, aggregations and window functions, which require repartitioning), the performance impact can be considerable. The slowdown may vary from 10% to even 100%+, depending on the network traffic and the CPU utilization.

#### Advanced performance tuning[#](https://trino.io/docs/current/security/internal-communication.html#advanced-performance-tuning) <a href="#advanced-performance-tuning" id="advanced-performance-tuning"></a>

In some cases, changing the source of random numbers improves performance significantly.

By default, TLS encryption uses the `/dev/urandom` system device as a source of entropy. This device has limited throughput, so on environments with high network bandwidth (e.g. InfiniBand), it may become a bottleneck. In such situations, it is recommended to try to switch the random number generator algorithm to `SHA1PRNG`, by setting it via `http-server.https.secure-random-algorithm` property in `config.properties` on the coordinator and all of the workers:

```
http-server.https.secure-random-algorithm=SHA1PRNG
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Be aware that this algorithm takes the initial seed from the blocking `/dev/random` device. For environments that do not have enough entropy to seed the `SHAPRNG` algorithm, the source can be changed to `/dev/urandom` by adding the `java.security.egd` property to `jvm.config`:

```
-Djava.security.egd=file:/dev/urandom
```
