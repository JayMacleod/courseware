<!--in-progress, may split out/remove some/all vpn and onion stuff-->

# IP address

<!--TOC_START-->
### Contents
- [Overview](#overview)
- [Usage](#usage)
	- [1. IP addresses](#1-ip-addresses)
		- [IPv4](#ipv4)
		- [Reserved IPv4 addresses](#reserved-ipv4-addresses)
		- [IPv4 Structure](#ipv4-structure)
		- [IPv6](#ipv6)
	- [2. ARP](#2-arp)
	- [3. `localhost`](#3-localhost)
		- [Ports](#ports)
	- [4. Routing](#4-routing)
		- [Network Classes](#network-classes)
		- [Subnets](#subnets)
		- [CIDR](#cidr)
	- [5. Gateways](#5-gateways)
		- [NAT](#nat)
		- [DNS](#dns)
- [Changing IP address](#changing-ip-address)
	- [VPNs](#vpns)
- [Tasks](#tasks)
	- [View the ARP table](#view-the-arp-table)
		- [macOS, Linux, or Windows Subsystem for Linux](#macos-linux-or-windows-subsystem-for-linux)
		- [Windows](#windows)
	- [Find your node's networking information](#find-your-nodes-networking-information)
		- [macOS, Linux, or Windows Subsystem for Linux](#macos-linux-or-windows-subsystem-for-linux-1)
		- [Windows](#windows-1)
	- [Find network information using a subnet calculator](#find-network-information-using-a-subnet-calculator)
	- [Change your IP address](#change-your-ip-address)
		- [Find your machine's GPS location](#find-your-machines-gps-location)
		- [Installing Opera VPN](#installing-opera-vpn)

<!--TOC_END-->
## Overview
Nearly all networks are based on the **Internet Protocol Suite (TCP/IP)**, the set of protocols that allow for a device to gain access to the Internet.

Contained within this suite is the **Transmission Control Protocol (TCP)**, which facilitates connections between nodes within a network, and the **Internet Protocol (IP)**, which has the task of delivering data from a source node to a destination node.

TCP is a connection protocol: it is in charge of ensuring that a connection has been made before data is transferred.

IP is connectionless: it does not care whether the information it attempts to deliver is received or not.

IP also *encapsulates* the data which it transmits, splitting it into several chunks, or **datagrams**, each containing a **header** and a **payload**:

|Type|Information| 
|-|-|
|**Datagram**|basic transfer unit - contains header; payload|
|**Header**|contains source IP address; destination IP address; routing metadata|
|**Payload**|content of data to be transported|

IP can then route these datagrams from a 'source' node to a 'destination' node.

Essentially, if TCP is the postal service, IP is the postman.

TCP makes sure that the connections are in place for deliveries to take place, but all IP does is delivers stuff, and doesn't care if that stuff ever gets where it's supposed to be or not.

## Usage
The Internet Protocol requires five specific things in order to work:
1. An **IP address**, assigned to whichever device is attempting to transfer information over the Internet
2. The **Address Resolution Protocool (ARP)**, a mechanism for mapping a physical MAC address (literally burned into the hardware) to a networked, non-physical IP address
3. A local network (the **`localhost`**), with **ports** to plug programs (or **services**) into
4. A **routing** mechanism for moving data around
5. A **network gateway**, which routes data from the local **internal** network to **external** networks

This section will discuss each of these characteristics.

### 1. IP addresses
In the same way that a postal system uses addresses for posting letters, IP makes use of addresses, which are assigned to every Internet-connected device, in order to send information from a source node to a destination node.

Each networked system must be identified as unique, so that when information is sent via IP, we can be sure of where we are sending that information - even if it is not received.

Addressing solves this issue by using a unique **IP address** which distinguishes one node from any other.

The **Internet Engineering Task Force (IETF)** regulates the usage of IP addresses.

#### IPv4
The current standard of IP addresses, **IPv4**, are made up of a 32-bit address, and can be expressed in four octets of binary:
```text
00001010.00001100.00111000.00010111
```

IPv4 addresses are designed to be human-readable, thus, they are usually written in dotted decimal:
```text
10.12.56.23
```

Allocating unique IP addresses is usually done automatically with the **Dyanmic Host Configuration Protocol (DHCP)**.

#### Reserved IPv4 addresses
As each section of an IPv4 address can only be numbered from 0 to 255, they are limited to 4.3 billion unique combinations - this may seem like a lot, but several restrictions apply: 

The following IPv4 ranges are designated for private purposes, namely local communications within a private network, and therefore cannot be used on the Internet:

|Range|
|-|
|`10.0.0.0 - 10.255.255.255`|
|`172.16.0.0 - 172.31.255.255`|
|`192.168.0.0 - 192.168.255.255`|

There are also other restrictions on IPv4 addresses - some important reserved address blocks are:

|Range|Usage|
|-|-|
|`0.x.x.x`|software use only|
|`127.x.x.x`|`localhost`|
|`224.0.0.0 - 239.255.255.255`|IP multicasting, e.g. for streaming media|
|`240.0.0.0 - 255.255.255.254`|reserved for future usage|
|`255.255.255.255`|local broadcast|

#### IPv4 Structure
An IPv4 address is made up of two groups of bits.

The first, more significant group of bits is the **network prefix**, which identifies an entire network (or sub-network) - it is also *static* and immutable.

The least significant set forms the **host identifier**, which specifies a particular interface of a host on that network - this is *variable* and therefore can be changed when necessary.

This division is used for two reasons - firstly, it is the basis of traffic routing between IP networks (which is discussed later in this module); secondly, it's used for address allocation purposes.

#### IPv6
Eventually, we will run out of IP addresses.

Given that the Internet has been in continuous usage for decades, the problem of IP address exhaustion has been prevalent since the late-1980s.

IPv6 was developed, and is currently in deployment, in preparation for the time when IPv4 addresses deplete entirely.

As a result of IPv4's depletion, IPv6 upgrades the amount of data stored in an IP address from IPv4's 32-bit system to 128-bit - allowing for 340.3 undecillion unique combinations.

This can be expressed in eight colonned groups of four hexadecimal digits, and allows for concatenation of zero values by double-colonning - thus, the same address can be written in multiple forms:
```text
2001:0db8:0000:0000:8a2e:0370:7334:0001

2001:db8:0:0:82ae:370:7734:1

2001:db8::8a2e:370:7334::1
```

IPv4 addresses can also be expressed in IPv6 format - the IPv4 address `10.12.56.23` becomes:
```text
0:0:0:0:0:ffff:a0c:3817

::ffff:a0c:3817
```

As 17% of IPv4 addresses are currently in-use (as of 2011), it is widely considered that IPv4 will be used alongside IPv6 for the forseeable future.

### 2. ARP
Every networked device contains at least one physical **Network Interface Card (NIC)**, such for Ethernet or wireless connections, providing a physical connection between that device and a network.

NICs can generate a **broadcast domain** between devices if they are all connected to the same system - the numbers in the below diagram signifies a different broadcast domain:

```text
                 1                  2
          ┌────────────[Router]────────────┐
       [Switch]           | 3           [Switch]
    ┌─────┴─────┐       [Node]       ┌─────┴─────┐
  [Node]      [Node]               [Node]      [Node]
```

Each NIC has a unique [MAC address](https://github.com/bob-crutchley/courseware/tree/master/topics/networking/modules/mac-address/) burned into it by its manufacturer, which are used by Ethernet and wireless connections to communicate between network devices.

As these NICs only understand MAC addresses, the **Address Resolution Protocol (ARP)** is used to resolve MAC to IP, or vice-versa.

ARP uses broadcast packets to all computers on the Ethernet segment to discover MAC addresses.

Here, **Node 1** and **Node 2** are both present on the same network:

```text
 ┌────────[Node 1]─────────┐						        			┌────────[Node 2]─────────┐
 │ MAC: 12:6e:eb:de:b3:ed  │────────────────────────────────────────────│ MAC: 12:6f:56:c0:c4:c1  │
 │ IP: 10.12.2.73          │											│ IP: 10.12.2.1           │
 └─────────────────────────┘							        		└─────────────────────────┘
```

If **Node 1** tries to find the MAC address associated with the IPv4 address `10.12.2.1`, it will broadcast an **ARP request** along the network until it finds the device it needs:

```text
 ┌────────[Node 1]─────────┐											┌────────[Node 2]─────────┐
 │ MAC: 12:6e:eb:de:b3:ed  │────────────>───────────────────>───────────│ MAC: 12:6f:56:c0:c4:c1  │
 │ IP: 10.12.2.73          │	   Source MAC: 12:6e:eb:de:b3:ed		│ IP: 10.12.2.1           │
 └─────────────────────────┘	   Source IP:  10.12.2.73  	       		└─────────────────────────┘
								   Target MAC: 00:00:00:00:00:00
								   Target IP:  10.12.2.1
```

Once **Node 2** receives this request, it will send an **ARP response** if the Target IP matches its IP address:

```text
 ┌────────[Node 1]─────────┐									  		┌────────[Node 2]─────────┐
 │ MAC: 12:6e:eb:de:b3:ed  │────────────<───────────────────<───────────│ MAC: 12:6f:56:c0:c4:c1  │
 │ IP: 10.12.2.73          │	   Source MAC: 12:6f:56:c0:c4:c1		│ IP: 10.12.2.1           │
 └─────────────────────────┘	   Source IP:  10.12.2.1	   	   		└─────────────────────────┘
								   Target MAC: 12:6e:eb:de:b3:ed
								   Target IP:  10.12.2.73
```

### 3. `localhost`
Let's say that we want to access network services that are running on a host device. 

To do this, we have to use the **local loopback mechanism**, a network interface which bypasses the networking hardware of that device entirely.

The cool thing about the local loopback mechanism is that it can run a service that's meant to be on a network - either the local network or the Internet - without having any networking hardware installed on the device at all.

There is a special category of IPv4 addresses reserved so that we can do this: `127.x.x.x`.

They can only be used by internal systems within a host device - in other words, when using these addresses, the host device acts as both the *source* of data and the *receiver* of data. 

`localhost` is one of these **loopback address** - so any connection it tries to make bypasses its networking devices entirely, and *loops* the connection back to its own internal systems.

`localhost` is hosted at the loopback address `127.0.0.1` (or `::1` in IPv6).

So, when we use the `localhost`, what we're basically asking the device to do is to point any data it sends to *itself*.

As such, the `localhost` is a testing area. 

It looks and feel as though you're hosting something (like a service or a website) on the Internet properly - but all that's really happening is that your device is looping back to itself whenever it tries to connect to the thing you're testing.

This is great for when we're developing stuff, because it allows us to test our services without ever exposing them to a) the rest of the network we're on, and b) the Internet.

#### Ports
Ports allow us to speak to a specific *service* - a small, self-contained program, which can be used either on its own or in combination with other services.

We 'plug in' the service to the port we want it to use (though for larger programs, this is usually done automaticaly).

The `localhost`, as part of testing, makes exclusive use of these ports.

Because `localhost` uses TCP/IP, any port that it tries to speak to is a **TCP port**.

For instance, if we want run a program which can be hosted on the Internet directly from a computer, such as a Java application, we might find that this program is hosted here:
```text
localhost:8080
```

In this instance, the `:8080` refers to the specific **port** which a program is using to run.

Since the `localhost` is just a name for a specific IP address, our Java application really runs here:
```text
127.0.0.1:8080
```

If we were to host this application outside of our `localhost`, the IP address would change, but the port would remain the same:
```text
34.231.45.89:8080
```

### 4. Routing
Routing refers to the way in which information is redirected through networks.

As we are now familiar with IP addresses, we can use IPv4 to demonstrate how this works.

#### Network Classes
The first IPv4 addresses were limited to 254 unique allocations. As these depleted, IPv4 was further subdivided into a **network class** structure by 1981.

Classful network design allowed for a larger number of individual networks.

The first three bits of an IP address determined its class.

The size of the network ID was based on the class of the IP address.

Each class used more octets for the network ID, making the host ID smaller and reducing the number of possible host addresses:

|Class|Network ID|Host ID|Networks|Addresses|
|-|-|-|-|-|-|-|
|**A**|`a`|`b.c.d`|128|16,777,216|
|**B**|`a.b`|`c.d`|16,384|65,536|
|**C**|`a.b.c`|`d`|2,097,152|256|
|**D**|`a.b.c.d`|`(experimental)`|2,110,199|512|   

#### Subnets
Network classes became mostly obsolete as IPv4 depletion rapidly increased with the dotcom bubble of the early-90s, and by 1993 they had phased out entirely.

The primary reason for this was that using a traditional class format to subnet usually resulted in networks having either a too-broad or a too-narrow broadcast domain.

For instance, a network that might have needed hundreds of IP addresses for its nodes (like a WAN across a city's municipal offices) could be assigned a class which was too limited, making them run out of addresses very quickly.

Meanwhile, a network that might not have needed more than a few devices connected at any given time (like a LAN in a small office) could be given swathes of IP addresses it didn't need.

Subnetting needed to change to be more manageable, and that signalled a move away from the class system.

To facilitate this, the split in IPv4 addresses between the network prefix and the host identifier are utilised, but an extra section of bits is added for clarity: the **subnet identifier**.

Let's consider the following address:
```text
192.168.100.14/24
```

In this case, how can we know which section of bits is the network prefix, and which section refers to the host?

#### CIDR
Classful network architecture was replaced by **Classless Inter-Domain Routing (CIDR)** in 1993.

CIDR allows for the dynamic assignment of several IP addresses to a network through a variety of subnets

Instead of the four classes we looked at earlier, there are now 32 different categories to choose from in order to fine-tune the scope of a particular network's broadcast domain.

The section of bits after the address we looked at earlier - the `/24` - is the **subnet identifier**.

The subnet ID then corresponds with a specific 32-bit **subnet mask**.

The subnet ID lets us know how much of the IP address is the network ID - here, it's the first `/24` bits, or 3 octets.

If we look at the IP address as binary, we can count these 24 bits:
```text
11000000.10101000.01100100    .00001110     /24
└───────────┬───────────┘     └───┬───┘     └┬┘
        network ID             host ID   subnet ID
```

If we convert from binary to octets, we can count the 3 octets:
```text
192.168.100       .14         /24
└────┬────┘       └┬┘         └┬┘
network ID      host ID    subnet ID
```
As the subnet ID is `/24`, the network ID is the first 3 octets; `192.168.100`, and doesn't change - it is *static*.

The host ID - the specific 'door number' of the node we're looking at - is `14`, and can change depending on the device we're looking at - it is *variable*.

Therefore, `192.168.100.14` is one of the 254 nodes within the `192.168.100` subnet.

CIDR uses these things to identify the sub-network, within a domain, that a particular node is on:

```text
┌──────┬─────────────┐ ┌──────┬─────────────┐ ┌──────┬───────────────┐ ┌──────┬─────────────────┐
│  ID  │ Subnet Mask │ │  ID  │ Subnet Mask │ │  ID  │  Subnet Mask  │ │  ID  │   Subnet Mask   │
├──────┼─────────────┤ ├──────┼─────────────┤ ├──────┼───────────────┤ ├──────┼─────────────────┤
│ /1   │  128.0.0.0  │ │ /9   │ 255.128.0.0 │ │ /17  │ 255.255.128.0 │ │ /25  │ 255.255.255.128 │
│ /2   │  192.0.0.0  │ │ /10  │ 255.192.0.0 │ │ /18  │ 255.255.192.0 │ │ /26  │ 255.255.255.192 │
│ /3   │  224.0.0.0  │ │ /11  │ 255.224.0.0 │ │ /19  │ 255.255.224.0 │ │ /27  │ 255.255.255.224 │
│ /4   │  240.0.0.0  │ │ /12  │ 255.240.0.0 │ │ /20  │ 255.255.240.0 │ │ /28  │ 255.255.255.240 │
│ /5   │  248.0.0.0  │ │ /13  │ 255.248.0.0 │ │ /21  │ 255.255.248.0 │ │ /29  │ 255.255.255.248 │
│ /6   │  252.0.0.0  │ │ /14  │ 255.252.0.0 │ │ /22  │ 255.255.252.0 │ │ /30  │ 255.255.255.252 │
│ /7   │  254.0.0.0  │ │ /15  │ 255.254.0.0 │ │ /23  │ 255.255.254.0 │ │ /31  │ 255.255.255.254 │
│ /8   │  255.0.0.0  │ │ /16  │ 255.255.0.0 │ │ /24  │ 255.255.255.0 │ │ /32  │ 255.255.255.255 │
└──────┴─────────────┘ └──────┴─────────────┘ └──────┴───────────────┘ └──────┴─────────────────┘
```
We can see, here, that our subnet mask is `255.255.255.0`.

The subnet mask also contains *static* and *variable* octets, which correspond to the static and variable octets in our IPv4 address.

In this case, since `192.168.100` in our IPv4 is static, so is `255.255.255` in our subnet mask.

And, just like `14` is variable in our IPv4 address, so is `0` in the subnet mask.

Therefore, there must be 254 different addresses within our network domain which a connected device can be assigned to:

```text
IPv4 address:       192.168.100.14
Network ID:         192.168.100.0
Host ID:            14
Subnet ID:          /24
Subnet mask:        255.255.255.0
Available hosts:    254
```

Our network contains every available address from `192.168.100.1` to `192.168.100.254`.

### 5. Gateways
A **network gateway** is a way for a private network to access public networks.

When connecting to the Internet, the packets of information we wish to send - those with a destination outside a given subnet mask - are sent to the network gateway.

Let's say we want to send data outside of the following private network:
```text
192.168.1.1/24
```
We know that this network has an address of `192.168.1.1`, and a subnet mask of `255.255.255.0` thanks to the CIDR.

When any data is addressed to an IP address outside of `192.168.1.0`, it is sent to the network gateway.

#### NAT
If several nodes on a private network are connecting to a public network at the same time, the data that passes between those networks is directed through a **default gateway**.

**Network Address Translation (NAT)** allows for information to pass between a private network and a public one, such as the Internet, by pointing all nodes within that network through the default gateway.

Let's say we have three nodes, all in the same network, looking to access the Internet:

```text
                  Local Network                                    
			Private IPv4: 192.168.X.X                                    Internet
┌───────────────────────┴────────────────────────────┐│┌─────────────────────┴────────────────────┐
                                                      │
 ┌─────[Node]────┐                                    │
 │ 192.168.100.3 ├──┐                                 │
 └───────────────┘  │                                 │
 ┌─────[Node]────┐  │                ┌────────────[Router]───────────┐
 │ 192.168.100.4 ├──┼────────────────┤ Default gateway: 192.168.1.1  ├───────────────────────────>
 └───────────────┘  │                │ Public IPv4:     145.12.131.7 │
 ┌─────[Node]────┐  │                └───────────────────────────────┘
 │ 192.168.100.5 ├──┘                                 │
 └───────────────┘                                    │
                                                      │
```

The nodes are all contained within a local network with a network ID of `192.168`.

Each of them have a **private IP** within this network of `192.168.X.X`.

The router redirects data from all nodes in the network to its **default gateway** of `192.168.1.1`.

NAT translates the information to a **public IP** through the default gateway, allowing the data to access any external IP address.

#### DNS
Once a device is connected to the Internet, the user will, most likely, type in some URL to visit:
```text
https://example.com/
```

For the purposes of this tutorial, this is the website's **host name**.

The **Domain Name System (DNS)** is used to convert a device's host name into an IP address which another device can understand.

For example, if a device needs to communicate with `example.com`, it will need the IP address for `example.com`. (It doesn't matter whether it's IPv4 or IPv6 in this case.)

It is the job of DNS to convert the host name to the IP address of the web server.

DNS is, essentially, a massive directory of web addresses - it converts a website's name that people know to a number that the Internet actually uses.

The command `ping` allows us to see DNS in action:
```text
C:\WINDOWS\system32>ping example.com

Pinging example.com [93.184.216.34] with 32 bytes of data:
Reply from 93.184.216.34: bytes=32 time=76ms TTL=51
Reply from 93.184.216.34: bytes=32 time=79ms TTL=51
Reply from 93.184.216.34: bytes=32 time=76ms TTL=51
Reply from 93.184.216.34: bytes=32 time=80ms TTL=51

Ping statistics for 93.184.216.34:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 76ms, Maximum = 80ms, Average = 77ms
```

We can see the IP address (IPv4 in this case) for `example.com` is `93.184.216.34`.

## Changing IP address
**IP-based geolocation** is the mapping of an issued IP address to the location in which the server that a particular node is connected to. 

While this is sometimes useful - such as for querying the location of a central web server which a particular node is connected to, or for **Global Positioning System (GPS)** location on mobile devices - there are cases in which changing a node's perceived location can be beneficial.

### VPNs
A **Virtual Private Network (VPN)** is used to extend a private network across a public network, enabling nodes to send and receive data on a public network as though they were directly connected to a private network.

Originally, VPNs were developed as a way to allow remote users, such as those working for large organisations, access to corporate applications and resources - eliminating the need to use a machine that is directly connected to an organisational network.

In terms of security, the connection to a private network is established through an encrypted layered **tunneling protocol**; the VPN user then authenticates themselves so as to gain access to the network.

In recent times, most individuals with a VPN use them for a number of reasons:
 - **Privacy** - it is commonly believed that **Internet Service Providers (ISPs)** are able to view the history of every website that was visited by each of its users; using a VPN arguably obfuscates this information.
 - **Security** - as a VPN extends a private network across a public system through an encrypted tunnel, they act as an extra measure when accessing sensitive information; some VPNs may themselves be encrypted.
 - **Anonymity** - a VPN can be used as a **proxy server**, which acts as an intermediary layer between a user's client and the servers they access. 
 - **Geo-spoofing/anti-censorship** - VPN companies usually host their servers in locations which have more lax Internet restrictions, such as for video- and music-streaming, than the countries they market their service to; these users connect to the VPN in order to spoof their location so as to appear as though they are accessing Web services from the location of these servers.

VPNs are less powerful than most of the general public believe, for a number of reasons. [This video explores some of the more common misconceptions surrounding VPNs.](https://www.youtube.com/watch?v=WVDQEoe6ZWY)

## Tasks
### View the ARP table
#### macOS, Linux, or Windows Subsystem for Linux
Open a terminal, then enter the following command:
```text
admin@ubuntu:~$ cat /proc/net/arp
```
You should see something like this:

```text
IP address		Hw type		Flags	HW address			Mask	Device
192.168.1.73	0x1			0x2		10:fe:ed:11:d4:ec	*		enp0s3
192.169.1.254	0x1			0x2		a0:39:ee:4e:5a:49	*		enp0s3
```
What might these represent?

#### Windows
Open a command prompt/Powershell, then enter the following command:
```text
 C:\WINDOWS\system32>arp -a
```
You should see one or more tables like this:
```text
Interface: 192.168.1.154 --- 0x18
  Internet Address      Physical Address      Type
  192.168.1.133         f4-30-b9-dc-fd-41     dynamic
  192.168.1.228         80-e8-2c-b3-d2-32     dynamic
  192.168.1.254         00-0c-29-3c-38-5f     dynamic
  192.168.1.255         ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
```
What might these represent?

### Find your node's networking information
#### macOS, Linux, or Windows Subsystem for Linux
Open a terminal, then enter the following command:
```text
admin@ubuntu~$ ifconfig
```
You should see a list of network adapters:
```text
wifi0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.43.109  netmask 255.255.255.0  broadcast 192.168.43.255
        inet6 2a01:4c8:1c00:be68:f94e:7ba5:ca3f:2673  prefixlen 64  scopeid 0x0<global>
        inet6 2a01:4c8:1c00:be68:2d49:57fe:237f:778a  prefixlen 128  scopeid 0x0<global>
        inet6 fe80::f94e:7ba5:ca3f:2673  prefixlen 64  scopeid 0xfd<compat,link,site,host>
        ether a0:af:bd:df:79:10  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth0: etc
```

Using this information, find your IPv4 address and subnet mask.

#### Windows
Open a command prompt/Powershell, then enter the following command:
```text
 C:\WINDOWS\system32>ipconfig/all
```
Doing this will give you a list of network adapters:
```text
Windows IP Configuration

   Host Name . . . . . . . . . . . . : DESKTOP-JLGAKSF
   Primary Dns Suffix  . . . . . . . :
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No

Ethernet adapter Ethernet:
   etc
```

Using this information, find your IPv4 address and subnet mask.

### Find network information using a subnet calculator
For this task, you will use your IPv4 address and subnet mask to discover more about your machine's connection to its local network.

Navigate to [this online subnet calculator](https://www.calculator.net/ip-subnet-calculator.html​).

Leave the network class as 'Any', then enter your machine's subnet mask and IP address into the corresponding fields.

Click calculate - you should receive a similar result to the following:

![Subnet calculator information](https://i.imgur.com/BsedIUN.png)

### Change your IP address
#### Find your machine's GPS location
Navigate to [this IP geolocation finder](https://www.iplocation.net/find-ip-address).

Which address, for which node, is this service using to locate your machine?

#### Installing Opera VPN
Download Opera VPN from [the official site](https://www.opera.com/features/free-vpn), then follow the installation instructions.

Once installed, open up a browser window in Opera VPN.

Navigate back to the IP geolocation finder - what is happening here?
