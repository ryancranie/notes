# IPv6

> "7 addresses to every atom of your body, for every person in the world = Number of IPv6 Addresses."

## Features of IPv6

### Improved Features
1. **Large address space** 
	- IPv6 addresses are 128 bits, compared to IPv4’s 32 bits. • 
2. **Elimination of public-to-private NAT** 
	- End-to-end communication traceability is possible. 
3. **Elimination of broadcast addresses**
	- IPv6 now includes *unicast*, *multicast*, and *anycast* addresses. 
4. **Support for mobility and security** 
	- Helps ensure compliance with mobile IP and IPsec standards. 
5. **Simplified header for improved router efficiency**

### New Features
6. **Prefix renumbering** 
	- IPv6 allows simplified mechanisms for address and prefix renumbering. 
7. **Multiple addresses per interface**
	- An IPv6 interface can have multiple addresses. 
8. **Link-local addresses** 
	- IPv6 link-local addresses are used as the next hop when IGPs are exchanging routing updates. 
9. **Stateless autoconfiguration** 
	- [DHCP](https://notes.ryancranie.com/Notes/Network%20Technologies/DHCP) is not required because an IPv6 device can automatically assign itself a unique IPv6 link-local address.

## IPv6 Address Types

| Address Type | Description                                                                                                                                                                                                                                                   | Topology                                  |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| Unicast      | *"One to One"*<br>- An address destined for a **single interface**.<br>- A packet sent to a unicast address is delivered to the interface identified by that address.                                                                                         | ![[Pasted image 20240928105915.png\|200]] |
| Multicast    | *"One to Many"*<br>- An address for a set of interfaces (typically belonging to different nodes).<br>- A packet sent to a multicast address will be delivered to all interfaces identified by that address.                                                   | ![[Pasted image 20240928105940.png\|200]] |
| Anycast      | *One to Nearest* (**Allocated from Unicast**)<br>- An address for a set of interfaces<br>- In most cases these interfaces belong to different nodes.<br>- A packet sent to an anycast address is delivered to the closest interface as determined by the IGP. | ![[Pasted image 20240928110031.png\|200]] |

## Is IPv4 Obsolete?
- IPv4 is in no danger of disappering overnight.
	- It will coexist with IPv6 then gradually be replaced
- IPv6 provides many transition options including:
	1. **Dual stack**
		- Both IPv4 and IPv6 are configured and run simultaneously on the interface
	2. **IPv6-to-IPv4 (6to4)**
		- tunnelling and IPv4-compatible tunneling
	3. **NAT [protocol](https://notes.ryancranie.com/Notes/Network%20Technologies/Protocols) translation (NAT-PT)**
		- between IPv6 and IPv4

## IPv6 Address Specifics
The **128-bit** IPv6 address is written using hexadecimal numbers. 
- Specifically, it consists of **8, 16-bit segments** separated with colons between each set of four hex digits (**16 bits**).
	- Referred to as “**coloned hex**” format. 
	- Hex digits are **not case sensitive**. 
	- The format is `x:x:x:x:x:x:x:x`, where x is a **16-bit hexadecimal field** 
		- therefore each x is representing four hexadecimal digits

An example address is as follows: 
- `2035:0001:2BC5:0000:0000:087C:0000:000A`

## Abbreviating IPv6 Addresses
Leading `0`s within each set of four hexadecimal digits can be omitted.
- `09C0` = `9C0`
- `0000` = `0`
- A pair of colons "`::`" can be used, once within an address, to represent successive `0`s.

### IPv6 Address Examples
#### Example 1
1. `2031:0000:130F:0000:0000:09C0:876A:130B`
	- full IPv6 address
2. `2031:0:130F:0:0:9C0:876A:130B`
	- `0000` abbreviated to `0`
3. `2031:0:130F::9C0:876A:130B`
	- most consecutive `0`s shortened to `::`

#### Example 2
1. `FF01:0000:0000:0000:0000:0000:0000:1`
2. `FF01:0:0:0:0:0:0:1`
3. `FF01::1`

#### Example 3
1. `3FFE:0501:0008:0000:0260:97FF:FE40:EFAB`
2. `3FFE:501:8:0:260:97FF:FE40:EFAB`
3. `3FFE:501:8::260:97FF:FE40:EFAB`

## IPv6 in an Enterprise Network
An IPv6 address consists of 2 parts:
1. A **subnet prefix**
	- represents the network to which the interface is connected.
2. An **interface ID**
	- sometimes called a local identifier or token
	- usually 64 bits in length

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240928181102.png" width="700">

## Subnet Prefix
IPv6 uses the “*/prefix-length*” **CIDR notation** to denote how many bits in the IPv6 address represent the subnet.
- The syntax is `ipv6-address/prefix-length`
	- `ipv6 address` is the 128 bit IPv6 address
	- `/prefix-length` is a decimal value representing how many of the left most continuous bits of the address comprise the prefix.

### Example
`fec0:0:0:1::1234/64` 
- is really 
`fec0:0000:0000:0001:0000:0000:0000:1234/64` 

1. The first 64-bits (`fec0:0000:0000:0001`) forms the **address prefix**. 
2. The last 64-bits (`0000:0000:0000:1234`) forms the **Interface ID**.

## Special IPv6 Addresses

| IPv6 Address        | Description                                                                                                                |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `::/0`              | - All routes and used when specifying a default static route.<br>- It is equivalent to the IPv4 **quad-zero** (`0.0.0.0`). |
| `::/128`            | - Unspecified address and is initially assigned to a host when it first resolves its local link address                    |
| `::1/128`           | - Loopback address of local host.<br>- Equivalent to `127.0.0.1` in IPv4                                                   |
| `FE80::/10`         | - Link-local unicast address.<br>- Similar to the Windows autoconfiguration IP address of `169.254.x.x`                    |
| `FF00::/8`          | - Multicast addresses.                                                                                                     |
| All other addresses | - Global unicast address.                                                                                                  |

## IPv6 Address Scopes
Address types have well defined scopes:
- Link-local address
- Global unicast address
- Site-local address

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240928182251.png" width="500">

**NOTE: Site-Local Addresses are deprecated in RFC 3879.**

## Multiple IP addresses
- An interface can have multiple IPv6 addresses simultaneously configured and enabled on it.
	- However, it must have a **link-local** address.
- Typically, an interface is assigned a link-local and one (or more) global IPv6 address
	- For example, an ethernet interface can have
		1. **Link-local** address
			- e.g. `FE80::21B:D5FF:FE5B:A408`
		2. **Global unicast** address
			- e.g. `2001:8:85A3:4289:21B:D5FF:FE5B:A408`
- **NOTE**: An interface could also be configured to simultaneously support IPv4 and IPv6 addresses.
	- This creates a “**dual-stacked**” interface which is discussed later

## IPv6 Link-Local Address Example

```
R1# show ipv6 interface loopback 100 
Loopback100 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::222:55FF:FE18:7DE8
  No Virtual link-local address(es): 
  Global unicast address(es): 
    2001:8:85A3:4290:222:55FF:FE18:7DE8, subnet is 2001:8:85A3:4290::/64 [EUI]
  Joined group address(es): 
    FF02::1 
    FF02::2 
    FF02::1:FF18:7DE8 
  MTU is 1514 bytes 
  ICMP error messages limited to one every 100 milliseconds
  ICMP redirects are enabled
  ICMP unreachables are sent ND DAD is not supported
  ND reachable time is 30000 milliseconds (using 31238)
  Hosts use stateless autoconfig for addresses. 
R1#
```

## Enable IPv6 Routing
- Enable the forwarding off IPv6 unicast diagrams

```
Router(config)#ipv6 unicast-routing
```

- Command is only required before configuring an IPv6 protocol
	- Command is not needed before configuring IPv6 interface addresses.
	- It is also required for the interface to provide stateless auto-configuration.
- Configuring `no ipv6 unicast-routing` disabled the IPv6 routing capabilities of the router and the router acts as an IPv6 end-station.

## Enable IPv6 on an Interface
- Enable the forwarding of IPv6 unicast datagrams. 

```
Router(config-if)# ipv6 address address/prefix-length [link-local | eui-64]
```

- Command is used to statically configure an IPv6 address and prefix on an interface.
	- This enables IPv6 processing on the interface. 
- The `link-local` parameter configures the address as the link-local address on the interface.
- The `eui-64` parameter completes a global IPv6 address using an EUI-64 format interface ID.

## Assigning a Link Local Address

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241001140148.png" width="400">

```
R1(config)# interface fa0/0 
R1(config-if)# ipv6 address FE80::1 ? 
link-local use link-local address 
R1(config-if)# ipv6 address FE80::1 link-local 
R1(config-if)# end
```

Link-local addresses are created
- Automatically using the EUI-64 format if the interface has IPv6 enabled on it or a global IPv6 address configured. 
- Manually configured interface ID. 
	- Manually configured interface IDs are easier to remember than EUI-64 generated IDs. 

NOTE: the prefix mask is **not required** on link-local addresses because they are not routed.

## Assigning a Static Global Unicast Address

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241001170909.png" width="400">

- Global Unicast IPv6 addresses are assigned by omitting the `link-local` parameter. 
- For example, IPv6 address `2001:1::1/64` is configured on R1’s Fast Ethernet 0/0.

```
R1(config)# ipv6 unicast-routing 
R1(config)# interface fa0/0 
R1(config-if)# ipv6 address 2001:1::1/64 
```

- Notice that the entire address is manually configured and that the EUI-64 format was not used

```
R1# show ipv6 interface fa0/1 

R1# config t 
R1(config)# int fa0/1 
R1(config-if)# ipv6 add 2001::/64 eui-64 
R1(config-if)# do show ipv6 interface fa0/1 
FastEthernet0/1 is administratively down, line protocol is down 
  IPv6 is enabled, link-local address is FE80::211:92FF:FE54:E2A1 [TEN] 
  Global unicast address(es): 
    2001::211:92FF:FE54:E2A1, subnet is 2001::/64 [EUI/TEN] 
  Joined group address(es): 
    FF02::1 
    FF02::2 
    FF02::1:FF54:E2A1 
  MTU is 1500 bytes 

<output omitted>
```

NOTE: by simply configuring a global unicast IPv6 address on an interface:
- also automatically generates a *link-local interface* (**EUI-64**)

## Assigning Multiple IPv6 Addresses

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241001171509.png" width="400">

- Interfaces can have **multiple IPv6 addresses** assigned to them. 
	- These addresses can be used *simultaneously*.

```
R1# show run interface fa0/0 
Building configuration... 
Current configuration : 162 bytes 
! 
interface FastEthernet0/0 
  ip address 10.10.10.1 255.255.255.0 
  duplex auto 
  speed auto 
  ipv6 address 2001:1::1/64 
  ipv6 address 2002:1::1/64 
  ipv6 address FE80::1 link-local 
  end 
R1#
```

## EUI-64 to IPv6 Interface Identifier

The EUI-64 standard explains how it inserts a 16-bit `0xFFFE` in the middle at the **24th bit of the MAC address** to create a unique 64-bit interface identifier.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241001171751.png" width="550">

## IPv6 Routing
IPv6 supports the following routing:
- Static Routing
- RIPng
- OSPFv3
- IS-IS for IPv6
- EIGRP for IPv6
- Multiprotocol BGP version 4 (MP-BGPv4)

For each routing option above, the `ipv6 unicast-routing` command must be configured

## Tunnelling Techniques

For IPv6, tunnelling is an integration method in which an IPv6 packet is encapsulated within IPv4.
- This enables the connection of IPv6 islands without the need to convert the intermediary network to IPv6.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241001171958.png" width="750">

## NAT-PT Example

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241001172041.png" width="750">

Node A is an IPv6 only node and wants to send an IPv6 datagram to node D and therefore forwards the packet to the NAT-PT router.
- The NAT-PT router maintains a pool of globally routable IPv4 addresses that are assigned to IPv6 nodes dynamically as sessions are initiated.

An advantage of NAT-PT is that no modifications are required on the hosts.

## Links
### Network Technologies

- [DHCP](https://notes.ryancranie.com/Notes/Network%20Technologies/DHCP)


### Revision History
001: 2024-09-27 - Initialized IPv6.md

---
<b>[Network Technologies Contents](https://notes.ryancranie.com/Contents/Network%20Technologies%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)