# Zero Trust Principles

Idea behind Zero Trust Model is that trust must be explicitly derived from a mix of identity-based and context-based aspects
1. **Never trust, always verify**
	- Authentication
	- Authorization
2. **Implement Least Privilege**
3. **Assume the network is already breached**
	- Reduce the attack surface

## Trojan Horse Myth
1. A large wooden horse was rolled into the fortified city of Troy
2. Unbeknownst to the people of Troy, many Greek soldiers were concealed inside the Trojan Horse
3. After the horse was in the city, the attack was unstoppable

New Age business requires more decentralization, which largens the attack surface.
- On-premises
- Public Cloud
- Private Cloud
- Remote Work

## The Never Trust Principle
Trust is explicitly derived from a mix of identity and context-based aspects.
1. Identification Process
	- **Multifactor authentication**
		- MFA = presenting two or more pieces of evidence/factors for authentication.
2. Context-based aspects
	- **Time and date**
		- can restrict access to business hours, or require a One-Time Password (**OTP**) outside of business times.
	- **Geolocation**
	- **Security posture**
		- example: restrictions are placed on a device that doesn't meet security requirements, such as updated patches and anti-virus software.

## Applying the Principle of Least Privilege
- **Privileged Access Management (PAM)**
	- PAM is an information security mechanism that safeguards identities with special access or capabilities beyond regular users. 
- **Protect Surface**
	- Defining the protect surface is a process of identifying network resources, assessing the degree of confidentiality, and determining which roles require access to them.
- **Kipling Method**
	- The Kipling method is the process of asking yourself a series of questions, such as who, what, when, where, why, and how when preparing to solve a problem.

## The Assume Breach Principle
- Prepare contingencies to prepare for the worst
	- Can be applied immediately after a breach
- Micro-segmentation
	- Restricts network flow for contagions.

## Other Zero Trust Methods
- Zero Trust Access (**ZTA**)
	- secure access method that supports zero trust security
	- uses role-based access control
		- Employee
		- Guest
		- Contractor
		- HR manager
		- IT analyst
		- Account manager
	- Endpoint agent for greater visibility and control of computer devices
		- Operating system
		- Patch level
		- Installed software
		- Assess risk level
	- Network Access Control (**NAC**) for headless devices, such as IoT
		- Identifies devices
		- More visibility
	- Zero Trust Network Access (**ZTNA**)
		- establishes secure connection automatically
			- regardless of location
		- ensures granular control over access to applications
		- enforces zero trust

### Benefits of Zero Trust
- **No Trust**
	- Trust must be proven through
		- MFA
		- Risk Measurement through a context-based assessment
- **Least privilege**
- **Review access to assets by asking what, why, when, where and how.**
- **Micro-segmentation**
## Links
### Revision History
001: 2024-09-26 - Initialized Zero Trust Principles.md

---
<b>[Network Security Contents](https://notes.ryancranie.com/Contents/Network%20Security%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)