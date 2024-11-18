# Security

We take security very seriously  - ensuring that your data is safe is crucial.

#### Does the system Store My Data?

The system only stores your actual row data when using the internal database. This is stored on a highly secure, scalable and fault tolerant multi-node couchDB cluster running in AWS, with the latest EC2 security updates.

When using external datasources, it works entirely as a proxy, and calls the datasource directly, returning the results to the client.

Database credentials are always applied server side, and will never be exposed in the UI.

The self-hosted version can be deployed entirely inside your own network, on your own servers. When running self-hosted, you have total control over how you want to secure your data, and it never has to leave your VPC.

#### How We Secure the system

* Force HTTPS connections and data-in-transit encryption in cloud with TLS.
* Full Audit log tracking in cloud for our internal systems.
* Regular penetration tests and AWS security configuration audits from 3rd party vendors
* Very regular data backups and snapshots in cloud coupled with consistent database replication to minimise risk of data loss
* Host all servers in Ireland in SOC 1/2 and ISO 27001 certified datacenters.
* 2FA enabled for internal cloud access.
