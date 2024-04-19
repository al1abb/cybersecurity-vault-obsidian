### ARP - Address Resolution Protocol
- Simple protocol used to map IP address of a machine to its MAC address
- Communication inside the network is carried out using the MAC address

![[ARP Protocol.png]]
*ARP Request / Broadcast message*

Each computer has an ARP table, that links IP addresses on the same network to their MAC address
It can be viewed on Linux and Windows machines using [[arp]] command:
`arp -a`

### ARP Spoofing Explained

ARP Spoofing happens when we get in between the victim and the router device by modifying the ARP table and thus becoming MITM (Man In The Middle)

![[ARP Spoofing Attack.png]]

### Why ARP spoofing is possible?

1) Clients accept responses even if they did not send a request
2) Clients trust response without any form of verification

> [!warning] Port forwarding
> Port forwarding has to be enabled in a machine to make sure the requests flow to router

#security/mitm 