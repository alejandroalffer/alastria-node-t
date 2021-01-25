# Alastria Node for  Alastria-T Network

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/alastria/alastria-node/blob/testnet2/LICENSE)
[![Slack Status](https://img.shields.io/badge/slack-join_chat-white.svg?logo=slack&style=social)](https://alastria.slack.com/)

Based in Jesús Ruiz develop, jesus@alastria.com

## Configuration & Installation: Quick Guide

* Clone or download this repository to the machine where you want to install and operate the Red T node and enter into the cloned directory.

* Edit the file `.env` and modify the lines with:

    + NODE_TYPE if your not sure what option its need, select _general_

    + NODE_NAME attribute according to the name you want for this node. The name SHOULD follow the convention:

    `TYPE_COMPANY_T_Y_Z_NN`

    Where _TYPE_ is the rol for the node in the network, _XX_ is your company/entity name, Y is the number of processors of the machine, Z is the amount of memory in Gb and NN is a sequential counter for each machine that you may have (starting at 00). For example:

    `NODE_NAME="VAL_IN2_T_2_8_00"`
    `NODE_NAME="BOT_DigitelTS_T_2_8_00"`

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
docker exec -it REG_ExampleOrg_T_2_8_00 geth --exec "admin.nodeInfo.enode" attach /root/alastria/data/geth.ipc
```

* Get the IP address of your node, as seen from the external world. 

```console
curl https://ifconfig.me/
```

* Create the full enode address like:

    `enode://YOUR_ENODE@YOUR_IP:21000?discport=0`

    where

    + **YOUR_ENODE** is the value of the ENODE_ADDRESS file
    + **YOUR_IP** is the external IP of your node

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
docker rm REG_ExampleOrg_T_2_8_00
```