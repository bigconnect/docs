# Verifying public host name configuration

You can use the Ambari REST API to double-check that Ambari has associated the public host name configured for the Agent.

1. Log in to Ambari.
2. In your web browser, open a new tab.
3. In the new tab Enter the following URL to look at what has been configured for a specific host: `http://ambari.server:8080/api/v1/hosts/your.hosts.fqdn`
4. Look for 'public\_host\_name' in the JSON returned from that request to ensure that the correct public host name has been configured for the host in question.



