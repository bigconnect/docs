# Modifying Environment Variables

The method that you use to modify environment variables depends on the BigConnect Pipelines installation type:\
\
**Tarball installation started manually from the command line**\
\
When you start BigConnect Pipelines manually from the command line on any operating system, edit the `$SDC_DIST/libexec/sdc-env.sh` file to modify environment variables.

Use a text editor to edit the `sdc-env.sh` file. Some of the environment variables in the file are commented out and do not reflect the default values. Be sure to uncomment the line when you change a variable value.

After you edit the file, restart BigConnect Pipelines from the command prompt to enable the changes.

**Note**: Do not restart BigConnect Pipelines from the user interface after modifying environment variables.\
\
**Tarball or RPM installation started as a service on operating systems that use the SysV init system**\
\
When you start BigConnect Pipelines as a service on CentOS 6, Oracle Linux 6, Red Hat Enterprise Linux 6, or Ubuntu 14.04 LTS, edit the `$SDC_DIST/libexec/sdcd-env.sh` file to modify environment variables.

Use a text editor to edit the `sdcd-env.sh` file.

After you edit the file, restart BigConnect Pipelines to enable the changes.

**Tarball or RPM installation started as a service on operating systems that use the systemd init system**\
\
When you start BigConnect Pipelines as a service on CentOS 7, Oracle Linux 7, Red Hat Enterprise Linux 7, or Ubuntu 16.04 LTS, edit the `sdc.service` file to modify environment variables.\
\
**Note**: When you install BigConnect Pipelines through a cloud service provider marketplace, BigConnect Pipelines is deployed as an RPM package on an operating system that uses the systemd init system.The location of the `sdc.service` file depends on how you installed BigConnect Pipelines:

* From the RPM package - `/usr/lib/systemd/system/sdc.service`
* From the tarball - `/etc/systemd/system/sdc.service`

Override the default values in the `sdc.service` file using the same procedure that you use to override unit configuration files on a systemd init system. For an example, see "Example 2. Overriding vendor settings" in this systemd.unit manpage.

After overriding the default values, use the following command to reload the systemd manager configuration:

```
systemctl daemon-reload
```

Then restart BigConnect Pipelines to enable the changes.

**Cloudera Manager installation**\
\
When you install BigConnect Pipelines through Cloudera Manager, modify environment variables by configuring the BigConnect Pipelines service through Cloudera Manager.\
