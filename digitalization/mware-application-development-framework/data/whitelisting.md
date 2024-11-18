# Whitelisting

Firewall Requirements for external data sources

If you are using remote data sources your database server is likely protected by a firewall. To allow Mware Application Development Framework to connect to your data source you may need to whitelist IP addresses in your firewall.

![](https://files.readme.io/e02f0100245426a26ec32ebc8d48b1c3000bceb415bb0ee6fe43fa0c8da598db-Untitled\_design.png)

### Cloud

The following IP addresses are used by Mware Application Development Framework Cloud to connect to your data source. You should whitelist these IP addresses on your firewall to allow Mware Application Development Framework Cloud to access your Database Server. Your organization Database Administrator should be able to help with this.

```
34.246.43.190
34.255.55.113
3.248.69.251
18.203.209.221
```

> ### 📘Firewall Considerations
>
> Note that the IP addresses above are subject to change. We will notify registered users prior to any change to give advance notice for firewall amendments. If you notice that a previously working connection has stopped working check back here.

### Dedicated Cloud hosting

If you have a dedicated cloud hosted account you will need to whitelist this IP address in addition to the others.

```
d2l5prqdbvm3op.cloudfront.net
```

### Self-hosted

If you are self-hosting Mware Application Development Framework on your own hardware or using a third-party provider you will need to ensure that the IP address used by your is whitelisted on any firewalls between Mware Application Development Frameworkand the data source.

#### Troubleshooting Connection Issues

If you have set up the connection to your data source within a self-hosted installation and you are experiencing difficulties connecting and you have terminal access to your instance you could try:

**Can you make contact from the** Mware Application Development Framework **container or host using telnet?**

```
apt install telnet
telnet 111.222.111.222 3306
```

Attempting a telnet connection to the IP address of your datasource and the port used by the data source (e.g. 3306 for MySQL, 5432 for Postgres, 1431 for MSSQL). If a connection is not made you should check if there is a firewall protecting your data source.

**Is your database server listening for remote connections?**

For security reasons many database servers will listen only for local connections by default. To allow connections from external servers your database server may need to be configured to listen for remote connections. As an example MySQL would need a configuration parameter as shown below:

```
bind-address           = 0.0.0.0
```

You can also check your database server is listening for remote connections by analysing the output from the `netstat` or `ss` commands.

**Does your data source force particular security settings?**

Some data sources will only allow encrypted connections. Some may require valid certificates for connecting. If you are having difficulties should should check with your Database Administrator for the correct settings to use.
