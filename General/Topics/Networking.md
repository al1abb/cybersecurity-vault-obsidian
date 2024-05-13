## Things that make up Network

![[Things that make up a network.png]]

Mainly 4 categories:
1) Host Devices: They have an IP address on the network. End-user devices
2) Shared peripheral devices: Have no IP address on the network. They communicate with other devices on the network through the device they are connected to. (Ex: web camera)
3) Network communication devices: Things that connect hosts. (Ex: Hub, Switch, Router)
4) Network communication environment (Media): Things that connect hosts and network devices. (Ex: copper wire, fiber optic, wireless)

---
## Network Topologies

### Bus Topology

In this type of topology, as can be seen in the figure, all computers are connected to each other via the same trunk cable without using a network device such as a hub or switch. This topology has many disadvantages. 

Only one computer at a time can send data. If another computer tries to send it, a collision will occur. Also, if there is a break in the trunk cable, all subsequent hosts lose their network connection. It is no longer used today.

![[Bus Topology.png]]

### Ring Topology

In this topology, as in the bus topology, like a hub or switch computers are connected to each other over the same cable without using a network device. A failure in this cable would cause all hosts to lose their network connection. It is no longer used today.

![[Ring Topology.png]]
### Star Topology

It is a topology often used today where a network device such as a hub or switch is at the center and hosts are connected to the network device through network media. 

In the event of a cable or port failure, only the relevant computer is affected. Fault detection is easily done with the leds on modern switches.

![[Star Topology.png]]

### Mesh Topology

It is a topology created by connecting hosts or network devices to each other with more than one network media. In this way, it is aimed to prevent network interruptions by providing a high level of redundancy. It costs more because more network devices and network media are used.

![[Mesh Topology.png]]

---
## Network Types

Networks are divided into LAN (Local Area Network) and WAN (Wide Area Network) according to their physical size.

![[WAN and LAN.png]]

**LAN (Local Area Network):** 
LAN is a network structure that provides services to users in a limited area; that is, in a small geographical area and allows hosts to access the network. SOHO (Small Office Home Office) networks, which we encounter most often today, are the most common LAN example. 
In a SOHO network, host devices such as computers and printers communicate with each other via Ethernet cables through a switch, and can also access the Internet through a router.

**WAN (Wide Area Network):** 
It is a network structure that connects LANs in different locations. 
Today, when we think of WAN connectivity, we usually think of the Internet. The Internet means inter-network and connects all computers worldwide.

---
## OSI Model

When networks first emerged, each manufacturer created a structure in which only their own devices could communicate with each other. For example, according to the model IBM called Systems Network Architecture (SNA) and published in 1974, only IBM brand devices could connect to this network. Other manufacturers also created their own network models.

In order to prevent this complexity and create a standard model, ISO (International Organization for Standardization) announced the OSI (Open Systems Interconnection) model consisting of 7 layers. This model consists of 7 layers as shown in the figure and each layer works independently of each other and performs a separate task.

![[OSI Model 3.png]]

Let's see what each of these layers do

### Layer 7. Application

It is the layer that provides an interface to the user. It is between the user and the application. Many protocols such as HTTP, FTP, SMTP, TFTP, DNS protocols work at this layer.

### Layer 6. Presentation

It is the layer where data is put into a specific format. An example is putting an image file into jpeg format.

### Layer 5. Session

The establishment, use and termination of the connection between the application on the two computers sending and receiving the data is done at this layer.

### Layer 4. Transport

It is at this layer that the data is broken down, sorted and reassembled on the receiving computer. In this layer, source and destination port information is added to the data coming from the upper layers.

### Layer 3. Network

Routing the data to the relevant network and assigning logical addresses to the data takes place at this layer. In this layer, source and destination IP address information is added to the data coming from the upper layers.

### Layer 2. Data Link

Converting the data into bits and sending it to the lower layer takes place at this layer. In this layer, source and destination MAC address information is added to the data coming from the upper layers.

### Layer 1. Physical

It is the layer where data is transmitted as an electrical signal over copper cable, as a light signal over fiber optic cable, or wirelessly as a radio frequency signal.

