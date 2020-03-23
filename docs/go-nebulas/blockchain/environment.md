# 星云节点环境

## Introduction

We are glad to release Nebulas Mainnet here. Please join and enjoy Nebulas Mainnet.

> [https://github.com/nebulasio/go-nebulas/tree/master](https://github.com/nebulasio/go-nebulas/tree/master)

## Hardware configuration

The nodes of the nebulas have certain requirements for machine performance. We recommend the performance of the machine has the following requirements:

```
CPU: >= 4cores(recommand 8 cores)
RAM: >= 16G
Disk: >= 600G SSD
```

## Environment

**Node Installation Tutorial** - review the [Nebulas Technical Documentation: Nebulas 101 - 01 Compile Installation](../tutorials/01-installation).


It’s recommended to build and deploy nodes via docker:



*   Install [docker](https://docs.docker.com/get-started/) and [docker-compose](https://docs.docker.com/compose/install/)
*   Execute the following docker command via [root](https://github.com/nebulasio/go-nebulas)

```
sudo docker-compose build

sudo docker-compose up -d

```


```
System: Ubuntu 18.04(recommand), other Linux is ok.
NTP: Ensure machine time synchronization
```

#### NTP
Install the NTP service to keep system time in sync.

Ubuntu install steps:

```
#install
sudo apt install ntp
#start ntp service
sudo service ntp restart
# check ntp status
ntpq -p
```

Centos install steps:

```
#install
sudo yum install ntp
#start ntp service
sudo service ntp restart
# check ntp status
ntpq -p
```


## Contribution

Feel free to join Nebulas Mainnet. If you did find something wrong, please [submit a issue](https://github.com/nebulasio/go-nebulas/issues/new) or [submit a pull request](https://github.com/nebulasio/go-nebulas/pulls) to let us know, we will add your name and url to this page soon.
