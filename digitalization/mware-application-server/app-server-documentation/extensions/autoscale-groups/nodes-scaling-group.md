# Nodes Scaling Group

This AutoScale extension can be used to scale a Deployment Group up and down across preconfigured SSH nodes.

### Installation

Installation is done by dropping modules into the `${PAYARA_HOME}/glassfish/modules` directory of your Payara Server installation. Ensure that you have the three core modules installed, and then download and install the following two modules.

* [Nodes Scaling Groups Core 1.0](https://nexus.payara.fish/repository/payara-artifacts/fish/payara/extensions/autoscale/groups/nodes/autoscale-groups-nodes-plugin-core/1.0/autoscale-groups-core-1.0.jar)
* [Nodes Scaling Groups Admin Console Plugin 1.0](https://nexus.payara.fish/repository/payara-artifacts/fish/payara/extensions/autoscale/groups/nodes/autoscale-groups-nodes-plugin-console/1.0/autoscale-groups-console-1.0.jar)

### Nodes Scaling Group Configuration

A nodes scaling group can be configured either via the command line or the Admin Console.

#### Command Line

The `set-nodes-scaling-group-configuration` command accepts the following parameters:

* `--deploymentgroup` - The Deployment Group that the scaling group should be attached to.
* `--config` - The config that instances created via the `scale-up` command will use.
* `--nodes` - List of comma-separated nodes that the scaling group will add or remove instances to.

The primary parameter of the command is the name of the scaling group you wish to configure.

```shell
set-nodes-scaling-group-configuration --deploymentgroup group1 --config group1-config --nodes "localhost-domain1,node1,node2" scale1
```

If the name of the scaling group you wish to configure is unknown, you can use the `list-scaling-groups` command to retrieve all scaling groups, or the `get-deployment-group-scaling-group` to get the name of the scaling group attached to a deployment group.

The `list-scaling-group` command accepts no parameters. The `get-deployment-group-scaling-group` command accepts one parameter: the name of the deployment group to query.

```shell
get-deployment-group-scaling-group group1
```

#### Admin Console

From _Deployment Groups_, select the deployment group that you wish to add a nodes scaling group for and click on the _AutoScale_ tab. This will display the active AutoScale group for the deployment group.

![Active AutoScale Group in the Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/nodes/active-nodes-scaling-group.png)

From here, click on _Nodes AutoScale Groups_, to view the current configured nodes scaling groups.

![List Nodes Scaling Groups in Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/nodes/list-nodes-scaling-groups.png)

Click on the name of the nodes scaling group that you’d like to configure, enter the details, and hit _Save_.

![Edit Nodes Scaling Group in Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/nodes/edit-nodes-scaling-group.png)

### Usage

#### Creating a Nodes Scaling Group

Creating a scaling group can be done via admin commands or from the admin console.

**Prerequisites**

* Each node’s scaling group must be attached to a deployment group, and said deployment group must not have any other scaling group attached to it.
* Each node’s scaling group requires a config which isn’t the default or server config.
* Each node’s scaling group requires at least one SSH node.
  * The default DAS node, `localhost-domain1` can also be used.
* Each nodes scaling group must have a unique name.

**Admin Command**

```shell
create-nodes-scaling-group --deploymentgroup group1 --config group1-config --nodes "localhost-domain1,node1,node2" nodes-scaler-1
```

**Admin Console**

From _Deployment Groups_, select the deployment group that you wish to add a nodes scaling group for and click on the _AutoScale_ tab. This will display the active AutoScale group for the deployment group (currently none).

![Active AutoScale Group in the Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/active-autoscale-group-empty.png)

From here, click on _Nodes AutoScale Groups_, to view the current configured nodes scaling groups.

![List Nodes Scaling Groups in Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/nodes/list-nodes-scaling-groups-empty.png)

Click on the _New_ button, and enter your desired details before hitting _OK_.

![New Nodes Scaling Groups in Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/nodes/new-nodes-scaling-group.png)

#### Scaling Up

A Nodes Scaling Group attempts to balance the instances across the configured nodes, but currently takes no other criteria for determining where the instances should be created.

|   | Scaling up is by default limited to between 1 and 100 instances per command. The upper limit can be changed by modifying the `fish.payara.autoscale.maxscale` system property in the `server-config`. |
| - | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Admin Command**

The `scale-up` command accepts a `--quantity` parameter, and is executed against your chosen _Deployment Group_ (**NOT** the scaling group).

```shell
scale-up --quantity 3 group1
```

**Admin Console**

From _Deployment Groups_, select the deployment group that you wish to scale and click on the _AutoScale_ tab. This will display the active AutoScale group for the deployment group.

Enter a quantity for how much you’d like to scale the deployment group up by, and click the _Scale Up_ button.

![Active AutoScale Group in the Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/nodes/active-nodes-scaling-group.png)

#### Scaling Down

A Nodes Scaling Group attempts to balance the instances across the configured nodes, but currently takes no other criteria for determining which instances should be stopped and removed.

|   | Scaling down is by default limited to between 1 and 100 instances per command. The upper limit can be changed by specifying the `fish.payara.autoscale.maxscale` system property in the `server-config`. |
| - | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Admin Command**

The `scale-down` command accepts a `--quantity` parameter, and is executed against your chosen _Deployment Group_ (**not** the scaling group).

```shell
scale-down --quantity 3 group1
```

**Admin Console**

From _Deployment Groups_, select the deployment group that you wish to scale and click on the _AutoScale_ tab. This will display the active AutoScale group for the deployment group.

Enter a quantity for how much you’d like to scale the deployment group down by, and click the _Scale Down_ button.

![Active AutoScale Group in the Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/nodes/active-nodes-scaling-group.png)

#### Deleting a Nodes Scaling Group

Creating a scaling group can be done via admin commands or from the admin console.

**Admin Command**

The `delete-scaling-group` command can be used to delete any AutoScale group from Payara Server. It accepts one parameter: the name of the scaling group you wish to delete.

```shell
delete-scaling-group nodes-scaler-1
```

**Admin Console**

From _Deployment Groups_, select the deployment group that you wish to delete a nodes scaling group for and click on the _AutoScale_ tab. This will display the active AutoScale group for the deployment group.

![Active AutoScale Group in the Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/nodes/active-nodes-scaling-group.png)

From here, click on _Nodes AutoScale Groups_, to view the current configured nodes scaling groups.

![List Nodes Scaling Groups in Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/nodes/list-nodes-scaling-groups.png)

Check the box of the nodes scaling group that you’d like to delete, and hit _Delete_.

#### Listing Nodes Scaling Groups

Listing all node scaling groups can be done via the admin console, or you can list all scaling groups (not necessarily nodes scaling groups) using admin commands.

**Admin Command**

The `list-scaling-groups` command can be used to list all scaling groups (not necessarily just nodes scaling groups). The command accepts no parameters.

```shell
list-scaling-groups
```

**Admin Console**

From _Deployment Groups_, select any deployment group and click on the _AutoScale_ tab. This will display the active AutoScale group for the deployment group.

![Active AutoScale Group in the Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/nodes/active-nodes-scaling-group.png)

From here, click on _Nodes AutoScale Groups_, to view the current configured nodes scaling groups.

![List Nodes Scaling Groups in Admin Console](https://docs.payara.fish/community/docs/\_images/autoscale/nodes/list-nodes-scaling-groups.png)
