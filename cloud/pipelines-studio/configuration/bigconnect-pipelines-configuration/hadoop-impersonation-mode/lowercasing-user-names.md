# Lowercasing User Names

When BigConnect Pipelines impersonates Hadoop users to perform tasks in Hadoop systems, you can also configure BigConnect Pipelines to lowercase all user names before passing them to Hadoop.

When the Hadoop system is case-sensitive and the user names are lowercase, you might use this property to lowercase mixed-case user names that might be returned, for example, from a case-insensitive LDAP system.

To lowercase user names before passing them to Hadoop, uncomment the `stage.conf_hadoop.always.lowercase.user` property and set it to true.
