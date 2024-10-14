# SASE

<audio controls>
    <source src="https://github.com/ryancranie/notes/raw/refs/heads/main/Attachments/Audio/SASE.mp3" type="audio/mpeg">
    Your browser does not support the audio tag.
</audio>
↑ AI-Generated Audio Overview via @<a href="https://notebooklm.google/">NotebookLM</a>

Secure Access Service Edge (**SASE**) is a technology that combines Network as a Service with Security-as-a-Service capabilities. SASE is delivered through the cloud as-a-service consumption model, to support secure access for todays distributed/hybrid enterprise networks.

SASE was introduced because users began to access information from a variety of locations and devices, which
- expands attack surface
- introduces multi-level compliance requirements
- sophisticated cyber-threats

<details><summary><b>Why not use a VPN?</b></summary>VPNs are unable to support zero-trust access policy enforcement present at the corporate on premise network.</details>

## SASE Capabilities
A SASE solution provides integrated networking and security capabilities including:
- Peering
	- allows network connection and traffic exchange directly across the internet without having to pay a third party
- A Nex-Generation Firewall (**NGFW**) or Firewall-as-a-Service (**FWaaS**)
	- IPS
	- Anti-[Malware](https://notes.ryancranie.com/Notes/Threat%20Landscape/Malware)
	- SSL Inspection
	- Sandbox
- A Secure Web Gateway
	- filters malware
	- enforces policies
- Zero Trust Network Access (**ZTNA**)
	- ensures no user or device is automatically trusted.
		- MFA
		- Network Access Control (**NAC**)
		- Access policy enforcement
- Data Loss Prevention (**DLP**)
	- prevents end-users from moving key information outside the network.
- DNS
	- provides SASE with threat detection capabilities

The three core capabilities of SASE are 
- zero trust network access
- NGFW
- DLP

## Links
### Revision History
001: 2024-09-26 - Initialized SASE.md

---
<b>[Network Security Contents](https://notes.ryancranie.com/Contents/Network%20Security%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)