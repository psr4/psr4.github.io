### 开启nginx+php
```
docker run --name php -d -v /www/wwwroot:/var/www/html php:7.1-fpm

docker run --name nginx -p 80:80 -d -v /www/wwwroot:/usr/share/nginx/html --privileged -v /www/nginx/conf.d:/etc/nginx/conf.d --privileged --link php:php nginx
```

### 创建5节点的PXC集群
> 注意，每个MySQL容器创建之后，因为要执行PXC的初始化和加入集群等工作，耐心等待1分钟左右再用客户
> 端连接MySQL。另外，必须第1个MySQL节点启动成功，用MySQL客户端能连接上之后，再去创建其他
> MySQL节点。

```
#创建第1个MySQL节点
docker run ‐d ‐p 3306:3306 ‐e MYSQL_ROOT_PASSWORD=abc123456 ‐e CLUSTER_NAME=PXC ‐e
XTRABACKUP_PASSWORD=abc123456 ‐v v1:/var/lib/mysql ‐v backup:/data ‐‐privileged ‐‐
name=node1 ‐‐net=net1 ‐‐ip 172.18.0.2 pxc
#创建第2个MySQL节点
docker run ‐d ‐p 3307:3306 ‐e MYSQL_ROOT_PASSWORD=abc123456 ‐e CLUSTER_NAME=PXC ‐e
XTRABACKUP_PASSWORD=abc123456 ‐e CLUSTER_JOIN=node1 ‐v v2:/var/lib/mysql ‐v
backup:/data ‐‐privileged ‐‐name=node2 ‐‐net=net1 ‐‐ip 172.18.0.3 pxc
#创建第3个MySQL节点
docker run ‐d ‐p 3308:3306 ‐e MYSQL_ROOT_PASSWORD=abc123456 ‐e CLUSTER_NAME=PXC ‐e
XTRABACKUP_PASSWORD=abc123456 ‐e CLUSTER_JOIN=node1 ‐v v3:/var/lib/mysql ‐‐
privileged ‐‐name=node3 ‐‐net=net1 ‐‐ip 172.18.0.4 pxc
#创建第4个MySQL节点
docker run ‐d ‐p 3309:3306 ‐e MYSQL_ROOT_PASSWORD=abc123456 ‐e CLUSTER_NAME=PXC ‐e
XTRABACKUP_PASSWORD=abc123456 ‐e CLUSTER_JOIN=node1 ‐v v4:/var/lib/mysql ‐‐
privileged ‐‐name=node4 ‐‐net=net1 ‐‐ip 172.18.0.5 pxc
#创建第5个MySQL节点
docker run ‐d ‐p 3310:3306 ‐e MYSQL_ROOT_PASSWORD=abc123456 ‐e CLUSTER_NAME=PXC ‐e
XTRABACKUP_PASSWORD=abc123456 ‐e CLUSTER_JOIN=node1 ‐v v5:/var/lib/mysql ‐v
backup:/data ‐‐privileged ‐‐name=node5 ‐‐net=net1 ‐‐ip 172.18.0.6 pxc
```

### redis 集群
```
docker pull yyyyttttwwww/redis
docker network create ‐‐subnet=172.19.0.0/16 net2

#创建2节点Redis容器
docker run ‐it ‐d ‐‐name r1 ‐p 5001:6379 ‐‐net=net2 ‐‐ip 172.19.0.2 redis bash
docker run ‐it ‐d ‐‐name r2 ‐p 5002:6379 ‐‐net=net2 ‐‐ip 172.19.0.3 redis bash

#启动2节点Redis服务器
#进入r1节点
docker exec ‐it r1 bash
cp /home/redis/redis.conf /usr/redis/redis.conf
cd /usr/redis/src
./redis‐server ../redis.conf
#进入r2节点
docker exec ‐it r2 bash
cp /home/redis/redis.conf /usr/redis/redis.conf
cd /usr/redis/src
./redis‐server ../redis.conf
#在r1节点上执行下面的指令
cd /usr/redis/src
mkdir ‐p ../cluster
cp redis‐trib.rb ../cluster/
cd ../cluster
#创建Cluster集群
./redis‐trib.rb create ‐‐replicas 1 172.19.0.2:6379 172.19.0.3:6379
172.19.0.4:6379 172.19.0.5:6379 172.19.0.6:6379 172.19.0.7:6379
```
