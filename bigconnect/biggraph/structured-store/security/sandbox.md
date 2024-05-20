# Sandbox

Ignite allows executing custom logic via various APIs including compute tasks, event filters, message listeners. This user-defined logic can utilize Java APIs to get access to host resources. For example, it can create/update/delete files or system properties, open network connections, use reflection and other APIs to get full control of the host environment. Ignite Sandbox is based on the [Java Sandbox model](https://docs.oracle.com/en/java/javase/11/security/java-se-platform-security-architecture.html#GUID-C203D80F-C730-45C3-AB95-D4E61FD6D89C) and allows you to restrict the scope of user-defined logic executed via Ignite APIs.

### Ignite Sandbox Activation

The activation of Ignite Sandbox involves the configuration of an `SecurityManager` instance and creation of an `GridSecurityProcessor` implementation.

#### Install SecurityManager

Due to the fact, that Ignite Sandbox is based on the Java Sandbox model, and [SecurityManager](https://docs.oracle.com/javase/8/docs/technotes/guides/security/spec/security-spec.doc6.html#a19349) is an important part of that model, you need to have it installed. The SecurityManager is responsible for checking, which security policy is currently in effect. It also performs access control checks. The security manager is not automatically installed when an application is running. If you run Ignite as a separate application, you must invoke the Java Virtual Machine with the `-Djava.security.manager` command-line argument (which sets the value of the `java.security.manager property`). There is also a `-Djava.security.policy` command-line argument, defining, which policy files are utilized. If you don’t include `-Djava.security.policy` into the command line, then the policy files specified in the security properties file will be used.

#### Provide GridSecurityProcessor Implementation

Currently, Apache Ignite does not provide an implementation of the `GridSecurityProcessor` interface out-of-the-box. But, you can implement this interface as a part of a custom plugin.

The `GridSecurityProcessor` interface has the `sandboxEnabled` method that manages a user-defined code execution inside the Ignite Sandbox. By default, this method returns `false`, which means no-sandbox. If you are going to use Ignite Sandbox, your overridden `sandboxEnabled` method needs to return `true`.

If the Ignite Sandbox is turned on, you can see the following trace line:

```
[INFO] Security status [authentication=on, sandbox=on, tls/ssl=off]
```

### Permissions

A user-defined code is always executed on behalf of the security subject that initiates its execution. The security subject’s sandbox [permissions](https://docs.oracle.com/en/java/javase/11/security/java-se-platform-security-architecture.html#GUID-DEA8EAB1-CF00-4658-AA6D-D2C9754C8B37) define actions that a user-defined code can perform. The Ignite Sandbox retrieves those permissions using the `SecuritySubject#sandboxPermissions` method.

If a security subject doesn’t have enough permissions to perform a security-sensitive operation, an AcccessControlException appears.

```java
// Get compute instance over all nodes in the cluster.
IgniteCompute compute = Ignition.ignite().compute();

compute.broadcast(() -> {
    // If the Ignite Sandbox is turned on, the lambda code is executed with restrictions.

    // You can use the public API of Ignite without granting any permissions.
    Ignition.localIgnite().cache("some.cache").get("key");

    // If the current security subject doesn't have the java.util.PropertyPermission("secret.property", "read") permission,
    // a java.security.AccessControlException appears here.
    System.getProperty("secret.property");
});
```

In the case of accessing the system property shown in the snippet above, you can see the following trace line with an exception:

```
java.security.AccessControlException: access denied ("java.util.PropertyPermission" "secret.property" "read")
```

\