> [!info] Did you know this?
>  
> ARPANET (Advanced Research Projects Agency Network), now known as DARPA (Defense Advanced Research Projects Agency), was the world's first working packet-switching network developed during the Cold War for the US Department of Defense's Advanced Research Projects Agency, and the ancestor of the Internet. 
> 
> Developed to connect research and researchers, ARPANET gave rise to the TCP/IP protocol, which later led to the development of the Internet. Source wikipedia

> [!info] Wi-Fi AP in OSI model
> WiFi (802.11) operates at the first two layers of the OSI model, in other words, **the physical layer and the data link layer**

---
## TCP/IP Protocol Stack

Although OSI was put forward as a standard model, the TCP/IP protocol stack came to the forefront in the 1990s because it is the set of protocols that form the basis of the Internet and was started to be developed earlier. The TCP/IP Protocol stack consists of 4 layers, as shown in the figure, and the role of each layer is similar to that of the OSI model.

![[TCP-IP Layers.png]]

The figure below compares the OSI model and the TCP/IP Protocol set. 
The Application, Presentation and Session layers in OSI appear as a single Application layer in TCP/IP. Again, the Data Link and Physical layer in OSI is called Network Access in TCP/IP.

![[Assets/Images/Networking/OSI vs TCP-IP.png]]

---

## TCP vs UDP

There are 2 main transport protocols: 
TCP (Transmission Control Protocol) 
and 
UDP

TCP is reliable. 
It makes sure your data is received. If it does not receive confirmation that you got it, TCP will send it again

It does 3-way handshake before exchanging data. After the handshake, communication is possible
- SYN (Synchronization)
- SYN+ACK
- ACK (Acknowledgement)

UDP is fast
For real-time applications, it does not make sense to use TCP and resend the data

---
## Network Devices

### Hub
![[Hub.png]]

It is the network device located in the center in Star topology. It is produced as 4-8-16-24-48 etc. ports. 

Since hubs transmit the signal from one port to all ports except the incoming port, they cause network congestion and collisions.

Although mechanisms such as half duplex (one-way communication) and CSMA/CD have been developed to prevent these, they are not used much anymore with the production of switches.

### Switch

![[Switch.png]]

In Star topology, it is the device at the center of the network. It is usually produced as 8-24 and 48 ports. There are manageable and unmanageable types. 

By creating a MAC table containing the information of which MAC address host is on which port, it ensures that the data coming from the sending computer goes only to the receiving computer. For this reason, it can work in full duplex (two-way) and there are no conflicts like hubs. 

Today, switches with 100 Mbit- 1 Gbit- 10 Gbit and 40 GBit ports have been produced and have become the most basic network component of LANs.

### Router

![[Router.png]]

Network devices that connect different local area networks (LANs) are called routers. The wide area network (WAN) we call the Internet is a network of thousands of routers. The figure above shows an ADSL router, often incorrectly called a modem, which is also a wireless access point. This router routes the computers in our home to the Internet.

> [!info] Modem VS Router
> A modem and router are both essential components of most home networks, especially for those who have a home office and work remotely. 
> 
> **The modem is responsible for sending and receiving signals from the ISP, while the router disperses the signal to devices on the network**.

### Firewall-UTM

In addition to its firewall feature, it is now known as UTM (Unified Threat Management-Unified Security System) with many features such as antivirus gateway, web filtering, hotspot, etc.

---
## Network Media

### Copper Cable

Copper cables are usually UTP (Unshielded Twisted Pair)
are produced in various categories (Cat5, Cat5e, Cat6, Cat6A, Cat6A, Cat7 and Cat7A). In order to increase the sensitivity of UTP cables to magnetic field and radio frequency interference, both wires are twisted together in a total of 8 wires. As the number of twists at a given distance increases, the category and speed increases. STP (Shielded Twisted Pair) is used where magnetic field and radio frequency interference is very intense. STP cables are shielded and grounded from end to end. STP cables are more expensive and difficult to handle than UTP cables. Mostly UTP cables are used in the market. UTP cables support an average distance of 100 meters. 

### Fiber Optic Cable

