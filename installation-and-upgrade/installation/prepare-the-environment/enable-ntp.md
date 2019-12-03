# Enable NTP

The clocks of all the nodes in your cluster and the machine that runs the browser through which you access the Ambari Web interface must be able to synchronize with each other.

To install the NTP service and ensure it's ensure it's started on boot, run the following commands on each host:

**RHEL/CentOS/Oracle 7**

```text
yum install -y ntp
systemctl enable ntpd
```

**SLES**

```text
zypper install ntp
chkconfig ntp on
```

**Ubuntu**

```text
apt-get install ntp
update-rc.d ntp defaults
```

**Debian**

```text
apt-get install ntp
update-rc.d ntp defaults
```

