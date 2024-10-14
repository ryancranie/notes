# DHCP
## DHCP for IPv4
Dynamic Host Configuration Protocol (DHCP) is commonly used for assigning IPv4 address information to a network host.

DHCP allows a DHCP client to obtain an 
- IP address
- subnet mask 
- default gateway 
- IP address 
- DNS server IP address 
- other types of IP addressing information
from a DHCP server

## DORA Process
1. Discover
2. Offer
3. Request
4. Acknowledge

DORA defines the exchange of messages that occurs as a DHCP client obtains IP addressing information from a DHCP server.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240924200041.png" width="500">

### Discover
When a DHCP client initially boots, it has no
- IP address 
- default gateway 
- other configuration information

Therefore, the way a DHCP client initially communicates is by sending a **broadcast DHCPDISCOVER** message to destination IP `255.255.255.255` and destination MAC `FFFF:FFFF:FFFF` attempting to discover a DHCP server. The source IP is `0.0.0.0`, and the source MAC is the MAC address of the sending device.

### Offer
When a DHCP server receives a **DHCPDISCOVER** message, it can respond with a **DHCPOFFER** message with 
- an unleased IP address
- subnet mask 
- default gateway information 

Because the **DHCPDISCOVER** message is sent as a broadcast, more than one DHCP server might respond with a **DHCPOFFER**. The client typically selects the server that sent the *first **DHCPOFFER** response it received*.

### Request
The DHCP client communicates with the selected server by sending a broadcasted **DHCPREQUEST** message indicating that it will be using the address provided in the **DHCPOFFER** and, as a result, wants the associated address leased to itself.

### Acknowledge
Finally, the DHCP server responds to the client with a **DHCPACK** message indicating that the IP address is leased to the client and includes any additional DHCP options that might be needed at this point, such as the lease duration.

## DHCP Relay Agent
The **DHCPDISCOVER** message is sent as a broadcast but it cannot cross the router boundary. Therefore, if a client resides on a different network from the DHCP server, *you need to configure the default gateway of the client as a DHCP relay agent* to forward the broadcast packets as unicast packets to the server.

You use the `ip helper-address {ip_address}` interface configuration mode command to configure a router to relay DHCP messages to a DHCP server in the organization.

Here, the DHCP client belongs to the `172.16.1.0/24` network, whereas the DHCP server belongs to the `10.1.1.0/24` network. Router R1 is configured as a DHCP relay agent, using the syntax shown in Example 1-3

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240924201101.png" width="500">

The `service dhcp` command enables the DHCP service on the router. It is usually not required because the DHCP service is enabled by default.

The `ip helper-address 10.1.1.2` command specifies the IP address of the DHCP server. If the wrong IP address is specified, the DHCP messages are relayed to the wrong device. In addition, the `ip helper-address` command must be configured *on the interface that is receiving the **DHCPDISCOVER** messages from the clients*.

