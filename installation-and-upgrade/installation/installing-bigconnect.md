# Installing BigConnect Data Lake

### 1. Download installation kit

The installation kit comes in the form of a `.tgz` archive that has all components.

```bash
$ wget https://bits.bigconnect.io/downloads/bdl-<A.B>-<os>.tgz
```

`<A.B>` is the release version of BigConnect.

`<os>` is one of the supported platforms: _rhel, sles, debian, ubuntu_

**Example:**

```bash
$ wget https://bits.bigconnect.io/downloads/bdl-4.1-rhel.tgz
```

### 2. Extract the archive

The archive **must** be extracted in the `/opt` directory. 

```bash
$ tar zxf bdl-<A.B>-<os>.tgz -C /opt/
```

### 3. Start Ambari Server

```bash
$ cd /opt/bdl/ambari
$ ambari-server start
```

### ​4. Log In to Apache Ambari

1. Point your web browser to `http://<your.ambari.server>:8080`, where `<your.ambari.server>` is the name of your Ambari server host. For example, a default Ambari server host is located at `http://c7401.ambari.apache.org:8080`
2. Log in to the Ambari Server using the default user name/password: `admin/admin`. You can change these credentials later. 

For a new cluster, the Cluster Install wizard displays a Welcome page.

### 5. Launch the Cluster Install Wizard

From the Ambari Welcome page, choose **Launch Install Wizard**.  


![Ambari Cluster Install Wizard](../../.gitbook/assets/image%20%2887%29.png)

### 6. Name Your Cluster

1. In **Name your cluster**, type a name for the cluster you want to create. Use no white spaces or special characters in the name. 
2. Choose **Next**.

### 7. Select Version

In this Step, you will select the software version for your cluster. 

### **8.** Install Options

In order to build up the cluster, the Cluster Install wizard prompts you for general information about how you want to set it up. You need to supply the FQDN of each of your hosts. The wizard also needs to access the private key file you created when you set up password-less SSH. Using the host names and key file information, the wizard can locate, access, and interact securely with all hosts in the cluster.

1. In **Target Hosts**, enter your list of host names, one per line. You can use ranges inside brackets to indicate larger sets of hosts. For example, for host01.domain through host10.domain use `host[01-10].domain`
2. If you want to let Ambari automatically install the Ambari Agent on all your hosts using SSH, select **Provide your SSH Private Key** and either use the **Choose File** button in the **Host Registration Information** section to find the private key file that matches the public key you installed earlier on all your hosts or cut and paste the key into the text box manually.
3. Enter the user name for the SSH key you have selected. If you do not want to use `root`, you must provide the user name for an account that can execute `sudo` without entering a password. If SSH on the hosts in your environment is configured for a port other than 22, you can change that also.
4. If you do not want Ambari to automatically install the Ambari Agents, select **Perform manual registration**.
5. Coose **Register and Confirm** to continue.

### 9. Confirm Hosts

**Confirm Hosts** prompts you to confirm that Ambari has located the correct hosts for your cluster and to check those hosts to make sure they have the correct directories, packages, and processes required to continue the install.

If any hosts were selected in error, you can remove them by selecting the appropriate checkboxes and clicking the grey **Remove Selected** button. To remove a single host, click the small white **Remove** button in the Action column.

At the bottom of the screen, you may notice a yellow box that indicates some warnings were encountered during the check process. For example, your host may have already had a copy of `wget` or `curl`. Choose **Click here to see the warnings** to see a list of what was checked and what caused the warning. The warnings page also provides access to a python script that can help you clear any issues you may encounter and let you run

{% hint style="info" %}
If Ambari Agents fail to register with Ambari Server during the Confirm Hosts step in the Cluster Install wizard. Click the Failed link on the Wizard page to display the Agent logs. The following log entry indicates the SSL connection between the Agent and Server failed during registration:

_INFO 2014-04-02 04:25:22,669 NetUtil.py:55 - Failed to connect to https://:8440/cert/ca due to \[Errno 1\] \_ssl.c:492: error:100AE081:elliptic curve routines:EC\_GROUP\_new\_by\_curve\_name:unknown group_
{% endhint %}

When you are satisfied with the list of hosts, choose **Next**.

### 10. Choose Services

In this step you are presented with the choice of Services to install into the cluster. A Stack comprises many services. You may choose to install any other available services now, or to add services later. The Cluster Install wizard selects all available services for installation by default.

SmartSense deployment is mandatory. You cannot clear the option to install SmartSense using the Cluster Install wizard.

To choose the services that you want to deploy:

1. Choose **none** to clear all selections, or choose **all** to select all listed services
2. Choose or clear individual check boxes to define a set of services to install now.
3. After selecting the services to install now, choose **Next**.

### 11. Assign Masters

The Cluster Install wizard assigns the master components for selected services to appropriate hosts in your cluster and displays the assignments in Assign Masters. The left column shows services and current hosts. The right column shows current master component assignments by host, indicating the number of CPU cores and amount of RAM installed on each host.

