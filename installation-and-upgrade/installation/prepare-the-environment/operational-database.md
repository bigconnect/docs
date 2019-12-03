# Operational Database

Components like Druid and Hive require an operational database to store metadata. You can use either MySQL, Postgres, Oracle or MariaDB, but we do recommend MySQL and we provide a default configuration for MySQL.

### Install MySQL

You can install MySQL 5.5 or later. On the Ambari host, install the JDBC driver for MySQL, and then add it to Ambari:

```text
yum install mysql-connector-java* \
sudo ambari-server setup --jdbc-db=mysql \
--jdbc-driver=/usr/share/java/mysql-connector-java.jar
```

1.Log in to the node on which you want to install the MySQL metastore.

2. Install MySQL and the MySQL community server, and start the MySQL service:

```text
yum localinstall \
https://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm

yum install mysql-community-server

systemctl start mysqld.service
```

3. Obtain the randomly generated MySQL root password.

```text
grep 'A temporary password is generated for root@localhost' \
/var/log/mysqld.log |tail -1
```

4. Reset the MySQL root password. Enter the following command. You are prompted for the password you obtained in the previous step. MySQL then asks you to change the password.

```text
/usr/bin/mysql_secure_installation
```

### Create databases

1. Launch the MySQL CLI:

```text
mysql -u root -p
```

2. Create databases and users:

```text
CREATE DATABASE druid DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE hive DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;

CREATE USER 'druid'@'%' IDENTIFIED BY '<password>';
GRANT ALL PRIVILEGES ON druid.* TO 'druid'@'%' WITH GRANT OPTION;

CREATE USER 'hive'@'%' IDENTIFIED BY '<password>';
GRANT ALL PRIVILEGES ON hive.* TO 'hive'@'%' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```



