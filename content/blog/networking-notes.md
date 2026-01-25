+++
author = "Doğukan Meral"
title = "Networking notes"
date = "2026-01-06"
description = "Networking protocols, methods notes."
tags = [
    "networking",
]
categories = [
    "networking",
]
+++

## OSI Model

![](https://blog.smartbuildingsacademy.com/hubfs/Imported_Blog_Media/OSI-Model.png)

---

## Ethernet

![](https://blog.kakaocdn.net/dna/pmXyP/btrCNMR5vDF/AAAAAAAAAAAAAAAAAAAAAIeiiPZ0wy5Ipv03g5FBXzt4HB-rZkc129oZjJrbvGz8/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1767193199&allow_ip=&allow_referer=&signature=%2BGck98ZZxcUTMFWaehpkqHhyBuo%3D)

Physical and data-link layer

- **Destination address**: 6 bytes
- **Source address**: 6 bytes
- **Ether Type**: Payload content format

> Each computer has **unique** MAC address in the network

> To broadcast to every device (send to), destination address is set to `ff:ff:ff:ff:ff:ff`

---

## PPP (Point-to-point)

![](https://www.researchgate.net/publication/369542690/figure/fig124/AS:11431281130634753@1679913463550/PPP-frame-format.jpg)

Data-link layer

- **Address** and **Control** fields are not used for now (set to some default)
- FCS = Frame check sequence


---

## Internet Protocol (IP)

![](https://www.teracomtraining.com/images/xteracom-tutorial-IP-packets.gif.pagespeed.ic.D7Shsws3A0.png)

IP is a routed protocol

- **Version**: Specifies IP version (4 or 6)
- **Service type**: How packets are treated when routed (Quality of service)
- **8-16 bytes**: Related with IP fragmentation 
- **Time to live**: Value used to prevent routing loops (Decrement at every hop)
- **Protocol**: Which type of package is in  IP as data
- **Checksum**: frame check sequence value

### IP Fragmentation

- IP packet is larger than that link can support (MTU)
- **More fragments flag**:  There is more data to come with same **identification number**
- **Fragment offset**: Bytes offset of packet when reassembling (**fragment count** is the order)
- **Don't fragment**: Tell router not to fragment the packet (if packet size is > MTU, router drops the packet and  returns with an ICMP packet) (Sometimes used for security purposes to prevent attackers from inserting fragments)
> IPv6 doesn't have a DF bit, and it uses a "**Packet too big** ICMPv6 message"

---

## Internet control message protocol (ICMP)

- Used by network devices to send error messages e.g. unreachable host, network, port or protocol
- Echo request / reply (used by ping)
- it's **above** IP: carried in IP datagrams, protocol number 1

consists of:
    - type
    - code plus header
    - first 8 bytes of IP datagram causing error

[](https://www.researchgate.net/publication/330322672/figure/tbl2/AS:713945207828482@1547229210251/shows-the-ICMP-message-types.png)

### How Traceroute works?

1. source sends set of UDP segments to destination ( 1st set has TTL=1, 2nd set has TTL=2, etc...)
2. datagram in nth set arrives to nth router (router discards datagram and sends ICMP message including it's IP address)
3. when arrived at host, host returns ICMP "**port unreachable**" and it stops

---

## Maximum Transmission Unit (MTU) 

[video](https://youtu.be/LeaEmOUVEn0)

![](https://media.geeksforgeeks.org/wp-content/uploads/20220217165120/MaximumSegmentSize.png)

- **The largest frame** or packet that can be transmitted or received on an interface

---

## Subnetting

- Taking a network and dividing it into sub-networks

Attributes of subnetting:
    - # of ip addresses in sub-network
    - **cidr** / **subnet**: converting between them
    - **Network IP**: first IP address in each sub-network
    - **Broadcast IP**: last IP address in each sub-network

> Network and broadcast IP's are **NOT assigned** to any user in the network!   e.g.  in /24,  256 possible addresses but only 254 usable

[Solving subnets video](https://youtu.be/5-wlfAdcmFQ?list=PLIFyRwBY_4bQUE4IB5c4VPRyDoLgOdExE)

---

## Address resolution protocol (ARP)

![](https://geek-university.com/wp-content/images/ccna/how_arp_works.jpg)

- Tracking an IP address to a physical machine in LAN (MAC)
- Sending a broadcast (every device receives) into network and asking "**who (MAC address)** owns this IP address?"
- Mapping between IP and Ethernet
- For IPv4

1. packet arrives at a gateway to a host machine
2. prompts the ARP program to match the IP address with a MAC address
3.  host searches it's ARP-cache, if not found starts process

---

## Transmission Control Protocol (TCP)

- encapsulated **inside IP packet**
- connection oriented
- client **initializes** connection to server

**helps with**:
- packet loss
- re-ordering
- multiple conversations
- flow control (slow down if too many packets get dropped)

![](https://www.pynetlabs.com/wp-content/uploads/2024/01/tcp-header-format.jpeg)

- **checksum**: frame check sequence for TCP header and **data**
- **sequence number**: set to a random number at the beginning, increased by 1 for each byte

### TCP Connection

![](https://blog.pcarleton.com/images/TCP-3-way.png)

![](https://miro.medium.com/v2/resize:fit:1282/1*7VWQK2DJI-Ouyf-NW2IVvA.png)

---

## User Datagram Protocol (UDP)

- **Connectionless**: no handshaking
- "No frills", "bare bone" internet transport protocol
- UDP segments may be **lost** or delivered **out of order**
- Each UDP segment is handled independently

**Pros:**
- No connection establishment delay
- Simple: no connection state
- Small header size
- No congestion control: UDP can blast away as fast as desired

**Used by:**
- Streaming multimedia apps (loss tolerant, rate sensitive)
- DNS (can still work in a congested state)
- SNMP
- HTTP/3

> If reliable transfer needed over UDP (e.g., HTTP/3): add **reliability** and **congestion control** at **application layer**

![](https://homepages.uc.edu/~thomam/Net1/Diagrams/udp_pdu.png)

---

## Network Address Translation (NAT)

![](https://ipcisco.com/wp-content/uploads/2018/10/nat-types-www.ipcisco.com_.jpg)

### Port Address Translation (PAT)

![](https://study-ccna.com/wp-content/uploads/2018/08/pat_explanation.jpg)

---

## Dynamic Host Configuration Protocol (DHCP)

- Operates over **UDP**
- Dynamically assigns an IP address to a machine that connects to the network
- Delivers the DHCP lease (time for IP lease)
- Configures network settings (subnet mask, gateway & DNS IP addresses, MTU etc...) 

[DHCP explained](https://youtu.be/Cy0M54GSpBg)

![](https://documentation.meraki.com/@api/deki/files/7191/DHCP_DORA_Process_Diagram.png?revision=2)