1. To change the host assignment for a service, select a host name from the drop-down menu for that service.
2. To remove a ZooKeeper instance, click the green **-** icon next to the host address you want to remove.
3. When you are satisfied with the assignments, choose **Next**.

### 12. Assign Slaves and Clients

The Cluster Install wizard assigns the slave components, such as DataNodes, NodeManagers, and RegionServers, to appropriate hosts in your cluster. It also attempts to select hosts for installing the appropriate set of clients.

1. Use **all** or **none** to select all of the hosts in the column or none of the hosts, respectively.

   If a host has an asterisk next to it, that host is also running one or more master components. Hover your mouse over the asterisk to see which master components are on that host.

2. Fine-tune your selections by using the check boxes next to specific hosts.
3. When you are satisfied with your assignments, choose **Next**.

### 13. Customize Services

The Customize Services step presents you with a set of tabs that let you review and modify your cluster setup. The Cluster Install wizard attempts to set reasonable defaults for each of the options. You are **strongly encouraged** to review these settings as your requirements might be more advanced.

![](../../.gitbook/assets/image%20%2853%29.png)

Ambari will group the commonly customized configuration elements together into four categories: Credentials, Databases, Directories, and Accounts. All other configuration will be available in the All Configurations section of the Installation Wizard.

**Credentials**

Passwords for administrator and database accounts are grouped together for easy input. Depending on the services chosen, you will be prompted to input the required passwords for each item, and have the option to change the username used for administrator accounts**.**

**Databases**

Some services require a backing database to function. For each service that has been chosen for install that requires a database, you will be asked to select which database should be used and configure the connectivity details for the selected database.

{% hint style="info" %}
By default, Ambari installs a new MySQL instance. If you plan to use existing databases for MySQL/MariaDB, Oracle or PostgreSQL, modify the database type and host before proceeding. For a quick example on creating external databases on MariaDB, see Example: Install MariaDB for use with multiple components, in Administering Ambari.
{% endhint %}

{% hint style="danger" %}
Using the Microsoft SQL Server or SQL Anywhere database options are not supported.
{% endhint %}

**Directories**

Choosing the right directories for data and log storage is critical. Ambari chooses reasonable defaults based on the mount points available in your environment but you are strongly encouraged to review the default directory settings recommended by Ambari. In particular, confirm directories such as /tmp and /var are not being used for HDFS NameNode directories and DataNode directories under the HDFS tab.

**Accounts**

The service account users and groups are also configurable from the Accounts tab. These are the operating system accounts the service components will run as. If these users do not exist on your hosts, Ambari will automatically create the users and groups locally on the hosts. If these users already exist, Ambari will use those accounts.

Depending on how your environment is configured, you might not allow groupmod or usermod operations. If this is the case, there are multiple options to choose how Ambari should handle user creation and modification:

> **Use Ambari to Manage Service Accounts and Groups**
>
> Ambari will create the service accounts and groups that are required for each service if they do not exist in /etc/password, and in /etc/group of the Ambari Managed hosts.**Use Ambari to Manage Group  Memberships.**
>
> Ambari will add or remove the service accounts from groups.**Use Ambari to Manage Service Accounts UID's.**
>
> Ambari will be able to change the UID’s of all service accounts.

**All Configurations**

Here you have an opportunity to review and revise the remaining configurations for your services. Browse through each configuration tab. Hovering your cursor over each of the properties, displays a brief description of what the property does. The number of service tabs shown here depends on the services you decided to install in your cluster. **Any service with configuration issues that require attention will show up in the bell icon with the number properties that need attention.**

![](../../.gitbook/assets/image%20%2836%29.png)

The bell popover contains configurations that require your attention, configurations that are highly recommended to be reviewed and changed, and configurations that will be automatically changed based on Ambari’s recommendations unless you choose to opt out of those changes. Required Configuration must be addressed in order to proceed on to the next step in the Wizard. Carefully review the required and recommended settings and address issues before proceeding**.**

After you complete Customizing Services, choose **Next**.

### **14.** Review

**Review** displays the assignments you have made. Check to make sure everything is correct. If you need to make changes, use the left navigation bar to return to the appropriate screen.

To print your information for later reference, choose **Print**. 

To export the blueprint for this cluster, choose **Generate Blueprint**.

When you are satisfied with your choices, choose **Deploy**.

### 15. Install, Start and Test

The progress of the install displays on the screen. Ambari installs, starts, and runs a simple test on each component. Overall status of the process displays in progress bar at the top of the screen and host-by-host status displays in the main section. Do not refresh your browser during this process. Refreshing the browser may interrupt the progress indicators.

To see specific information on what tasks have been completed per host, click the link in the **Message** column for the appropriate host. In the **Tasks** pop-up, click the individual task to see the related log files. You can select filter conditions by using the **Show** drop-down list. To see a larger version of the log contents, click **Open** or to copy the contents to the clipboard, use **Copy**.

When `Successfully installed and started the services` appears, choose **Next**.

### 16. Complete

The **Summary** page provides you a summary list of the accomplished tasks. Choose **Complete**. Ambari Web opens in your web browser.

