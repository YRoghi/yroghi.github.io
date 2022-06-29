---
layout: post
title: Linux is easy, right?
subtitle: Spoiler - Only when the docs are up to date
---

I'm currently doing my CCNA certification as part of my job (would have been cool if we had actually touched any form of networking equipment in Uni but alas...) and as a result I've been exploring alternatives to CISCOs Network Emulation tool [Packet Tracer](https://www.netacad.com/courses/packet-tracer). One such service is an open source platform called GNS3. I decided to try spin up a GNS3 server that allows me to use the WebUI from any machine on my network as a first project for my lab environment. Unfortunately, the way to get there was anything but straightforward...

# GNS3 Remote Server

## Ubuntu 22.04 LTS Install

Installing Ubuntu Server 22.04LTS as a GEN1 HyperV VM using the AMD64 image. Selected OpenSSH Server to be included on install.
Credentials are:
`Username: gns3admin` `Password: gns3admin`

Install the required tools (net-tools, GNU C Compiler (gcc), Make, python setup tools)

```bash
sudo apt-get install net-tools gcc cmake python3-setuptools git
```

## GNS3 Server Install

Installing the GNS3 server via the "Remote Server" instructions on the docs does not work as it installs a depriciated version of GNS3 Server that is not compatible with the new client (welcome to open Open Source software maintenance). Additionally, the remote server VHDs that can be downloaded on install of the GNS3 Client also install a depriciated version of the server that is not compatible with the new version of the client. Below is the how I ended up being able to install a current version GNS3 Server on my Ubuntu Server VM.

### Dependencies

To install uBridge git clone the [uBridge](https://github.com/GNS3/ubridge/) git repo. From the uBridge directory run the following commands:

```bash
# Installing Dependencies
sudo apt-get install libpcap-dev

make -f Makefile
sudo make install
```

To install dynamips git clone the [dynamips](https://github.com/GNS3/dynamips/) git repo. From the dynamips directory run the following commands:

```bash
# Installing Dependencies
sudo apt-get install libelf-dev

mkdir build
cd build
cmake ..
make -f Makefile
sudo make install
```

To install Virtual PC Simulator (VPCS) git clone the [VPCS](https://github.com/GNS3/vpcs/) git repo. From the VPCS directory run the following commands:

```bash
# Installing Dependencies
sudo apt-get install qemu-kvm libvirt-dev mtools

cd src
./mk.sh
sudo ln -s /home/gns3admin/vpcs/src/vpcs /usr/bin/vpcs
```

### GNS3Server

To install the GNS3 server git clone the [GNS3](https://github.com/GNS3/gns3-server/)  git repo. From the gns3server directory run the following commands:

```bash
sudo python3 setup.py install
# Once the server is installed, create a config file
cd ~/.config/GNS3/2.2/
cp ~/gns3server/conf/gns3_server.conf gns3_server.conf
nano gns3_server.conf

# Change the host IP to the current IP of the host then hit CTRL + X, Y, then Enter

# Use the following command to test the server
gns3server

# Open a web browser on the same network, and connect to the IP on port 3080.
# If this does not work/there is red on the server CLI, check your config/install
```


As an additional tool, there is a Youtube guide by [Thomas Tucker](https://www.youtube.com/watch?v=6KxyfI4yz40&ab_channel=ThomasTucker) that explains an install on a Debian system (this method installs a depriciated version of GNS3Server), but helps with adding firmware to the server once installed if you are lost. It's also worth noting that from this install, the WebGui is enabled by default and accesible via the server IP on port 3080. WebGui is not available prior to version 2.2 of GNS3.

**TODO** *Figure out where to find free appliances I can use to actually start creating networks with CISCO or other networking devices. Do I need licenses?*