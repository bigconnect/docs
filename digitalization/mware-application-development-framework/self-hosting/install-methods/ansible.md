# Ansible

Mware Application Development Framework is deployed as a container and the container image includes all the components needed to run it. Since it is a container it will run in many scenarios and platforms where a container engine such as Docker exists. One scenario could be to run it with Docker on a Linux server. There are multiple ways to achieve this including Docker Compose and Portainer, but Ansible could also be used to configure and run a container on a Linux server as described below.

#### Prerequisites

For this example you will need:

* Ansible installed on your computer
* A server or virtual machine installed with internet connectivity and a Linux operating system - we are using Debian 11 in this example
* A valid IP address on the server with a DNS entry for a domain you own pointing to the IP address. In the example below we are using debian001.domain.com pointing to 111.222.111.222
* Password-less SSH and sudo privileges for the server, or willingness to add access details to your ansible-playbook command

#### &#x20;Folder Structure

In the example we will use the following folder structure on a laptop:

* group\_vars
* host\_vars
* playbooks
* plugins
* roles
* inventory.yaml

![](https://files.readme.io/e60ba68-image.png)

#### Install Pre-requisites

The first step might be to ensure that ansible commands work so within the folder above run the following command to install ansible support for docker containers and the role:

```
ansible-galaxy collection install community.general  
```

#### Inventory

Next, the inventory.yaml file needs information pointing to your server e.g.

```
all:  
  children:  
    app_servers:  
      hosts:  
        debian001.domain.com:  
            ansible_port: 22  
            host: debian001.domain.com
```

#### Playbook

Then add a playbook in the playbooks folder e.g. `debian001.domain.com.yaml` with content similar to the following but substituted with your own values:

```
- hosts:
  - debian001.domain.com  
    become: true  
    vars:  
      custom_domain: mydomain.com  
      jwt_secret: yourownrandomstring  
      minio_access_key: yourownrandomstring  
      minio_secret_key: yourownrandomstring  
      redis_password: yourownrandomstring  
      couchdb_user: yourownrandomstring  
      couchdb_password: yourownrandomstring  
      internal_api_key: yourownrandomstring  
    roles:
  - geerlingguy.docker
```

Note:

* If you set the environment variable `custom_domain` this will be used to request an SSL certificate from Let’s Encrypt for your domain name. So you must have DNS resolving for this to work.
* In each of the environment variables replace `yourownrandomstring` with your own passwords to secure your installation. You may also wish to encrypt these variables using Ansible Vault.

#### Run the Playbook

When you have your roles configured you can run your Ansible playbook:

```
ansible-playbook -i inventory.yaml ./playbooks/debian001.domain.com.yaml
```

#### Backups

In the examples above we used the directory /data directory to persist data if containers restart. You should ensure you have a strategy to take backups of that directory for disaster recovery and upgrades.
