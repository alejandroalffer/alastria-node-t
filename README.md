# Alastria Node for Alastria-T Network

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/alastria/alastria-node/blob/testnet2/LICENSE)
[![Slack Status](https://img.shields.io/badge/slack-join_chat-white.svg?logo=slack)](https://alastria.slack.com/)

Based on the work of:
* Jesús Ruiz, https://github.com/hesusruiz
* Marcos Serradilla Diez, https://github.com/marcosio 
* Alfonso de la Rocha, https://github.com/adlrocha
* ... and many other contributors to the Alastria ecosystem


Alastria-T Network is a `GoQuorum` network that uses the IBFT consensus algorithm 
## Configuration & Installation: Quick Guide for docker-compose

In Alastria-T network there are 3 types of nodes.

* Validators: They are in charge of the mining and validation of the blocks using the IBFT consensus algorithm.
* Bootnodes: They are responsible for the permission of the nodes in the network.
* Regular: They are responsible for accepting transactions, verifying them and delivering them to the “validator”. These kind of nodes are use for the interaction from `web3` and `Smart Contracts`, and should be the option for deploy uses cases of Blockchain. 

The following process explain the installation for a Regular (also called _general_) nodes:

* Clone or download this repository to the machine where you want to install and operate the Red T node and enter into the cloned directory.

* :fire: Edit the `.env`  file and modify the lines with:

>+ NODE_TYPE if your not sure what option its need, select _general_
>+ NODE_NAME attribute according to the name you want for this node. The name SHOULD follow the convention:

> `TYPE_COMPANY_T_Y_Z_NN`

Where _TYPE_ is the rol for the node in the network (use `REG` for regular/general nodes), _XX_ is your company/entity name, Y is the number of processors of the machine, Z is the amount of memory in Gb and NN is a sequential counter for each machine that you may have (starting at 00). For example:

>+ `NODE_NAME="REG_IN2_T_2_8_00"`
>+ `NODE_NAME="REG_DigitelTS_T_2_8_00"`

This is the name that will appear in the public listings of nodes of the network. It does not have any other usage.

* :fire: Edit the `docker-compose.yml` file, and make your own changes.

* In the root directory of the repository (where the file `docker-compose.yml` exists) run:

```console
$ docker-compose up -d
```

* The command runs the service in the background, and you can check its activity by running:

```console
$ docker-compose logs -f --tail=20
```
  * **You're done!** :sunglasses: :dancer: :v: :beers: 
## Performing permissioned

You should see the node initializing and starting to try to contact peers. However, the node is not yet permissioned, so it can not participate in the blockchain network yet.

All nodes that will be installed in the Alastria Networks must be permissioned. To ask for permission you must enter your data in this [electonic form](https://portal.r2docuo.com/alastria/forms/noderequest) and make a PR for the files that are modified in the installation process. If an associated will want to remove a node from the network, it is kindly appreciated that a a request must be notified through a PR. Other guides related with operation of Alastria Node are aviable in following documents:

* [Alastria-T Network Operation and Government Policies (en_GB)](https://alastria.io/wp-content/uploads/2020/04/POLI-TICAS-GOBIERNO-Y-OPERACIO-N-RED-ALASTRIA-V1.01-DEF-en-GB.pdf)
* [Alastria-T Network Operation and Government Policies (es_ES)](https://alastria.io/wp-content/uploads/2020/04/POLI-TICAS-GOBIERNO-Y-OPERACIO-N-RED-ALASTRIA-V1.01-DEF.pdf)

* [Conditions of operation of the Alastria-T Network Regular Nodes (en_GB)](https://alastria.io/wp-content/uploads/2020/06/CONDICIONES-USO-RED-NODOS-REGULARES-A-LA-RED-ALASTRIA-v1.1-DEF-en-GB.pdf)
* [Conditions of operation of the Alastria-T Network Regular Nodes (es_ES)](https://alastria.io/wp-content/uploads/2020/06/CONDICIONES-USO-RED-NODOS-REGULARES-A-LA-RED-ALASTRIA-v1.1-DEF.pdf)

* [Conditions of operation of the Alastria-T Network Critical (boot && validator) Nodes (en_GB)](https://alastria.io/wp-content/uploads/2020/06/CONDICIONES-OPERACIO-N-RED-T-POR-PARTE-DE-NODOS-CRI-TICOS-V1.1-DEF-en-GB.pdf)
* [Conditions of operation of the Alastria-T Network Critical (boot && validator) Nodes (es_ES)](https://alastria.io/wp-content/uploads/2020/06/CONDICIONES-OPERACIO%CC%81N-RED-T-POR-PARTE-DE-NODOS-CRI%CC%81TICOS-V1.1-DEF.pdf)

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

* With that value, create a pull request to request permission, adding the line to the node list. You can access to this Alastria form, https://portal.r2docuo.com/alastria/forms/noderequest, to perform administrative permission:


The corresponding repository is alastria-node and the branch will be testnet2.

The files to be modified will be:

>+ `DIRECTORY_REGULAR.md`: you should include your node name, and the enode and IP direction.
>+ `data/regular-nodes.json`: you should include the enode and IP direction.

* When the pull request is accepted, you will see that your node starts connecting to its peers and starts synchronizing the blockchain. The process of synchronization can take hours or even one or two days depending on the speed of your network and machine.

Now it's time to start knowing more about `geth`and `goquorum`:
* https://geth.ethereum.org/docs/interface/command-line-options
* https://docs.goquorum.consensys.net/en/stable/
* https://github.com/ConsenSys/quorum 
## Maintaining the Node

You can use the standard docker-compose commands to manage your node. For example:

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

> personal.newAccount()
Passphrase:
Repeat passphrase:
"0x1234..."
> admin.peers
> admin.nodeInfo
> eth.blockNumber
> eth.syncing
> eth.mining
```

An easy way to test that your node is operating normally is to generate a fund transfer transaction from the node's account, itself from 0 weis.

```console
> personal.unlockAccount(eth.accounts[0],"_your_eth0_password_",2000)
> Unlock account 0x1234...
Passphrase:
true
> eth.sendTransaction({from: eth.accounts[0], to: eth.accounts[0], value:0 })
"0x1234..."
```

If the transaction appears in [Alastria-T Network explorer](https://blkexplorer1.telsius.alastria.io/blocks), the node its working correctly.
## Backup

The following items should be backed up:

>+ `/root/alastria/data/geth/nodekey`: This file contains the cryptographic information for joying the network. This file can be restored to start over a new installation without restarting the permissioning  process.
>+ ` /root/alastria/data/keystore/`: This directory contains local accounts created from the node.

## Resetting DLT

```console:
$ cp /root/alastria/data/geth/nodekey <enode-backup>
$ geth removedb_DONOTACCIDENTALY --datadir /root/alastria/data
$ geth --datadir /root/alastria/data init /root/genesis.json
$ cp <enode-backup> /root/alastria/data/geth/nodekey
$ (restart-container)
```

## System requirements

| Hardware | minimum | desired |
|--- |--- |--- |
| CPU's | 2 |  4 |
| Memory | 4Gb |  8Gb |
| Hard Disk | 128 Gb |  256 Gb |

DLT database grows 1Gb/week: keep in mind for future updates. SSD disc its also mandatory.

## System ports (INPUT)

The following ports must be open, at least, to the nodes defined in the `/root/alastria/data/static-nodes.json` and `/root/alastria/data/permissioned-nodes.json` files. We recommend that these ports be universally open: the `whisper protocol` defined in `geth`/`GoQuorum` is robust enough to be published without the need for control through the firewall.

| Port | Type | Definition |
|--- |--- |--- |
| 21000| TCP/UDP | Geth process application port (inbound and outbound for ethereum traffic) |
| 53 | TCP/UDP | Access to external Internet based resolvers |

`tcp/21000` and `udp/21000`port are mandatory, as is the common standard for the Alastria-T Network.
## System ports (OUTPUT)

We strongly advise not to filter outgoing ports. If necessary, these are the destinations

| Port | Type | Definition |
|--- |--- |--- |
| 80 | TCP | Outbound for Websockets feed to netstats server |
| 8086 | TCP | Outbound for InfluxDB statistics collection |

## Mandatory parametres

Some parametres are high hardcored in this installer, but can be change:

* Working directory: The install procedure expect use of `/root/alastria/data` as the main directory.
* Geth / Go versions: Changing the `alastria-node/Dockerfile` its easy to change the build version.
> NOTE: Using 20.10 its still experimental, as described in [UPGRADE_TO_LAST_VERSION](https://github.com/alastria/alastria-node/blob/testnet2/UPGRADE_TO_LAST_VERSION.md) but should be available soon.

* Data directory: Because of the size that the DLT database can reach, a Docker volume has been deployed to set the storage on some independent path from the one set by the Docker installation. This parameter is set in `docker-compose.yml`, in _volumes_ tag.
* Geth parametres: Other geth options can be personalized in `geth.node.bootnode.sh`, `geth.node.general.sh` or `geth.node.validator.sh`.

### Enviroment Variables

These variables should be use for any script in:

* `NODE_TYPE=[general|boot|validator]`: Rol for your node in the network.
* `NODE_NAME=REG_ExampleOrg_T_2_8_00`: Name for your node.
* `NODE_BRANCH=main`: Used for future improvements.

## Istanbul Gobernance IBTF

As the T-network is using the Istanbul BFT consensus protocol, the way to generate new blocks in the test-net is to have validator nodes available in the network and integrate them into the set of nodes that are part of the validation round.

Each round is initiated by a different node that "proposes" a set of transactions in a block and distributes them to the rest of the nodes.

The validator nodes must focus on operating the consensus protocol, integrating the transactions in the blockchain and distributing them to the rest of the nodes. 

## Parameters for Regular/General Nodes

```console
NODE_ARGS=" --rpc --rpcaddr 127.0.0.0 --rpcapi admin,db,eth,debug,miner,net,shh,txpool,personal,web3,quorum,istanbul --rpcport 22000"
```

Also websockets connection its allowed:
```console
NODE_ARGS=" --ws --wsaddr 127.0.0.0 --wsport 22001 --wsorigins source.com"
```

### Application Ports ###
To use your node through web3 applications, some connection method must be enabled. In this case, the following connection methods are offered:

* JSON / RPC connection: you should upgrade the following files, in order to allow web3 connections; `docker-compose.yml` allow new connection from tcp/22000, or the one defined in `alastria-node-data/env/geth.common.sh` related to _RCP_ connections.
> NOTE: exposing this port should be controled by any kind of firewall, or using any proxy filtering, as proposed in [alastria-access-point](https://github.com/alastria/alastria-access-point) proyect.
* WebSockets connection: please, follow this article [Connecting to an Alastria-T Network node using WebSockets](https://tech.tribalyte.eu/blog-websockets-red-t-alastria) created by Ronny Demera, from Tribalyte.

## Parameters for Boot Nodes

Boot nodes are responsible for permitting the nodes in the network. They are visible to all types of nodes. The boot node should not be used in any case to operate directly with it to interact with the network, so `rcp/ws` ports are not allowed.

### geth parametres for boot nodes

```console
NODE_ARGS="--maxpeers 200"
```

## Parameters for Validator Nodes

The validator nodes should not be used in any case to operate directly with it to interact with the network, so `rcp/ws` ports are not allowed.

* `istanbul.getValidators` () retrieves the list of validators that make up the validation round.

* `istanbul.propose ("0x ...", true)` votes for the validator represented by the coinbase to be integrated into the validation round. It must be accepted by at least half of the nodes.

* `istanbul.propose ("0x ...", false)` votes for the validator represented by the coinbase to be excluded from the validation round. It must be rejected by at least half of the nodes. 

```console
$ geth attach alastria/data/geth.ipc
> istanbul.getValidators() 
[...]
> istanbul.propose("_coinbase_of_node_validator_", true) #adding validator node
> istanbul.propose("_coinbase_of_node_validator_", false) #put out validator node
```

### geth parametres for validator nodes

```console
NODE_ARGS=" --maxpeers 100 --mine --minerthreads $(grep -c "processor" /proc/cpuinfo)"
```

## Other Resources

+ [Wiki](https://github.com/alastria/alastria-node/wiki)
+ [FAQ ES](https://github.com/alastria/alastria-node/wiki/FAQ_ES)
+ [FAQ EN](https://github.com/alastria/alastria-node/wiki/FAQ_EN)

* **Resources**

| URL | Managed by | Notes|
|--- |--- |--- |
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
Do not hesitate to contact Alastria Support Team to solve any doubt in support@alastria.io.

* [Slack](https://github.com/alastria/alastria-node/wiki/HELP)
* [Github (use & governance)](https://github.com/alastria/alastria-node/wiki/HELP)

# Changes from `testnet2` branch:

* Use of constellation it's no longer supported. Previous documentation can be accesed in [Constellation Using Pivate Transactions in Alastria T](https://github.com/alastria/alastria-node/wiki/Constellation---Using-Pivate-Transactions-in-Alastria-T)
* Use of nginx as proxy it's no longer supported. However, the repository alastria-node-access it's still avaiable
* Use of `monitor` its deprecated
* Use of local fork of `quorum`, https://github.com/alastria/quorum it's no longer used
