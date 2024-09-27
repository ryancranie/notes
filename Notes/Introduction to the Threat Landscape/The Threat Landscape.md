# The Threat Landscape

## Threat Actors

Bad actor: person tries to sabotage InfoSec
Bad actors can be grouped via character, motivation and attack methods

Types of bad actors:
- **Explorer**
	- least nefarious, just searches for exploits. Might change a page on a website to embarrass someone.
	- attacks via Phishing
- **Hacktivist**
	- activist against a certain cooperation or belief, political or social organization, anonymous.
	- attacks via DDoS
- **Cyberterrorist**
	- main motivation is just to destroy things. Such as operational technology. Operate like a virtual army.
	- attacks via Spear Phishing, DDoS and [social engineering](https://notes.ryancranie.com/Notes/Introduction%20to%20the%20Threat%20Landscape/Social%20Engineering)
- **Cybercriminal**
	- they want money
	- attacks via phishing, fraud or ransomware
- **Cyberwarrior**
	- least self-interested, have the resources of a lot. They fight for nations.
	- attacks via zero-day exploits

Categories of hackers:
- **White Hat**
	- ethical hacker
- **Black Hat**
	- attacks for profit/harm
- **Grey Hat**
	- Not malicious, but not always ethical
- **Blue Hat**
		- Ethical, hired third party

## Cybersecurity Threats
An attack vector is a method used by a bad actor to illegally access or inhibit a network, system, or facility.

Attack Vector:
1. Vulnerability
2. Mechanism
3. Pathway

Attack Vectors:
- **Electronic Social Engineering**
- **Physical Social Engineering**
- **Technical Vulnerabilities**

An attack path is the chain of events that occurs when attack vectors are exploited.

Cybersecurity Threat Categories
- **Social engineering**
	- phycological manipulation via gaining trust
- **[Malware](https://notes.ryancranie.com/Notes/Introduction%20to%20the%20Threat%20Landscape/Malware)**
	- software which is designed to damage a computer system.
- **Unauthorized access**
	- tailgating: going through a gate following an authorized user, unauthorized.
	- shoulder surfing: peeping for a password
- **System design failure**
	- security flaw in a computer system/application that a bad actor can exploit.

Attack Vector Categories
- **DoS and DDoS**
	- Command and control (C&C) server signals to thousands of infected computers to send requests to a target server.  
- **Spear Phishing**
	- Targeted phishing attack
- **Whale Phishing**
	- Phishing attack aimed at a high level target
	- Trojan Horse Virus is a type of malware that downloads onto a computer disguised as legitimate.
- **Ransomware**
	- Malware that encrypts data

Pre-exploit stage
Birthday Attack
- attacks weaknesses in some password [hashing](https://notes.ryancranie.com/Notes/Cryptography%20and%20the%20Public%20Key%20Infrastructure/Hashing%20and%20Digital%20Signatures) algorithms. The weakness is that the same input value produces the same output value.
Brute Force
- relies on individual having weaker password
Counters to these attacks:
- implementing a strong password policy

## Threat Intelligence

Threat-intelligence defines an existing or emerging menace/hazard to assets that can be used to inform decisions regarding the subject's response to that threat/hazard.

Threat Intelligence has 3 requisite characteristics
- **Relevant**
	- does the information impact your organization?
- **Actionable**
	- is there enough information to act upon?
- **Contextual**
	- is there enough information to assess the threat?

Where do you find threat intelligence?
- **Internally**
	- server logs
	- penetration test results
- **Externally**
	- government sites
		- dhs.gov
		- fbi.gov
		- nist.gov
- **Private**
	- sans.org
	- FortiGuard Labs
- **OSINT: MITRE ATT&CK**
	- Knowledge base for adversary tactics and techniques
- **Verticals**
	- Orgs that share threat intelligence information with one another.

Common Vulnerability Scoring System (**CVSS**)
- free and open industry standard for assessing computer system vulnerabilities.
- CVSS is an example of Open Source Intelligence (**OSINT**)

Formatting Standards for Threat Intelligence
- Structured threat information expression (**STIX**)
	- stixproject.github.io
	- Information on
		- Bad Actors
		- Incidents that occurred
		- Indicators of Compromise (**IOC**)
		- Tactics and Exploits used to carry out attacks
- Trusted automate exchange of indicator information (**TAXII**)
	- taxiiproject.guthub.io
	- application [protocol](https://notes.ryancranie.com/Notes/Network%20Technologies/Protocols) for exchanging cyber threat intelligence (**CTI**) over HTTPS
	- defines a RESTful API and a set of requirements for TAXII clients and servers.

RESTful API
Application Programming Interface (**API**) that conforms to the constraints of REST architecture.

CTI Process steps
1. Identify primary threats to your network.
	- impossible to stop all cyber-defences
	- decide via CVSS and Fortiguard
2. Assemble threat information from internal and external sources
3. Process the information
4. Analyze the information and look for IoC
5. Disseminate analysis and any new information
6. Implement lessons learned

## Attack Frameworks

Attack frameworks are a toolbox for cybersecurity professionals to enhance an organization's security posture.

Advanced Persistent Threats (**APTs**)

Cyber Kill Chain
1. **Reconnaissance**
	- the attacker gathers information about the target and its vulnerabilities.
2. **Weaponization**
	- the attacker creates a payload or exploit that can be delivered to the target.
		- malware, virus, trojan horse.
3. **Delivery**
	- the attacker delivers the payload to the target
		- email attachment
		- exploiting vulnerability in a website
4. **Exploitation**
	- the attacker uses the weapon to access a system or leverage vulnerabilities
	- executing/using payload
5. **Installation**
	- the attacker establishes a foothold within the target's systems, ensures persistent access.
		- installing **rootkit**
		- installing malware
6. **Command & Control**
	- the attacker establishes a means of communication with the compromised systems. 
		- C&C server
7. **Exfiltration**
	- the attacker extracts the data or other assets that were the goal of the attack.
		- copying data to remote system
		- using compromised systems as attackers.

A **rootkit** is software used by bad actors to gain control over a targeted computer or network.
A **payload** is malicious code. Originally, it referred to the load carried by a truck, airplane or missile.

Disadvantages of Cyber Kill Chain Attack Framework
1. Assumes that the origin of the attack is external to the network
2. Kill chain methodology aims to support traditional defense methods.

MITRE ATT&CK
This Attack Framework classifies and describes cyberattacks and intrusions.
- The Matrix is organized into a series of "techniques".
- Techniques are grouped into categories based on the type of attack or activity being performed.
	- Framework provides tactics, techniques and procedures (**TTPs**) used by attackers.
## Links
### Revision History
001: 2024-09-25 - INITIALIZE

---
<font size=3><b>[INTRODUCTION TO THE THREAT LANDSCAPE CONTENTS](https://github.com/ryancranie/cybersecurity-osint/blob/main/Contents/-%20Introduction%20to%20the%20Threat%20Landscape%20Contents.md)<br>
[README](https://github.com/ryancranie/cybersecurity-osint/blob/main/README.md)<br>
[LICENSE](https://github.com/ryancranie/cybersecurity-osint/blob/main/LICENSE)</b></font>