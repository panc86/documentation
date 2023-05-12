---
layout: default
title: Setup
grand_parent: SMDRM
parent: Get Started
nav_order: 1
---

# Prerequisites

You should have at least 3 CentOS based Virtual Machines (VM) with sudo priviledges.

|Node|Role|CPU|RAM|DISK|Notes|
|----|----|---|---|----|-----|
|  #1|master|8|16gb|200gb|NFS server|
|  #2|worker|8|32gb|100gb| |
|  #3|worker|8|32gb|100gb| |

_Table 1. Cluster minimum hardware specifications._

# Install Docker

Install [Docker Engine](https://docs.docker.com/engine/install/centos/) on each VM.

We also recommend to install [Docker Compose v2.3.3](https://docs.docker.com/compose/install/linux/)
on the master node for development purpose.

{: .important}
Latest version of SMDRM is powered by Docker Engine 20.10.16 and Docker Compose v2.3.3

# Open Firewall Ports

{: .warning}
Ensure the following ports are not used by any other process.

Open firewall ports on the master node.

```shell
firewall-cmd --permanent --add-port=2376/tcp
firewall-cmd --permanent --add-port=2377/tcp
firewall-cmd --permanent --add-port=7946/tcp
firewall-cmd --permanent --add-port=7946/udp
firewall-cmd --permanent --add-port=4789/udp
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --reload
```

Open firewall ports on the worker nodes.

```shell
firewall-cmd --permanent --add-port=2376/tcp
firewall-cmd --permanent --add-port=7946/tcp
firewall-cmd --permanent --add-port=7946/udp
firewall-cmd --permanent --add-port=4789/udp
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --reload
systemctl restart docker
```

# Swarm Mode

Follow the [Swarm tutorial](https://docs.docker.com/engine/swarm/swarm-tutorial/)
to enable and configure Swarm mode.

{: .important}
One of the key features of Docker Swarm is the ability to share storage volumes
across multiple containers and hosts. This can be useful in several scenarios,
such as when you need to share data between containers or when you want to use
a persistent storage volume available to all Swarm members.
Network File System (NFS) is a popular driver to mount the same
[Storage Area Network](https://en.wikipedia.org/wiki/Storage_area_network)
storage volume onto each Docker Swarm node.

# Network File System

For a detailed step-by-step guide, read this [blog post](https://www.tecmint.com/install-nfs-server-on-centos-8/).

1. Setup a NFS Client/Server on one of the VMs 

```shell
# install the package
dnf install nfs-utils
# start the service
systemctl start nfs-server.service
systemctl enable nfs-server.service
# verify the status
systemctl status nfs-server.service
```

{: .important}
Ensure that the partition hosting the file systems has enough free space
to contain the amount of data you plan to process.


{:style="counter-reset:none"}
1. Add the file systems in the NFS server /etc/exports configuration file.

```text
</nfs/share/absolute/path>      *.example.com(rw,sync)
```

{: .note}
Read `man exports` for more information and export options.

{:style="counter-reset:none"}
1. Export the file systems.

```shell
exportfs -arv
```

{:style="counter-reset:none"}
1. Allow traffic to the necessary NFS services via the firewall.

```shell
firewall-cmd --permanent --add-service=nfs
firewall-cmd --permanent --add-service=rpc-bind
firewall-cmd --permanent --add-service=mountd
firewall-cmd --reload
```

# What's Next?

After completing the setup of the required components, you can [deploy the Docker stack in Swarm mode]({{ site.baseurl }}/smdrm/deploy).
