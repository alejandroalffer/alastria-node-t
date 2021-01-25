# Alastria Node for  Alastria-T Network

```
WIP
```

Based in Jesús Ruiz develop, jesus@alastria.com

```console
$ docker-compose up -d
```

```console
docker exec -it REG_Example_T_2_8_00 geth --exec "admin.nodeInfo.enode" attach /root/alastria/data/geth.ipc
```

* The command runs the service in the background, and you can check its activity by running:
  
```console
$ docker-compose logs -f --tail=20
```

```console
$ docker-compose down
```

To restart the node:

```console
$ docker-compose restart
```