As a DHCP relay agent, the router relays a few other broadcast types in addition to a DHCP message. Other [protocols](https://notes.ryancranie.com/Notes/Network%20Technologies/Protocols) that are forwarded by a DHCP relay agent include the following: 
- TFTP 
- Domain Name System (DNS) 
- Internet Time Service (ITS) 
- NetBIOS name server 
- NetBIOS datagram server
- BootP
- TACACS

## DHCP Message Types

| DHCP Message | Description                                                                                                                                                                                                                                  |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| DHCPDISCOVER | A client sends this message in an attempt to locate a DHCP server. This message is sent to broadcast IP address 255.255.255.255, using UDP port 67.                                                                                          |
| DHCPOFFER    | A DHCP server sends this message in response to a DHCPDISCOVER message, using UDP port 68.                                                                                                                                                   |
| DHCPREQUEST  | This broadcast message is a request from the client to the DHCP server for the IP addressing information and options that were received in the DHCPOFFER message.                                                                            |
| DHCPDECLINE  | This message is sent from a client to a DHCP server to inform the server that an IP address is already in use on the network.                                                                                                                |
| DHCPACK      | A DHCP server sends this message to a client and includes IP configuration parameters.                                                                                                                                                       |
| DHCPNAK      | A DHCP server sends this message to a client and informs the client that the DHCP server declines to provide the client with the requested IP configuration information.                                                                     |
| DHCPRELEASE  | A client sends this message to a DHCP server and informs the DHCP server tha tthe client has released its DHCP lease, thus allowing the DHCP server to reassign the client IP address to another client.                                     |
| DHCPINFORM   | This message is sent from a client to a DHCP server and requests IP configuration parameters. Such a message might be sent from an access server requesting IP configuration information for a remote client attaching to the access server. |

## Router as a DHCP Client or DHCP Server

Router configured as a DHCP client so the router can obtain its IP address from a DHCP server:

```
R1# configure terminal
R1(config)# int fa 0/1
R1(config-if)# ip address dhcp 
```

Router configured as a DHCP server

```
R1(config)# ip dhcp excluded-address 10.8.8.1 10.8.8.10
R1(config)# ip dhcp pool POOL-A 
R1(dhcp-config)# network 10.8.8.0 255.255.255.0 
R1(dhcp-config)# default-router 10.8.8.1 
R1(dhcp-config)# dns-server 192.168.1.1 
R1(dhcp-config)# netbios-name-server 192.168.1.2
```

Note: You do not have to include the IP address of the router interface in the excluded-address because the router never hands out its own interface IP address.

## DHCP Troubleshooting Issues

| Issue                                                     | Solution                                                                                                                                                                                                                                                                                                        |
| --------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **A router not forwarding broadcasts**                    | A router needs to be explicitly configured to act as a DHCP relay agent if the DHCP client and DHCP server are on different subnets.                                                                                                                                                                            |
| **DHCP pool out of IP addresses**                         | Once a pool becomes depleted, new DHCP requests are rejected.                                                                                                                                                                                                                                                   |
| **Misconfiguration**                                      | The configuration of a DHCP server might be incorrect.                                                                                                                                                                                                                                                          |
| **Duplicate IP addresses**                                | Handing out an IP address to a client that is statically assigned to another host.                                                                                                                                                                                                                              |
| **Redundant services not communicating**                  | DHCP servers can coexist with other DHCP servers for redundancy. If inter-server communication fails, the DHCP servers hand out overlapping IP addresses to their client’s.                                                                                                                                     |
| **The “pull” nature of DHCP**                             | The DHCP server has no ability to initiate a change in the client IP address after the client obtains an IP address. The DHCP server cannot push information changes to the DHCP client.                                                                                                                        |
| **Interface not configured with IP address in DHCP pool** | A router or a multilayer switch that is acting as a DHCP server must have an interface with an IP address that is part of the pool/subnet that it is handing out IP addresses for. This is not the case if a relay agent is forwarding DHCP messages between the client and the router that is the DHCP server. |

## DHCP Troubleshooting Commands

The `show ip dhcp conflict` command 

```
R1# show ip dhcp conflict 
IP address Detection method Detection time 172.16.1.3 
Ping Oct 15 2018 8:56 PM
```


The output indicates a duplicate `172.16.1.3` IP address on the network, which the router discovered via a ping. You clear the information displayed by issuing the clear `ip dhcp conflict *` command after resolving the duplicate address issue on the network.

Example 1-6 shows the `show ip dhcp binding` command. The output indicates that IP address `10.1.1.10` was assigned to a DHCP client. You can release this DHCP lease with the clear `ip dhcp binding 10.1.1.10` command.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240924203342.png" width="500">

Example 1-7 shows sample output from the `debug ip dhcp server events` command. The output shows updates to the DHCP database.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240924203415.png" width="500">

Example 1-8 shows sample output from the `debug ip dhcp server packet` command. The output shows a DHCPRELEASE message being received when a DHCP client with IP address 10.1.1.3 is shut down. 

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240924203604.png" width="600">

You can also see the four-step process of a DHCP client obtaining IP address `10.1.1.4` with the following messages:

- DHCPDISCOVER
- DHCPOFFER
- DHCPREQUEST
- DHCPACK

## DHCP for IPv6

- Manually assigning IP addresses (either IPv4 or [IPv6](https://notes.ryancranie.com/Notes/Network%20Technologies/IPv6)) is not a scalable option. 
- With IPv4, DHCP provides a dynamic addressing option. With IPv6, you have three dynamic options to choose from: stateless address autoconfiguration (SLAAC), stateful DHCPv6, or stateless DHCPv6. 
- This section looks at the issues that might arise for each and how to troubleshoot them.

## SLAAC

SLAAC stands for **Stateless Address Autoconfiguration**

SLAAC is designed to enable a device to configure its own 
- IPv6 address
- prefix
- default gateway 

without a DHCPv6 server. 

Windows PCs automatically have SLAAC enabled and generate their own IPv6 addresses

On Cisco routers, if you want to take advantage of SLAAC, you need to **enable it manually** on an interface with the `ipv6 address autoconfig` command.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240924203951.png" width="600">

When a PC and router interface are enabled for SLAAC, they send a **Router Solicitation (RS)** message to *determine whether there are any routers connected to the local link*.

They wait for a router to send a **Router Advertisement (RA)** that identifies the prefix being used by the router (the default gateway) connected to the same network.

They use that prefix information to generate their own IPv6 address in the same network as the router interface that generated the **RA**.

The router uses **EUI-64** for the **interface ID**, and the PC randomly generates the interface ID unless it is configured to use EUI-64. In addition, the PC uses the *IPv6 link-local address of the device that sent the RA as the default gateway address*.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240924204201.png" width="600">

The source IPv6 address is the Gig0/0 link-local address, and the source MAC address is the MAC of Gig0/0. The destination IPv6 address is the *all-nodes link-local multicast IPv6 address* `FF02::1`. The destination MAC address is the all-nodes destination MAC address `33:33:00:00:00:01`. *By default, all IPv6-enabled interfaces listen for packets and frames destined for these two addresses*.

To verify an IPv6 address generated by SLAAC on a router interface, use the `show ipv6 interface` command.

As shown in Example 1-16, the *global unicast address* was generated using SLAAC. Also notice at the bottom of the example that the default router is listed as the link-local address of R1. However, note that this occurs only if IPv6 unicast routing was not enabled on router R1 and, as a result, the router is acting as an end device.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240924204425.png" width="600">

## Router Advertisements (RA)

RAs are generated by default on router interfaces only if 
1. the router interface is enabled for IPv6
2. IPv6 unicast routing is enabled
3. RAs are not being suppressed on the interface. 

Therefore, if SLAAC is not working, check the following:
1. ipv6 unicast-routing is configured. 
2. The appropriate interface is enabled for IPv6 by using the `show ipv6 interface` command. 
3. The router interface advertising RAs has a /64 prefix (SLAAC works only if the router is using a /64 prefix.). 
4. That RAs are not being suppressed on the interface, as shown in Example 1-18.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240925054537.png" width="600">

## Stateful DHCPv6

With SLAAC a device can determine its IPv6 address, prefix, and default gateway but not much else.

In modern networks devices may need additional information such as 
- NTP server 
- domain name 
- DNS server 
- TFTP server 

To hand out the IPv6 addressing information along with all optional information, use a **Stateful DHCPv6 server**. Both Cisco routers and multilayer switches may act as DHCP servers.

Example 1-21 provides a sample DHCPv6 configuration on R1 and the ipv6 dhcp server interface command necessary to enable the interface to use the DHCP pool for handing out IPv6 addressing information.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240925054809.png" width="500">

Although it is not pictured in Example 1-21 the `ipv6 nd managed-config-flag` interface configuration command on interface GigabitEthernet 0/0 ensures that the **RA** from router R1 *informs the client to contact a DHCPv6 server* for all 
- IPv6 network addressing
- prefix length
- and other information

## Stateless DHCPv6

Stateless DHCPv6 is a combination of SLAAC and DHCPv6.

The router’s **RA** is used by the clients to automatically determine the
- IPv6 address 
- prefix
- default gateway 

Also included in the RA is a **flag** that *tells the client to get other non-addressing information from a DHCPv6 server*, such as 
- the address of a DNS server 
- a TFTP server. 

To accomplish this, ensure that the `ipv6 nd other-config-flag` interface configuration command is enabled. This ensures that the RA *informs the client that it must contact a DHCPv6 server for other information*.

In Example 1-23, the output of `show ipv6 interface gigabitEthernet 0/0` states that hosts obtain IPv6 addressing from stateless autoconfig and other information from a DHCP server

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240925055346.png" width="500">

## DHCPv6 Operation

DHCPv6 has a four-step negotiation process, like IPv4. However, DHCPv6 uses the following messages:

1. SOLICIT
	- A client sends this message to locate DHCPv6 servers using the multicast address FF02::1:2, which is the all-DHCPv6-servers multicast address.
2. ADVERTISE
	- Servers respond to SOLICIT messages with a unicast ADVERTISE message, offering addressing information to the client.
3. REQUEST
	- The client sends this message to the server, confirming the addresses provided and any other parameters.
4. REPLY
	- The server finalizes the process with this message.

## DHCPv6 Messages

| DHCP Message            | Description                                                                                                                                                                        |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **SOLICIT**             | A client sends this message in a attempt to locate a DHCPv6 server.                                                                                                                |
| **ADVERTISE**           | A DHCPv6 server sends this message in response to a SOLICIT: indicating it is still availiable                                                                                     |
| **REQUEST**             | This message is a request for IP configuration parameters sent from a client to a specific DHCPv6 server                                                                           |
| **CONFIRM**             | A client sends this message to a server to determine whether the address it was assigned is still appropriate                                                                      |
| **RENEW**               | A client sends this message to the server that assigned the address in order to extend the lifetime of the addresses configured.                                                   |
| **REBIND**              | When there is no response to a RENEW, a client sends a REBIND message to a server to extend the lifetime on the address assigned.                                                  |
| **REPLY**               | A server sends this message to a client containing assigned address and configuration parameters in response to a SOLICIT, REQUEST, RENEW or REBIND message received from a client |
| **RELEASE**             | A client sends this message to a server to inform the server that the assigned address is no longer needed                                                                         |
| **DECLINE**             | A client sends this message to a server to inform the server that the assigned address is already in use                                                                           |
| **RECONFIGURE**         | A server sends this message to a client when the server has new or updated information.                                                                                            |
| **INFORMATION REQUEST** | A client sends this message to a server when the client only needs additional configuration information without any IP address assignment.                                         |
| **RELAY-FORW**          | A relay agent uses this message to forward messages to a DHCP server                                                                                                               |
| **RELAY-REPL**          | A DHCP server uses this message to reply to the relay agent.                                                                                                                       |

## DHCPv6 Relay Agent

If you review the multicast address of the **SOLICIT** message, notice that it is a *link-local scope multicast address* (It starts with `FF02`). Therefore, the multicast does not leave the local network, and the client is not able to reach the DHCPv6 server.

To relay the DHCPv6 messages to a DHCPv6 server in another network, the local router interface in the network the client belongs to **needs to be configured as a relay agent** with the `ipv6 dhcp relay destination` interface configuration command.

Example 1-24 shows interface Gigabit Ethernet 0/0 configured with the command `ipv6 dhcp relay destination 2001:db8:a:b::7`, which is used to *forward **SOLICIT** messages to a DHCPv6 server* at the address listed.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240925060643.png" width="700">

## Commands

service dhcp: Enables the DHCP service on the router (often unnecessary as it's enabled by default)

`ip helper-address [ip_address]`: Configures a router interface to act as a DHCP relay agent, forwarding DHCP broadcasts to a server at the specified IP address

`ip address dhcp`: Configures a router interface to obtain an IP address from a DHCP server

`ip dhcp excluded-address [start_IP] [end_IP]`: Defines a range of IP addresses to exclude from the DHCP pool, preventing them from being assigned to clients

`ip dhcp pool [pool_name]`: Creates a DHCP pool and enters DHCP pool configuration mode for further settings

`network [network_address] [subnet_mask]`: Specifies the network address and subnet mask for the DHCP pool

`default-router [IP_address]`: Sets the default gateway IP address to be assigned to DHCP clients

`dns-server [IP_address]`: Specifies the DNS server IP address to be provided to DHCP clients

`netbios-name-server [IP_address]`: Assigns a NetBIOS Name Server IP address to DHCP clients

`show ip dhcp conflict`: Displays detected IP address conflicts on the network, including the conflicting IP address, detection method, and time

`clear ip dhcp conflict *`: Clears the DHCP conflict information after resolving the issue

`show ip dhcp binding`: Lists IP addresses leased by the DHCP server to clients, along with their MAC addresses and lease duration

`clear ip dhcp binding [IP_address]`: Releases a specific DHCP lease associated with the specified IP address

`debug ip dhcp server events`: Provides real-time updates to the DHCP server database, showing events like address assignments, renewals, and releases

`debug ip dhcp server packet`: Captures and displays detailed information about DHCP packets exchanged between the server and clients

`ipv6 address autoconfig`: Enables SLAAC on a router interface, allowing it to automatically configure its IPv6 address

`show ipv6 interface`: Displays information about an IPv6-enabled interface, including its address, prefix, and default gateway

`ipv6 unicast-routing`: Enables IPv6 unicast routing on the router

`ipv6 nd managed-config-flag`: Configures the RA to inform the client to contact a DHCPv6 server for all IPv6 addressing information

`ipv6 dhcp server`: Enters DHCPv6 server configuration mode

`ipv6 nd other-config-flag`: Configures the RA to signal the client to obtain non-addressing information (like DNS server address) from a DHCPv6 server

`ipv6 dhcp relay destination [IPv6_address]`: Configures a router interface as a DHCPv6 relay agent, forwarding SOLICIT messages to a DHCPv6 server at the specified IPv6 address

## FAQ

### Easy

<details><summary><b>What does DHCP stand for?</b></summary>DHCP stands for Dynamic Host Configuration Protocol.</details>

<details><summary><b>What is the purpose of a DHCP relay agent?</b></summary>A DHCP relay agent forwards DHCP broadcasts from clients to servers located on different networks. Since broadcasts cannot cross router boundaries, a relay agent is needed for clients to obtain IP information from a server on a different subnet.</details>

<details><summary><b>What command enables the DHCP service on a Cisco router?</b></summary>The command <code>service dhcp</code> enables the DHCP service on a Cisco router. However, this is usually not required as the service is enabled by default.
</details>

<details><summary><b>How does a client indicate that it wants to use the address offered by a DHCP server?</b></summary>The client sends a <b>broadcast DHCPREQUEST</b> message to the server, indicating it will use the offered address and wants to lease it.
</details>

### Medium

<details><summary><b></b></summary></details>

<details><summary><b></b></summary></details>

<details><summary><b></b></summary></details>

### Hard

<details><summary><b>Explain the concept of SLAAC and its limitations. How do stateful and stateless DHCPv6 address these limitations?
</b></summary>SLAAC (Stateless Address Autoconfiguration) allows devices to automatically generate their IPv6 address, prefix, and default gateway information from Router Advertisements (RAs) sent by routers.
<br>
While this offers simplicity, its limitations include:
<br>
<li>Limited Configuration Information: SLAAC only provides basic addressing information. Devices might need additional parameters like DNS server addresses, NTP server information, or domain names, which SLAAC cannot provide.</li>
<li>Lack of Centralized Management: SLAAC lacks the centralized control provided by DHCP, making it difficult to track and manage address assignments across a network.</li>
<br>
Stateful DHCPv6 overcomes these limitations by providing a centralized DHCPv6 server that assigns addresses and delivers other configuration parameters. Clients receive all necessary information from the server, ensuring comprehensive configuration and management.
<br>
Stateless DHCPv6 combines the benefits of both approaches. Devices still utilize SLAAC for address assignment but obtain additional configuration parameters from a DHCPv6 server, providing both autoconfiguration and comprehensive configuration options.
</details>

<details><summary><b></b></summary></details>

<details><summary><b></b></summary></details>

<details><summary><b></b></summary></details>

## Links
### Network Technologies

- [IPv6](https://notes.ryancranie.com/Notes/Network%20Technologies/IPv6)


### Revision History
001: 2024-09-24 - Initialized DHCP.md

---
<b>[Network Technologies Contents](https://notes.ryancranie.com/Contents/Network%20Technologies%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)