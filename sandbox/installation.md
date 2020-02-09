# Installation

## 1. System Requirements

Supported Operating Systems:

* RHEL/Centos 7 64bit
* Ubuntu 16, Ubuntu 18 64bit

Hardware requirements:

* Minimum RAM: 12GB
* Minimum CPU cores: 4
* Minimum available disk space in /opt : 5 GB

## 2. Install Prerequisites

{% hint style="info" %}
Make sure that you have **root** access or an OS user with **sudo** rights for installing pre-requisites.
{% endhint %}

{% hint style="info" %}
The instructions for each operating system below assume that you start with a fresh 64-bit OS minimal installation.
{% endhint %}

{% tabs %}
{% tab title="RHEL/Centos 7" %}
1.Change OS limits and disable firewall:

```text
echo "fs.file-max = 100000" | sudo tee -a /etc/sysctl.conf
echo "vm.max_map_count = 262144" | sudo tee -a /etc/sysctl.conf
echo "* soft nofile 65535" | sudo tee -a /etc/security/limits.conf
echo "* hard nofile 65535" | sudo tee -a /etc/security/limits.conf
echo "* soft nproc 65535" | sudo tee -a /etc/security/limits.conf
echo "* hard nproc 65535" | sudo tee -a /etc/security/limits.conf
sudo sysctl -p

sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

2. Add your hostname to the /etc/hosts file and make sure the following command returns a valid ip:

```text
ping `hostname`
```

3. Reboot the machine.

4. Install required OS packages:

```text
sudo rpm -v --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
sudo rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm

sudo tee /etc/yum.repos.d/MariaDB.repo<<EOF 
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.4/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
EOF

sudo yum install -y epel-release wget
sudo yum install -y rabbitmq-server ffmpeg nmap-ncat.x86_64 python3
sudo yum install -y MariaDB-server MariaDB-client
```

5. Configure RabbitMQ:

```text
sudo systemctl enable --now rabbitmq-server
sudo rabbitmq-plugins enable rabbitmq_management
sudo systemctl restart rabbitmq-server
sudo rabbitmqctl add_user admin admin
sudo rabbitmqctl set_user_tags admin administrator
sudo rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
```

6. Configure MariaDB to start automatically:

```text
sudo systemctl enable --now mariadb
```

7. Create the bdl OS user with sudo rights and BDL installation folder:

```text
sudo useradd bdl
sudo usermod -aG wheel bdl
sudo mkdir -p /opt/bdl
sudo chown -R bdl:bdl /opt/bdl
```

8. Set the password for the bdl user \(you will be asked for it later on\):

```text
passwd bdl
```
{% endtab %}

{% tab title="Ubuntu 16" %}
1.Change OS limits and disable firewall:

```text
echo "fs.file-max = 100000" | sudo tee -a /etc/sysctl.conf
echo "vm.max_map_count = 262144" | sudo tee -a /etc/sysctl.conf
echo "* soft nofile 65535" | sudo tee -a /etc/security/limits.conf
echo "* hard nofile 65535" | sudo tee -a /etc/security/limits.conf
echo "* soft nproc 65535" | sudo tee -a /etc/security/limits.conf
echo "* hard nproc 65535" | sudo tee -a /etc/security/limits.conf
echo "session required pam_limits.so" | sudo tee -a /etc/pam.d/common-session
sudo sysctl -p

sudo ufw disable
```

2. Add your hostname to the /etc/hosts file and make sure the following command returns a valid ip:

```text
ping `hostname`
```

2. Reboot the machine.

3. Install required OS packages:

```text
sudo apt-get install -y rabbitmq-server

sudo add-apt-repository -y ppa:jonathonf/ffmpeg-4
sudo apt-get update -y
sudo apt-get install -y ffmpeg software-properties-common build-essential libssl-dev libffi-dev python3-dev

sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
sudo add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://nyc2.mirrors.digitalocean.com/mariadb/repo/10.2/ubuntu xenial main'
sudo apt-get update -y
sudo apt-get install -y mariadb-server mariadb-client
# hit Enter when being asked for the MariaDB root password
```

4. Configure RabbitMQ:

```text
sudo systemctl enable --now rabbitmq-server
sudo rabbitmq-plugins enable rabbitmq_management
sudo systemctl restart rabbitmq-server
sudo rabbitmqctl add_user admin admin
sudo rabbitmqctl set_user_tags admin administrator
sudo rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
```

5. Configure MariaDB to start automatically:

```text
sudo systemctl enable --now mariadb
```

6. Create the bdl OS user with sudo rights and BDL installation folder:

```text
sudo adduser bdl
sudo usermod -aG sudo bdl
sudo mkdir -p /opt/bdl
sudo chown -R bdl:bdl /opt/bdl
```
{% endtab %}

{% tab title="Ubuntu 18" %}
1.Change OS limits and disable firewall:

```text
echo "fs.file-max = 100000" | sudo tee -a /etc/sysctl.conf
echo "vm.max_map_count = 262144" | sudo tee -a /etc/sysctl.conf
echo "* soft nofile 65535" | sudo tee -a /etc/security/limits.conf
echo "* hard nofile 65535" | sudo tee -a /etc/security/limits.conf
echo "* soft nproc 65535" | sudo tee -a /etc/security/limits.conf
echo "* hard nproc 65535" | sudo tee -a /etc/security/limits.conf
echo "session required pam_limits.so" | sudo tee -a /etc/pam.d/common-session
sudo sysctl -p

sudo ufw disable
```

2. Add your hostname to the /etc/hosts file and make sure the following command returns a valid ip:

```text
ping `hostname`
```

2. Reboot the machine.

3. Install required OS packages:

```text
wget -O- https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc | sudo apt-key add -
echo "deb https://packages.erlang-solutions.com/ubuntu bionic contrib" | sudo tee -a /etc/apt/sources.list.d/rabbitmq.list
wget -O- https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc | sudo apt-key add -
wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | sudo apt-key add -
echo "deb https://dl.bintray.com/rabbitmq/debian $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/rabbitmq.list
sudo apt update -y
sudo apt -y install erlang rabbitmq-server

sudo add-apt-repository -y ppa:jonathonf/ffmpeg-4
sudo apt-get update -y
sudo apt-get install -y ffmpeg software-properties-common python3-distutils

sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
sudo add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://nyc2.mirrors.digitalocean.com/mariadb/repo/10.2/ubuntu xenial main'
sudo apt-get update -y
sudo apt-get install -y mariadb-server mariadb-client
```

4. Configure RabbitMQ:

```text
sudo systemctl enable --now rabbitmq-server
sudo rabbitmq-plugins enable rabbitmq_management
sudo systemctl restart rabbitmq-server
sudo rabbitmqctl add_user admin admin
sudo rabbitmqctl set_user_tags admin administrator
sudo rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
```

5. Configure MariaDB to start automatically:

```text
sudo systemctl enable --now mariadb
```

6. Create the bdl OS user with sudo rights and BDL installation folder:

```text
sudo adduser bdl
sudo usermod -aG sudo bdl
sudo mkdir -p /opt/bdl
sudo chown -R bdl:bdl /opt/bdl
```
{% endtab %}
{% endtabs %}

## 3. Install BDL

1.Login as the bdl user

```text
sudo su - bdl
```

4.Download the installation kit \(_X.Y.Z is the BDL version number_\) and extract the archive to the **/opt/bdl** folder:

```text
curl -o /tmp/bdl.tgz https://bits.bigconnect.io/downloads/bdl-X.Y.Z.tgz
tar -zxf /tmp/bdl.tgz -C /opt
```

5.Setup the BDL python environment and create BDL databases:

```text
/opt/bdl/lib/setup-python-env.sh
sudo mysql < /opt/bdl/lib/bdl/init/create-dbs.sql
```

See the next page for how to start/stop the BDL infrastructure.

