# Security

An important feature of the BigConnect Core is that it includes a layer of fine-grained data security: every operation on the data structure is made using a set of Authorizations and each piece of data has a Visibility label attached to it.&#x20;

The Visibility is used to determine whether a given user meets the security requirements to read the value.  This enables data of various security levels to be stored in the same element (vertex or edge) and users of varying degrees of access to query the data, while preserving data confidentiality.

When changes to the graph are made, users can specify a visibility label for each value. These labels consist of a set of user-defined tokens that are required to read the value the label is associated with.  The set of tokens required can be specified using syntax that supports logical AND `&` and OR `|` combinations of terms, as well as nesting groups `()` of terms together.

Each term is comprised of one to many alpha-numeric characters, hyphens, underscores or periods. Optionally, each term may be wrapped in quotation marks which removes the restriction on valid characters. In quoted terms, quotation marks and backslash characters can be used as characters in the term by escaping them with a backslash.

For example, suppose within our organization we want to label our data values with security labels defined in terms of user roles. We might have tokens such as: _admin, audit, system_

`// Users must have admin privileges`\
`admin`\
\
`// Users must have admin AND audit privileges`\
`admin & audit`\
\
`// Users with either admin OR audit privileges`\
`admin | audit`\
\
`// Users must have audit and one or both of admin or system`\
`(admin|system)&audit`

When both `|` and `&` operators are used, parentheses must be used to specify precedence of the operators.

When clients attempt to read data from BigConnect,  any security labels present are examined against the set of authorizations passed by the client code.  If the authorizations are determined to be insufficient to satisfy the visibility label, the value is suppressed from the set of results sent back to the client.

Authorizations are specified as a comma-separated list of tokens the user possesses:

```java
Authorization auths = new Authorizations("admin","system");
```

## Audit

BigConnect provides an options to audit all users actions. Audit data can be logged or persisted in the data store in a special audit table.
