# Packet Forwarding

<audio controls>
    <source src="https://github.com/ryancranie/notes/raw/refs/heads/main/Attachments/Audio/Packet Forwarding.mp3" type="audio/mpeg">
    Your browser does not support the audio tag.
</audio>
↑ AI-Generated Audio Overview via @<a href="https://notebooklm.google/">NotebookLM</a>

## The Layer 3 Packet Forwarding Process

If you are experiencing connectivity issues between two hosts on a network, you could check Layer 3 by **pinging** between the hosts. 

If the pings are successful: 
- the issue resides at upper layers of the OSI reference model (Layers 4 through 7).

If the pings fail: 
- you should troubleshoot Layers 1 through 3. 

If you determine the problem is at Layer 3, you might look at the **packet-forwarding process** of a router. Review the Layer 3 Packet-Forwarding Process consider Figure 1-10. In this topology, PC1 needs to access HTTP resources on Server1. 

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241009190233.png" width="700">

Notice that PC1 and Server1 are on **different networks**.

### Step 1. PC1 concludes that the destination IP address **resides** on a **remote subnet**.
- Therefore, PC1 needs to send the **frame** to its **default gateway**.
- PC1's default gateway address is `192.168.1.1`, which is router R1.
- To construct a Layer 2 frame
	- PC1 needs the **MAC address** of the **frame’s destination**
	- which is PC1’s **default gateway** (**R1**).
	- If the MAC address is not in PC1’s Address Resolution Protocol (**ARP**) cache
		- PC1 uses *ARP* to discover it.
		- Once PC1 receives an ARP reply from router R1.
			- PC1 adds router R1’s MAC address to its ARP cache. 
	- PC1 then sends its data destined for **Server1** in a frame addressed to **R1**, as shown in Figure 1-11.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241009191237.png" width="700">

### Step 2. R1 **receives the frame** sent from PC1
- And because the destination MAC address is R1’s:
	- R1 tears off the **Layer 2 header** and *interrogates* the **Layer 3 header**.
		- Router R1 decrements the packet’s **TTL** field
		- If the value in the TTL field is reduced to zero:
			- the router **discards the packet** and sends a **time-exceeded ICMP message** back to the **source**.
		- Assuming the TTL is **not** decremented to zero
			- R1 checks its **routing table** to determine the **best path** to reach the IP address `192.168.3.2`.
			- R1’s **routing table** has an entry stating that network `192.168.3.0/24` is accessible through interface `Serial 1/1`.
				- ARP is not required for **serial interfaces** because they do **not have MAC addresses**.
			- **R1** forwards the frame out its `Serial 1/1` interface
				- using the Point-to-Point Protocol (**PPP**) Layer 2 **framing header**

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241010131154.png" width="700">

### Step 3. R2 receives the frame
- it removes the **PPP header** 
- then **decrements the TTL** in the IP header, just as router **R1** did.
- Again, assuming that the TTL did not get decremented to zero
	- router R2 *interrogates* the IP header to determine the **destination network**
- In this case, the destination network `192.168.3.0/24` is directly attached to router **R2**’s `Fast Ethernet 0/0` interface.
- R2 sends an **ARP request** to determine the **MAC address** of Server1
	- (if it is not already known in the **ARP cache**)
- Once an ARP reply is received from Server1
	- router R2 stores the results of the **ARP reply** in the **ARP cache**
	- and forwards the frame out its `Fast Ethernet 0/0` interface to Server1, as shown in Figure 1-13.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241010131234.png" width="700">

### IP Routing Table
When a router needs to route an IP packet 
- it consults its IP routing table to find the best match. 
	- The best match is the route that has the **longest prefix**. 

For example, suppose a router has a routing entry for networks 
1. `10.0.0.0/8`
2. `10.1.1.0/24` 
3. `10.1.1.0/26`

Also, suppose that the router is trying to *forward* a packet with the **destination IP address** `10.1.1.10`. 

The router selects the `10.1.1.0/26` route entry as the best match 
- because that route entry has the **longest prefix**, */26* 
	- (**it matches the most bits**).

### Layer 3-to-Layer 2 mapping table

In Figure 1-13 (Step 3), R2’s **ARP cache** contains *Layer 3-to-Layer 2 mapping information*. 

