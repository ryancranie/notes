# Network Segmentation

Network segmentation divides a network into smaller, isolated segments. This allows the IT managers a better time managing/overseeing everything going on.

If one subnetwork becomes compromised, other subnetworks would not be effected.
## Demilitarized Zone (DMZ)
A company may need internet access to some servers on their network, but wouldn't want to expose unnecessary devices. In this case, the servers would be placed into a Demilitarized Zone (**DMZ**), separated from the rest of the internal network. 

The DMZ can be separated even further through micro-segmentation
- each resource is uniquely identified and protected via Zero Trust.
	- hosts
	- users
	- applications
	- etc.

Traffic going between a DMZ and the internet can be classified as **North-south traffic**

Traffic going between a data center or a cloud network is called **East-west traffic**
## Where to Segment a Network
A network can be segmented into physical or logical segments.

### Physical
- Network Layer (Layer 3)
	- This approach is limited.
	- Firewall policies
	- ACLs
	- Routers
### Logical
- Data Link Layer (Layer 2)
	- Virtual Local Area Networks (**VLANs**)
		- Provide the way for groups of devices to communicate through switches

### SD-WAN
- Application Layer (Layer 7)
	- [SD-WAN](https://notes.ryancranie.com/Notes/Secure%20Network/SD-WAN) allows communication over the internet using encrypted tunnels.
		- creates overlay network

The **underlay** network refers to the physical network infrastructure
The **overlay** network refers to the virtualized network built over the underlay network
### Jump Box
One way to securely access segmented networks is through a **Jump Box**
- enhanced access control
- limited authorization
- acts as a proxy to the devices in the internal segment
- has additional monitoring and logging to alert the IT manager if it has been compromised.

### Bastion Host
Another way to access segmented networks is via a **Bastion Host**
- a server/computer whos purpose is to provide access to a private network from an external network
- configured to withstand attacks
	- allows users through subnets via 1 application
		- small attack surface
- an example of this is a secured HTML connection.
	- bastion host provides secure SSH and RDP connection to the internal network segment.

## Benefits of network segmentation
- Network management config is made easier
- Network broadcasts are reduced
- Network congestion is minimized
- Attacks are limited to a specific segment
- Vulnerable devices are given greater protection
- Compliance affects a smaller scope.
## Links
### Revision History
001: 2024-09-26 - INITIALIZE

---
<font size=3><b>[SECURE NETWORK CONTENTS](https://github.com/ryancranie/cybersecurity-osint/blob/main/Contents/-%20Secure%20Network%20Contents.md)<br>
[README](https://github.com/ryancranie/cybersecurity-osint/blob/main/README.md)<br>
[LICENSE](https://github.com/ryancranie/cybersecurity-osint/blob/main/LICENSE)</b></font>