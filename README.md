# Alastria Node for Alastria-T Network

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/alastria/alastria-node/blob/testnet2/LICENSE)
[![Slack Status](https://img.shields.io/badge/slack-join_chat-white.svg?logo=slack)](https://alastria.slack.com/)

Based on the Jesús Ruiz develop, [RedTValidatorNode](https://github.com/hesusruiz/RedTValidatorNode)

## Configuration & Installation: Quick Guide

* Clone or download this repository to the machine where you want to install and operate the Red T node and enter into the cloned directory.

* Edit the file `.env` and modify the lines with:

    + NODE_TYPE if your not sure what option its need, select _general_

    + NODE_NAME attribute according to the name you want for this node. The name SHOULD follow the convention:

    > `TYPE_COMPANY_T_Y_Z_NN`

    Where _TYPE_ is the rol for the node in the network (use `REG` for regular/general nodes), _XX_ is your company/entity name, Y is the number of processors of the machine, Z is the amount of memory in Gb and NN is a sequential counter for each machine that you may have (starting at 00). For example:

    > `NODE_NAME="REG_IN2_T_2_8_00"`
    > `NODE_NAME="REG_DigitelTS_T_2_8_00"`

    This is the name that will appear in the public listings of nodes of the network. It does not have any other usage.

* In the root directory of the repository (where the file `docker-compose.yml` exists) run:

```console
$ docker-compose up -d
```

* The command runs the service in the background, and you can check its activity by running:

```console
$ docker-compose logs -f --tail=20
```
  * **You're done!** 
## Performing Permisioned

You should see the node initializing and starting to try to contact peers. However, the node is not yet permissioned, so it can not participate in the blockchain network yet.

In order to perform permissioning, follow these steps:

  * Display the contents of the ENODE_ADDRESS file (the actual contents of your file will be different than in the example):

```console
$ docker exec -it REG_ExampleOrg_T_2_8_00 geth --exec "admin.nodeInfo.enode" attach /root/alastria/data/geth.ipc
```

* Get the IP address of your node, as seen from the external world. 

```console
$ curl https://ifconfig.me/
```

* Create the full enode address like:

    `enode://YOUR_ENODE@YOUR_IP:21000?discport=0`

    where

    >+ **YOUR_ENODE** is the value of the ENODE_ADDRESS file
    >+ **YOUR_IP** is the external IP of your node

* With that value, create a pull request to request permission, adding the line to the node list.

* When the pull request is accepted, you will see that your node starts connecting to its peers and starts synchronizing the blockchain. The process of synchronization can take hours or even one or two days depending on the speed of your network and machine.
## Maintaining Node

You can use the standard docker-compose commands to manage your node. For example, to stop the node:

* Stop node:
  
```console
$ docker-compose down
```

* To restart the node:

```console
$ docker-compose restart
```

* Delete current container

```console
$ docker rm REG_ExampleOrg_T_2_8_00
```

Node management is done through the geth console. It can be accessed through the following commands:

```console
$ geth attach http://localhost:22000 (in case geth were started with --rpc options)
```

```console
$ geth attach /root/alastria/data/geth.ipc
```

The commands can be invoked from the Docker client, or by accessing the container: 

```console
$ docker ps -a
$ docker exec -it <container_name> /bin/bash
```

Some useful commands:

```console
root@62369c8b018e:/usr/local/bin# geth attach /root/alastria/data/geth.ipc
Welcome to the Geth JavaScript console!

instance: Geth/REG_DigitelTS-pre_T_2_4_00/v1.8.18-stable(quorum-v2.2.3-0.Alastria_EthNetstats_IBFT)/linux-amd64/go1.9.5
coinbase: 0x1e02232b297055717e3381ad458f8b23cb9ada03
at block: 60568501 (Mon, 25 Jan 2021 21:37:51 UTC)
 datadir: /root/alastria/data
 modules: admin:1.0 debug:1.0 eth:1.0 istanbul:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0

> eth.blockNumber
> admin.peers
> admin.nodeInfo
> personal.unlockAccount(eth.accounts[0],"_your_eth0_password_",2000)
> eth.sendTransaction({from: eth.accounts[0], to: eth.accounts[0], value:0 })
> eth.syncing
> eth.mining
```

## Backup

TBD

## System requirements

| Hardware | minimum | desired |
|:------- |:-------- |:---------|
| CPU's | 2 |  4 |
| Memory | 4Gb |  8Gb |
| Hard Disk | 128 Gb |  256 Gb |

DLT database grows 1Gb/week: keep in mind for future updates.

## System ports (INPUT)

The following ports must be open, at least, to the nodes defined in the `/root/alastria/data/static-nodes.json` and `/root/alastria/data/permissioned-nodes.json` files. We recommend that these ports be universally open: the wishper protocol defined in geth / GoQuorum is robust enough to be published without the need for control through the firewall.

| Port | Type | Definition |
|:------:|:-----:|:---------- |
| 21000| TCP/UDP | Geth process application port (inbound and outbound for ethereum traffic) |
| 53 | TCP/UDP | Access to external Internet based resolvers |

`tcp/21000` and `udp/21000`port are mandatory, as is the common standard for the Red-T network.
## System ports (OUTPUT)

We strongly advise not to filter outgoing ports. If necessary, these are the destinations

| Port | Type | Definition |
|:------:|:-----:|:---------- |
| 80 | TCP | Outbound for Websockets feed to netstats server |
| 8086 | TCP | Outbound for InfluxDB statistics collection |

## Mandatory parametres

Some parametres are high hardcored in this installer, but can be change:

* Working directory: The install procedure expect use of `/root/alastria/data` as the main directory.
* Geth / Go versions: Changing the `alastria-node/Dockerfile` its easy to change the build version.
> NOTE: Using 20.10 its still experimental, as described in [UPGRADE_TO_LAST_VERSION](https://github.com/alastria/alastria-node/blob/testnet2/UPGRADE_TO_LAST_VERSION.md) but should be available soon. 

* Data directory: Because of the size that the network-T database can reach, a Docker volume has been deployed to set the storage on some independent path from the one set by the Docker installation. This parameter is set in `docker-compose.yml`, in _volumes_ tag.
* Geth parametres: Other geth options can be personalized in `geth.node.bootnode.sh`, `geth.node.general.sh` or `geth.node.validator.sh`.
## Optional parametres

### Application Ports ###
To use your node through web3 applications, some connection method must be enabled. In this case, the following connection methods are offered:

* JSON / RPC connection: you should upgrade the following files, in order to allow web3 connections; `docker-compose.yml` allow new connection from tcp/22000, or the one defined in `alastria-node-data/env/geth.common.sh` related to _RCP_ connections.
> NOTE: exposing this port should be controled by any kind of firewall, or using any proxy filtering, as proposed in [alastria-access-point](https://github.com/alastria/alastria-access-point) proyect
* WebSockets connection: please, follow this article [Connecting to an Alastria's Red T node using WebSockets](https://tech.tribalyte.eu/blog-websockets-red-t-alastria) created by Ronny Demera, from Tribalyte

## Istanbul Gobernance IBTF

TBD

## Parameters for Boot Nodes

TBD

## Parameters for Validator Nodes

TBD

## Other Resources

+ [Wiki](https://github.com/alastria/alastria-node/wiki)
+ [FAQ ES](https://github.com/alastria/alastria-node/wiki/FAQ_ES)
+ [FAQ EN](https://github.com/alastria/alastria-node/wiki/FAQ_EN)

* **Resources**

| URL | Managed by | Notes|
|:---- |:---- |:----|
| [http://netstats.telsius.alastria.io](http://netstats.telsius.alastria.io/) | Planisys | |
| [https://alastria-netstats2.planisys.net/snapshot/index.html](https://alastria-netstats2.planisys.net/snapshot/index.html) | Planisys | This is generated through Netstats API |
| [https://blkexplorer1.telsius.alastria.io](https://blkexplorer1.telsius.alastria.io) | CouncilBox | |
| [https://geth-metrics.planisys.net/](https://geth-metrics.planisys.net/) | Planisys | Influxdb generated by geth |
| [https://alastria-caliper.planisys.net/](https://alastria-caliper.planisys.net/) | Planisys | Prometheus generated by geth 1.9.7 - quorum 2.6.0


# Contributing

The following developments are in place or in backlog. Any help/volunteers are welcomed:

* HardFork to upgrade to the new EVM and the new Quorum version. `WIP`
* Netstats improvements, to allow this tool to handle a very big number of nodes.
* Validator nodes automatic round, to allow the network to make automatic change of the validator nodes without intervention of the humans administrators, in case of any fault/malfunctioning of the nodes
* HardFork to upgrade GetH binary to prepare for Gas implementation Faucet or Gas Distributor

# Help Resources

Please, use Github to contribute and collaborate on open issues that are in development on Alastria Github platform.
Do not hesitate to contact Alastria Support Team to solve any doubt in support@alastria.io

* [Slack](https://github.com/alastria/alastria-node/wiki/HELP)
* [Github (use & governance)](https://github.com/alastria/alastria-node/wiki/HELP)

# Changes from `testnet2` branch:

* Use of constellation it's no longer supported. Previous documentation can be accesed in [Constellation Using Pivate Transactions in Alastria T](https://github.com/alastria/alastria-node/wiki/Constellation---Using-Pivate-Transactions-in-Alastria-T)
* Use of nginx as proxy it's no longer supported. However, the repository alastria-node-access it's still avaiable
* Use of `monitor` its deprecated
* Use of local fork of `quorum`, https://github.com/alastria/quorum it's no longer used