It is a cable made of hair-thin glass or plastic, through which electrical signals are transmitted in the form of light. Problems such as interference of UTP copper cables due to magnetic field and radio frequency are not experienced in fiber optic cable. Therefore, it is possible to transmit data over longer distances (on average 2000 meters) without loss and quickly. 

Today, fiber optic cables that weave our world like a spider web constitute the infrastructure of the Internet. You can visit the website https://www.submarinecablemap.com/ to see the fiber optic cable lines that surround the world and are laid under the seas.

### Wireless

Today, devices such as computers, laptops and cell phones are mostly wireless
we see that it is connected to the network. Wireless LAN and IEEE 802.11 standards
Wireless network technologies, which are defined by wireless network technologies, are accelerating day by day and expanding their coverage area. 

In Wireless LANs, which we also call Wi-Fi, data is transmitted in the form of radio waves. Routers with wireless connection feature are called wireless access points. 

Wireless access points are similar to hubs in terms of operating logic. For this reason, we cannot get the same efficiency from a wireless connection as from a wired connection. Speed reductions and disconnections may occur due to thick and dense iron walls absorbing wireless signals and interference from other wireless access points. The IEEE standards used by wireless access points are shown in the table below. Wireless network devices manufactured in recent years come with the 802.11 ac standard. This standard operates at 5GHz and theoretically provides Gbit speed communication.

---
## What is a MAC address?

A computer needs a **NIC (Network Interface Card)** to connect to a network. This network interface card can be wired or wireless. 
The figure shows an external network interface card used in desktop computers.

![[Network Interface Card.png]]

Today, these wired network interface cards come integrated on the motherboard. All of these wired or wireless network interface cards have a unique MAC (Media Access Control) address, which consists of 48 bits and is represented by a hexadecimal number system (16-number system). 

A router's interface configured as the Default Gateway also has a MAC address. As we mentioned in the OSI model, the source and destination MAC address information is added to the data at layer 2. 

In Windows, we can find out the MAC address of our network interface card with the ipconfig/all command on the command line (CMD). MAC address is also called Hardware address and Physical address.

---
## IP Protocol

All devices connected to the network, whether on the local network or on the Internet, must have a unique logical address. This address is called the IP address (IP: Internet Protocol). The IP side of the TCP/IP protocol stack is responsible for this addressing. IP addresses are of two types, IPv4 and IPv6. IPv4 will be described in detail here. 

IPv4 addresses consist of 32 bits. These addresses, which actually consist of 32 zeros and ones (0-1), are written in decimal (decimal) number system for ease of use. 
For example, the IP address 192.168.1.5, which is shown as decimal, can actually be opened as binary 11000000.10101000.00000001.00000101. 

32-bit IPv4 addresses are written in 4 groups of 8s with a dot between them and each group of 8 is called an octet. So IPv4 addresses consist of 4 octets.

---
## IP Addresses and Classes

IPv4 addresses are divided into 5 classes, A, B, C, D and E, according to the first bits of their first octet. 
The first bit in the first octet of class A addresses is zero, 
The first bit in the first octet of class B addresses is one, 
The first two bits in the first octet of class C addresses are one, 
The first three bits in the first octet of class D addresses are one, and 
The first four bits in the first octet of class E addresses are one. 

The following table shows the IP address classes and ranges.

![[IP address classes.png]]

IP addresses are divided into public and private. 

Private IP addresses are used on the LAN and are not routed to the Internet, while public IP addresses are used on the WAN, the Internet. 

The IP addresses of multiple computers used on the LAN are subjected to a process called NAT (Network Address Translation) when going out to the Internet. Most routers and firewalls perform NAT automatically.

Below are the public IP addresses according to their class.
Class A `1.0.0.0-126.255.255.255`
Class B `128.0.0.0-191.255.255.255`
Class C `192.0.0.0-223.255.255.255`

---
## Subnet Mask

The Subnet Mask indicates which part of an IP address is network and which part is host. 
Ones in an octet indicate the network part while zeros indicate the host part. 
The subnet mask is 
255.0.0.0.0 for class A IP addresses, 
255.255.0.0 for class B IP addresses and 
255.255.255.255.0 for class C IP addresses. 

