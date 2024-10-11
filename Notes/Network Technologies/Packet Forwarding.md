# Packet Forwarding

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



## Links
### Revision History
001: 2024-10-11 - Initialized Packet Forwarding.md

---
<b>[Network Technologies Contents](https://notes.ryancranie.com/Contents/Network%20Technologies%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)