The **ARP cache** has a mapping that says MAC address `2222.2222.2222`, which corresponds to IP address `192.168.3.2`. 
An **ARP cache** is the Layer 3-to-Layer 2 mapping data structure used for Ethernet networks, but similar data structures are used for 
- Multipoint Frame Relay networks 
- Dynamic Multipoint Virtual Private Network (**DMVPN**). 

For **PPP** or **HDLC** networks
- there is **only one other possible device** connected to the other end of the link 
	- so **no mapping information** is needed to determine the *next-hop device*.

Querying a router’s routing table and its ARP cache is **less than efficient**. 
- Fortunately, Cisco Express Forwarding (**CEF**) gleans its information from the 
	- router’s IP routing table 
	- and **ARP cache**. 
- Then, **CEF**’s data structures in hardware can be *referenced* when **forwarding packets**.

#### The 2 Primary CEF Data Structures

##### Forwarding Information Base (FIB)

The FIB contains Layer 3 information 
- similar to the information found in an IP routing table. 

In addition, an FIB contains information about 
- **multicast routes** 
- **directly connected hosts**

##### Adjacency Table

When a router performs a route lookup using CEF, 
- the FIB *references an entry* in the **adjacency table**. 

The adjacency table entry contains **frame header information** *required* by the router to properly form a **frame**. 

An **egress interface** and a **next-hop MAC address** is in an *adjacency entry* for a multipoint Ethernet interface, whereas a point-to-point interface requires only egress interface information.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241010143447.png" width="700">

## Troubleshooting the Packet Forwarding Process

When troubleshooting packet-forwarding issues: 
- you need to *examine* a router’s **IP routing table**. 
- If the observed behaviour of the traffic is **not conforming** to information in the IP routing table, 
	- remember that the IP routing table is maintained by a **router’s control plane** and is used to build the tables at the **data plane**. 
	- **CEF is operating in the data plane** and uses the **FIB**. 
	- You need to **view the CEF data structures** that contain all the information required to make packet-forwarding decisions.
		- (that is, the FIB and the adjacency table)  

Example 1-25 provides sample output from the `show ip route [ip_address]` command. 
- The output shows that the **next-hop IP address** to reach IP address `192.168.1.11` is `192.168.0.11`, which is accessible via interface `Fast Ethernet 0/0`. 
- Because this information is coming from the **control plane**,
	- it includes information about the **routing protocol OSPF**

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241010144022.png" width="700">

Example 1-28 provides sample output from the `show ip cef [ip_address]` command. The output indicates that, according to CEF, 
- IP address `192.168.1.11` is accessible out interface `FastEthernet 0/0`, 
	- with the **next-hop** IP address `192.168.0.11`

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241010144205.png" width="700">

The following snippet provides sample output from the `show ip cef exact-route [source_address] [destination_address]` command.

```
Router# show ip cef exact-route 10.2.2.2 192.168.1.11 
10.2.2.2 -> 192.168.1.11 : FastEthernet0/0 (next hop 192.168.0.11)
```

The output indicates that a packet **sourced** from IP address `10.2.2.2` and destined for IP address `192.168.1.11` will be sent out interface `FastEthernet 0/0` to **next-hop** IP address `192.168.0.11`.

For a **multipoint interface** such as 
- point-to-multipoint Frame Relay 
- Ethernet

when a router knows the next-hop address for a packet 
- it needs appropriate Layer 2 information 
	- for example, next-hop MAC address or data link connection identifier [DLCI] to properly construct a frame.

Example 1-30 provides sample output from the `show ip arp` command, which displays the **ARP cache** that is stored in the **control plane** on a router. The output shows the 
- learned or configured MAC addresses 
- along with their associated IP addresses.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241011092116.png" width="650">

Example 1-33 provides sample output from the `show adjacency detail` command.

The output shows the CEF information used to **construct frame headers** needed to reach the next-hop IP addresses through the various router interfaces.

Notice the value **64510800** for `Serial 1/0`. This is a 
- *hexadecimal representation* of 
- information that is needed by the router to 
- successfully forward the packet to 
- the next-hop IP address `172.16.33.5`, 
	- including the **DLCI 405**. 

Notice the Ethernet frame value **CA1B01C4001CCA1C164000540800** for `Fast Ethernet 3/0`. This is the 
1. **destination MAC address** - The first 12 hex values are the destination MAC address 
2. the **source MAC address** - the next 12 are the source MAC address
3. and the **EtherType** code - 0800 is the IPv4 EtherType code.

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241011092615.png" width="700">

