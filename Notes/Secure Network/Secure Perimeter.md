---
layout: default
---
# Secure Perimeter

A **secure perimeter** is a form of protection that consists of devices or techniques added to the edge of a managed network. 
- Everything outside of the secure perimeter is considered untrusted.
- A remote device can also be a part of a secure perimeter, as long as it is using a secure remote access.

The trusted zone for a company is the IT managed portion of devices and applications.
- The secure perimeter protects the trusted zone.

The secure perimeter consists of specific applications for
- authentication
- authorization
Which protects and provide confidentiality, while filtering traffic to the trusted zone 

## Security in the OSI Model 
The secure perimeter can filter traffic at different **[OSI Model](https://github.com/ryancranie/cybersecurity-osint/blob/main/Notes/Network%20Security/OSI%20Model.md) Layers**.

7. **Application**
	- End User Layer
	- *HTTP, FTP, IRC, SSH, DNS*
6. **Presentation**
	- Syntax Layer
	- *SSL, SSH, IMAP, FTP, MPEG, JPEG*
5. **Session**
	- Sync and send to port
	- *API's, Sockets, WinSock*
4. **Transport**
	- End-to-end connections
	- *TCP, UDP*
3. **Network**
	- Packets
	- *IP, ICMP, IPSec, IGMP*
2. **Data Link**
	- Frames
	- *Ethernet, PPP, Switch, Bridge*
1. **Physical**
	- Physical Structure
	- *Coax, Fiber, Wireless, Hubs, Repeaters*

### Data Link
At the **Data Link** layer, a secure perimeter device can perform Media Access Control (**MAC**).
- example: IT manager creates an Access Control List (**ACL**): A defined list of known addresses in the trusted network.
	- The secure perimeter would only allowed known devices through into the network.

A Media Access Control (**MAC**) address is a unique identifier assigned to a Network Interface Controller (**NIC**).

An Access Control List (**ACL**) is a list that defines trusted devices and specific actions for a particular network.

### Transport
At the **Transport** layer, a secure perimeter device can perform packet filtering.
- Packet Filtering allows or denies packets based on a configured set of rules.
- **Stateless** packet filtering: each packet is checked based on its
	- IP addresses
	- source/dest ports
	- [protocol](https://github.com/ryancranie/cybersecurity-osint/blob/main/Notes/Network%20Technologies/Protocols.md)
- **Stateful** packet filtering: Returned traffic is validated, only if it matches corresponding incoming traffic. The security device filters traffic and tracks the 
	- 5-Tuple check
		- This characterizes and allows tracking of a TCP/IP connection via these parameters:
			- source IP address
			- source port number
			- destination IP address
			- destination port number
			- protocol
	- TCP/IP connection state

At the **Transport** layer, a secure perimeter device can also perform Network Address Translation (**NAT**) filtering.
- NAT Filtering can translate a public IP address to a private IP address, and vice versa
	- A translation is required to access the internal resource from the internet
- This protects
	- Private Network
		- a private network is a network that has its own private address space of IP addresses 
	- IP
	- port

### Application
At the **Application** layer, a secure perimeter device can perform proxy filtering, through a Man-in-the-middle mechanism.
- this gateway hides the internal user from the internet
- The main-in-the-middle mechanism intercepts communication and splits it in two
	- All packets are then received by proxy: the packets can then be modified, before the proxy forwards them to the other end.

At the **Application** layer, a secure perimeter device can also act as an application gateway (**ALG**).
- the ALG inspects the packets up to layer 7
	- allows opening of TCP/UDP data ports, dynamically.
	- example: protocols like FTP or SIP have a control connection that provides the port used for data communication
		- the secure perimeter device opens this temporary port only for the corresponding sessions.

## Limitations of Secure Perimeters
- Remote Working
- Bring-Your-Own-Devices (**BYOD**)
- Internet of Things (**IoT**)
- Cloud Working Model

## Links
### Revision History
001: 2024-09-26 - INITIALIZE

---
<font size=3><b>[SECURE NETWORK CONTENTS](https://github.com/ryancranie/cybersecurity-osint/blob/main/Contents/-%20Secure%20Network%20Contents.md)<br>
[README](https://github.com/ryancranie/cybersecurity-osint/blob/main/README.md)<br>
[LICENSE](https://github.com/ryancranie/cybersecurity-osint/blob/main/LICENSE)</b></font>