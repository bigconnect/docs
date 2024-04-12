# System access control

A system access control enforces authorization at a global level, before any connector level authorization. You can use one of the built-in implementations in Trino, or provide your own by following the guidelines in System access control.

To use a system access control, add an `etc/access-control.properties` file with the following content and the desired system access control name on all cluster nodes:

```
access-control.name=allow-all
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Multiple system access control implementations may be configured at once using the `access-control.config-files` configuration property. It should contain a comma separated list of the access control property files to use (rather than the default `etc/access-control.properties`).

Trino offers the following built-in system access control implementations:

| Name        | Description                                                                                                                                                           |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `default`   | <p>All operations are permitted, except for user impersonation and triggering Graceful shutdown.</p><p>This is the default access control if none are configured.</p> |
| `allow-all` | All operations are permitted.                                                                                                                                         |
| `read-only` | Operations that read data or metadata are permitted, but none of the operations that write data or metadata are allowed.                                              |
| `file`      | Authorization rules are specified in a config file. See File-based access control.                                                                                    |
| `opa`       | Use Open Policy Agent (OPA) for authorization. See Open Policy Agent access control.                                                                                  |

If you want to limit access on a system level in any other way than the ones listed above, you must implement a custom System access control.

Access control must be configured on the coordinator. Authorization for operations on specific worker nodes, such a triggering Graceful shutdown, must also be configured on all workers.

### Read only system access control[#](https://trino.io/docs/current/security/built-in-system-access-control.html#read-only-system-access-control) <a href="#read-only-system-access-control" id="read-only-system-access-control"></a>

This access control allows any operation that reads data or metadata, such as `SELECT` or `SHOW`. Setting system level or catalog level session properties is also permitted. However, any operation that writes data or metadata, such as `CREATE`, `INSERT` or `DELETE`, is prohibited. To use this access control, add an `etc/access-control.properties` file with the following contents:

```
access-control.name=read-only
```