## Routing Information Sources

This section explains which sources of routing information are the **most believable** and how the routing table *interacts* with various **data structures** to **populate itself** with the best information.

### Data Structures and the Routing Table

As a router receives routing information from a neighboring router, 
- the information is stored in the **data structures** of the IP routing protocol
- and **analyzed** by the routing [protocol](https://notes.ryancranie.com/Notes/Network%20Technologies/Protocols)
- to **determine the best path**, based on *metrics*.

An IP routing protocol’s data structure can also be **populated by the local router**. For example, a router might be configured for **route redistribution**, where routing information is redistributed from the **routing table** into the IP routing protocol’s data structure. 

Specific **interfaces** can also participate in the IP routing protocol process and 
- the **network** that the interface *belongs* to 
	- is placed into the **routing protocol data structure** as well. 

Review Figure 1-15, 
1. the **routing protocol data structure** can populate the routing table, 
2. a **directly connected route** can populate the routing table
3. and **static routes** can populate the routing table. 

These are all known as **sources of routing information**

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020241011093302.png" width="650">

### Sources of Routing Information

Routing information sources are each assigned an **administrative distance** (**AD**). Administrative distance is the *believability* or *trustworthiness* of a **routing source**, when comparing it to the other routing information sources

#### Default Administrative Distance of Routing Sources

| Source of Routing Information                          | AD  |
| ------------------------------------------------------ | --- |
| **Connected Interface**                                | 0   |
| **Static Route**                                       | 1   |
| **[EIGRP](https://notes.ryancranie.com/Notes/Network%20Technologies/EIGRP) summary route**                                | 5   |
| **eBGP** (External Border Gateway Protocol)            | 20  |
| (**Internal**) **EIGRP**                               | 90  |
| **OSPF**                                               | 110 |
| **IS-IS** (Intermediate System to Intermediate System) | 115 |
| **RIP**                                                | 120 |
| **ODR** (On-Demand Routing)                            | 160 |
| (**External**) **EIGRP**                               | 170 |
| **iBGP** (Internal Border Gateway Protocol)            | 200 |
| **Unknown** (not believable)                           | 255 |

**NOTE:** The **lower** the AD, the more **trustworthy**.

Routes are injected into the routing table only if **the router concludes** that they came from **the best routing source**. 

If you ever need to make sure that the routing information or subset of routing information received from a particular source is never used: 
1. **change the AD** of specific routes or all routes from that source to **255** 
	- which means “do not believe.” 
2. Another option is to **create a floating static route** 
	- which is a backup route configured to have a **higher AD** 
		- and therefore be less preferred, than the route that is preferred.

## Glossary

### Layer 2 Terminology
<details><summary><b>ARP (Address Resolution Protocol)</b></summary>is a protocol used to discover the MAC address associated with an IP address.<br><br></details>
<details><summary><b>ARP Cache</b></summary>is a table that stores mappings between IP addresses and MAC addresses on a network.<br><br></details>
<details><summary><b>DMVPN (Dynamic Multipoint Virtual Private Network)</b></summary>is a type of VPN that uses a dynamic mesh of tunnels between endpoints.<br><br></details>
<details><summary><b>Ethernet</b></summary>is a family of networking technologies commonly used in local area networks (LANs).<br><br></details>
<details><summary><b>Frame</b></summary>is a data unit used at Layer 2 of the OSI model, encapsulating data from higher layers.<br><br></details>
<details><summary><b>HDLC (High-Level Data Link Control)</b></summary>is a bit-oriented synchronous data link layer protocol.<br><br></details>
<details><summary><b>MAC Address</b></summary>is a unique hardware identifier assigned to a network interface card (NIC).<br><br></details>
<details><summary><b>Multipoint Frame Relay</b></summary>is a type of Frame Relay network that supports multiple endpoints on a single physical circuit.<br><br></details>
<details><summary><b>PPP (Point-to-Point Protocol)</b></summary>is a data link layer protocol used to establish a direct connection between two networking nodes.<br><br></details>

### Layer 3 Terminology 
<details><summary><b>CEF (Cisco Express Forwarding)</b></summary>is a high-performance packet forwarding mechanism used in Cisco routers.<br><br></details>
<details><summary><b>Control Plane</b></summary>is the part of a router that manages routing protocols and maintains the routing table.<br><br></details>
<details><summary><b>Data Plane</b></summary>is the part of a router that forwards packets based on the information in the routing table.<br><br></details>
<details><summary><b>Default Gateway</b></summary>is the router that a device uses to send traffic to destinations outside its local network.<br><br></details>
<details><summary><b>Destination IP Address</b></summary>is the IP address of the intended recipient of a packet.<br><br></details>
<details><summary><b>Egress Interface</b></summary>is the interface on a router through which a packet exits the router on its way to its destination.<br><br></details>
<details><summary><b>FIB (Forwarding Information Base)</b></summary>is a data structure used by CEF that contains Layer 3 routing information.<br><br></details>
<details><summary><b>ICMP (Internet Control Message Protocol)</b></summary>is a network protocol used for error reporting and diagnostics.<br><br></details>
<details><summary><b>IP Routing Table</b></summary>is a data structure used by a router to determine the best path for forwarding packets.<br><br></details>
<details><summary><b>Longest Prefix</b></summary>refers to the routing table entry that matches the most bits of a destination IP address, used to select the most specific route.<br><br></details>
<details><summary><b>Next-Hop IP Address</b></summary>is the IP address of the next router in the path to a packet's destination.<br><br></details>
<details><summary><b>OSPF (Open Shortest Path First)</b></summary>is a link-state routing protocol commonly used in IP networks.<br><br></details>
<details><summary><b>Packet-Forwarding Process</b></summary>is the series of steps that a router takes to forward a packet from its source to its destination.<br><br></details>
<details><summary><b>Pinging</b></summary>is the process of sending ICMP echo requests to test the reachability of a host on a network.<br><br></details>
<details><summary><b>Remote Subnet</b></summary>is a subnet that is different from the subnet that a device is currently on.<br><br></details>
<details><summary><b>Route Redistribution</b></summary>is the process of exchanging routing information between different routing protocols.<br><br></details>
<details><summary><b>Routing Protocol Data Structure</b></summary>is a data structure used by a routing protocol to store and manage routing information.<br><br></details>
<details><summary><b>Serial Interface</b></summary>is a type of interface used for point-to-point communication over a serial link.<br><br></details>
<details><summary><b>Source</b></summary>is the origin of a packet or data transmission.<br><br></details>
<details><summary><b>Static Route</b></summary>is a route that is manually configured on a router, rather than learned dynamically through a routing protocol.<br><br></details>
<details><summary><b>TTL (Time to Live)</b></summary>is a field in an IP packet header that limits the number of hops a packet can take before being discarded.<br><br></details>

### Routing Protocol Terminology
<details><summary><b>Administrative Distance (AD)</b></summary>is a measure of the trustworthiness of a routing source, used to determine which route to prefer when multiple routes exist to the same destination.<br><br></details>
<details><summary><b>eBGP (External Border Gateway Protocol)</b></summary>is a version of BGP used to exchange routing information between different autonomous systems.<br><br></details>
<details><summary><b>EIGRP (Enhanced Interior Gateway Routing Protocol)</b></summary>is a distance-vector routing protocol developed by Cisco Systems.<br><br></details>
<details><summary><b>Floating Static Route</b></summary>is a backup route configured with a higher AD, making it less preferred than the primary route.<br><br></details>
<details><summary><b>iBGP (Internal Border Gateway Protocol)</b></summary>is a version of BGP used to exchange routing information within an autonomous system.<br><br></details>
<details><summary><b>IS-IS (Intermediate System to Intermediate System)</b></summary>is a link-state routing protocol used in large-scale IP networks.<br><br></details>
<details><summary><b>ODR (On-Demand Routing)</b></summary>is a routing technique that creates routes only when they are needed.<br><br></details>
<details><summary><b>RIP (Routing Information Protocol)</b></summary>is a distance-vector routing protocol commonly used in small-scale IP networks.<br><br></details>

### Miscellaneous Terminology 
<details><summary><b>Data Link Connection Identifier (DLCI)</b></summary>is a number used to identify a virtual circuit in a Frame Relay network.<br><br></details>
<details><summary><b>EtherType</b></summary>is a two-byte field in an Ethernet frame that identifies the protocol encapsulated in the frame payload.<br><br></details>
<details><summary><b>Hexadecimal Representation</b></summary>is a base-16 numbering system used to represent data in a compact form.<br><br></details>
<details><summary><b>Multicast Route</b></summary>is a route used to send traffic to a group of devices on a network.<br><br></details>

## Commands

### Packet Forwarding Commands

<details><summary><code>ping</code></summary>Used to test network connectivity between two devices.<br><br></details>
<details><summary><code>show ip route [ip_address]</code></summary>Displays the IP routing table and provides information on how to reach a specific IP address.<br><br></details>
<details><summary><code>show ip cef [ip_address]</code></summary>Shows the Cisco Express Forwarding (CEF) information for a particular IP address.<br><br></details>
<details><summary><code>show ip cef exact-route [source_address] [destination_address]</code></summary>Displays the CEF information for a specific route, given a source and destination IP address.<br><br></details>
<details><summary><code>show ip arp</code></summary>Shows the ARP cache, which contains mappings between IP addresses and MAC addresses.<br><br></details>
<details><summary><code>show adjacency detail</code></summary>Displays detailed information about CEF adjacencies, including Layer 2 framing information needed to reach next-hop IP addresses.<br><br></details>

## QnA
<details><summary><b>What is the purpose of pinging between two hosts when troubleshooting network connectivity?</b></summary>Pinging helps determine whether the connectivity issue lies at <b>Layer 3</b> (network layer) of the OSI model or at higher layers.<br><br></details>
<details><summary><b>What is the first step PC1 takes when it needs to send data to a device on a different network?</b></summary>PC1 first determines that the destination <b>IP address</b> is located on a <b>remote subnet</b>.<br><br></details>
<details><summary><b>Why doesn't a serial interface require ARP?</b></summary>Serial interfaces don't have <b>MAC addresses</b>, so ARP, which is used to resolve <code>IP addresses</code> to <b>MAC addresses</b>, is unnecessary.<br><br></details>
<details><summary><b>What is the administrative distance of a directly connected interface?</b></summary>A directly connected interface has an administrative distance of <b>0</b>, making it the most trustworthy source of routing information.<br><br></details>
<details><summary><b>Explain the process a router uses to forward a packet to a device on a remote network.</b></summary>When a router receives a packet, it first decrements the <code>TTL</code> field. If the <code>TTL</code> reaches zero, the packet is discarded, and an <code>ICMP</code> message is sent to the source. If the <code>TTL</code> is not zero, the router consults its routing table to determine the best path to the destination <code>IP address</code>. This process involves identifying the <b>egress interface</b> and the <b>next-hop IP address</b>. If the next hop is on a multipoint network like <b>Ethernet</b>, the router uses <code>ARP</code> to determine the <b>MAC address</b> of the next-hop device. The packet is then encapsulated in a new frame with the appropriate Layer 2 header and forwarded out the correct interface.<br><br></details>
<details><summary><b>Describe the function of Cisco Express Forwarding (CEF) and its two primary data structures.</b></summary>CEF is a mechanism used by Cisco routers to optimize packet forwarding. It achieves this by using pre-built data structures derived from the routing table and ARP cache. The two primary data structures are: <ul><li><b>Forwarding Information Base (FIB):</b> The FIB contains Layer 3 routing information, including multicast routes and information about directly connected hosts. It's similar to the <code>IP routing table</code> but optimized for forwarding.</li><li><b>Adjacency Table:</b> This table stores the Layer 2 information needed to forward packets to next-hop devices. It's referenced by the FIB and includes details such as the <b>egress interface</b> and the next-hop <b>MAC address</b> (for multipoint interfaces).</li></ul><br></details>
<details><summary><b>How does a router prioritize different sources of routing information, and how can an administrator influence this prioritization?</b></summary>Routers use a concept called <b>administrative distance (AD)</b> to rank the reliability of different routing information sources. A lower AD value indicates a more trustworthy source. For example, directly connected interfaces have an AD of <b>0</b>, static routes have an AD of <b>1</b>, while routing protocols like <code>OSPF</code> and <code>RIP</code> have higher AD values. When multiple routes to the same destination exist, the router selects the route with the lowest AD.<br><br>Administrators can manipulate route selection by adjusting the AD. For instance, they can configure a floating static route with a higher AD as a backup path in case the preferred route becomes unavailable. Setting an AD of <b>255</b> effectively discards routes from a specific source. This allows administrators to control the routing behavior and ensure that traffic follows desired paths.<br><br></details>

## Links
### Revision History
001: 2024-10-11 - Initialized Packet Forwarding.md

---
<b>[Network Technologies Contents](https://notes.ryancranie.com/Contents/Network%20Technologies%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)