For example, the default subnet mask for IP address 192.168.1.10 is 255.255.255.255.0. 
Thus 192.168.1. indicates the network part while 10 indicates the host part. 
In other words, the IP addresses of all computers on this network start with 192.168.1, and 1,2,3,4,........ and so on until 255.

![[Subnet Mask.png]]

---
## DNS

When visiting a site on the Internet, we type an address like www.site.com in the address line of our browser. This is called a domain name. After all, there is an IP address for this domain name. However, since it would be difficult to remember IP addresses, a system called DNS (Domain Name System) was developed to match IP addresses with domain names. 

For example, when you want to visit www.google.com, as we mentioned in the OSI model, it is necessary to learn this address first since it is necessary to write the destination IP address in the Network layer. 

When configuring the IP address on the computer, the DNS server address is also written. This DNS server is asked what is the IP address of www.google.com and the IP address learned in the response is written as the destination IP address. 

Of course, before asking the DNS server directly, the host file is checked in the Windows operating system. Then the DNS cache, which you can view with the `ipconfig /displaydns` command, is checked. If it is not found there, the DNS server is asked. Unless you have applied a different setting, you usually use the DNS server address provided by your Internet service provider.

---
## DHCP

We said that every device connected to the network needs a unique IP address. 
Along with the 
IP address, 
the subnet mask, 
default gateway and 
DNS server information 
must also be assigned to a host. If we want, we can do this statically as you can see in the figure.

![[TCP-IPv4 Settings Windows.png]]

However, a typo will cause the configuration to not work correctly, and in a very large network it will be difficult to configure IP to each host individually. 

With DHCP (Dynamic Host Configuration Protocol), we ensure that IP configuration happens automatically. By checking the Obtain an IP address automatically option, we enable a host to obtain its IP configuration from a DHCP server on our network.

---
## Types of Communication on a LAN

Communication between computers on a local area network can be unicast, broadcast and multicast. 

Unicast: A message from one computer goes to only one computer. 
Broadcast: A message from one computer goes to all computers in that network. 
Multicast: A message from one computer goes to a group of computers. 

---
## ARP Protocol

In a network, computers communicate as follows. 
According to the figure below, PC1 wants to communicate with PC4. 
We said that the destination MAC address is added to the data at layer 2 of OSI (Data Link). So PC1 first looks in the arp table for the MAC address of PC4 with IP address 192.168.1.5. The ARP table can be viewed with `arp -a` on the command line (CMD). If not available, it sends a broadcast message to all computers on the network with the destination MAC address FF-FF-FF-FF-FF-FF-FF-FF. This message is called an ARP request. 

The message means: I want to communicate with 192.168.1.5 on my network with IP address 192.168.1.2 and MAC address AAA, but I don't know its MAC address and I want to know it. All computers on the network receive the message and throw it away, except PC4. 

PC4 sends an ARP Reply message with the MAC address. PC1 receiving the ARP Reply has now learned the MAC address of PC4 and can write it as the destination MAC address in Layer 2. 
This process to learn the MAC address of a computer whose IP address is known is called ARP (Address Resolution Protocol).

There are many protocols in our local network that generate ARP-like broadcast messages. Although an operation is performed in the end, excessive broadcast traffic in a local area network causes congestion and slowness in the network. VLAN (Virtual LAN) solution is used to minimize broadcast traffic in local networks with many computers in the market.

---
## Some commands

### ping

The ping command is a program written by Mike Muus for testing end-to-end connectivity. It sends 4 32 byte ICMP Echo Request packets on the Windows command line and waits for an ICMP Echo Reply. If the 4 packets are exchanged without loss, the connection between the two computers is confirmed.

### ipconfig (ifconfig in Linux)

The ipconfig command is used to view the IP configuration of our computer. 
To see the entire configuration (MAC addresses, etc.) we use the ipconfig/all command.

### nslookup

With the nslookup command, we display the DNS server used by our computer and then allow the domain names we enter to be resolved.

### tracert

The Tracert program uses ICMP messages like ping. By tracing the packets going to the target computer we can see where the problems and delays are.

---
## Related

Check out this network drawing: [[Networking drawings|Networking drawings]]