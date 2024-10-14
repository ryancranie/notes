# PKI

<audio controls>
    <source src="https://github.com/ryancranie/notes/raw/refs/heads/main/Attachments/Audio/PKI.mp3" type="audio/mpeg">
    Your browser does not support the audio tag.
</audio>
↑ AI-Generated Audio Overview via @<a href="https://notebooklm.google/">NotebookLM</a>

Public Key Infrastructure (**PKI**) is an ecosystem comprised of
- Policies
- Procedures
- Hardware
- Software

These are used to

- Create
- Distribute
- Store
- Use
- Revoke

Digital Certificates

A digital certificate is revoked when it is no longer trusted
- the content in the certificate is rendered void, including the public key

1. Certification Authority (**CA**)
2. Registration Authority (**RA**)
3. Directory Server
4. End Entity

## Digital Certificates
In cryptography, a **digital certificate** is an electronic document issued, and signed by a trusted entity, such as a CA.
- It contains the name of the certificate holder, and may/may not contain a public key
- if the certificate has been issued to a specific person, device or application:
	- -> it binds the entity's identity to the public key by way of a digital signature
	- the certificate is trusted because it is signed by a trusted identity
- certificates with public keys:
	- **Encryption Certificate**
		- contains a public encryption key used for encipherment
	- **Verification Certificate**
		- contains a public verification key used for verifying a signature
- certificates without public keys
	- **Policy Certificate**
		- contains policy information
	- **Certificate Revocation List**s (**CRLs**)
		- contains information about revoked certificates
- different practices for producing & managing certificates, but the most important is **X.509 version 3**

Certificates are elements that allow trust and secure communications between entities within a private or public domain. Given it's importance, the CA deserves critical consideration

Certificate templates specify the attributes that will be in different certificate types
- including the purpose of the certificate
- example attribute:
	- KEY USAGE = KEY ENCIPHERMENT
	- this is an **Encryption Key** for a **Encryption Certificate**

## Common Fields found in a certificate
1. **Serial Number**
	- unique value assigned by the trusted entity/CA that created the certificate
	- used to identify the the certificate
		- example: revoked certificates are identified by CRLs via their serial numbers
2. **Algorithms**
	- algorithms that were used to hash and sign the certificate
		- aka: encrypt the hash result
3. **Validity Period**
	- defines range of dates which the certificate is valid
4. **Issuer Name**
	- identifies the trusted entity which issued certificate
		- often a CA
	- the name is expressed as a Distinguished Name (**DN**). The DN maps to the entity's entry in a directory server
		- entries in server are organized into a hierarchical fashion, resembling a tree, and is referred to as a Directory Information Tree (**DIT**)
5. **Subject Name**
	- identifies the entity (person, device or application) which is bound to the other content in the certificate
	- can be expressed as:
		- DN
		- email address
		- Fully Qualified Domain Name (**FQDN**)
		- some other unique identifier within the systems certificates operate
	- can be obfuscated to keep the identity private
		- example: company that issues certificates to customers can hide those customers from its competitors
6. **Key value**
	- the alphanumeric code embedded in the certificate
		- can be the public asymmetric key
7. **Key usage**
	- defines how the key can be used
	- examples:
		- KEY USAGE = keyEncipherment
			- the key pair is used to encrypt and decrypt
		- KEY USAGE = digitalSignature
			- the key pair is used to sign and verify signatures
		- CA certificates must have the **keyCertSign attribute**
			- needed in order for the CA's keys to sign and verify certificates

## The CA 2 primary functions
1. Issue certificates to end entities and to the system to help manage the following certificates
	- CRLs
	- Policy certificates
2. Establish an ecosystem of trust, where all entities within it can safely interact with one another. 
	- Trust hinges on the CA's private key
		- all certificates issued by the CA are signed by its private key
		- storing private keys:
			- can be stored in a key store that is provided by the OS
				- windows: Microsoft Cryptographic Service Provider (**CSP**) key store, aka **MS CAPI**
			- other applications can provide CSPs and stores to protect private keys
			- enhanced security: private keys can be generated and stored in a Hardware Security Module (**HSM**)
			- end entities can store their private keys in MS CAPI, or even a file.
				- typically have an extension of p12 (for PKCS#12) or key
					- PKCS #12 is one of the family of standards called Public-Key Cryptography Standards (**PKCS**)

## Why trust the private key
- Relationship with the trusted entity
- A legal framework supports the trust:
	- proof of the highest security standards
	- high-assurance versus low-assurance CAs
		- low-assurance CAs might not implement all of the security elements of a high-assurance CA
			- they do not have the same rigorous legal requirements
		- high-assurance CA can prove in court their CA's key pair was created in a secure manner

## PKI Organization
In a PKI implementation, multiple CAs can be organized in different ways.
- In a **hierarchical structure** there would be one **root CA**, and one or more **subordinate CAs**.
	- root CA is the source of trust
	- subordinate CAs issue certificates to end entities 
		- subordinate CAs can have subordinate CAs
- it is possible for a CA in one PKI to trust a CA of another PKI by **cross-certifying**
	- this is a **lateral structure**
		- the trust relationship exists two CAs of different PKIs
	- in the process of cross-certifying, cross certificates are created that allow users of one PKI to trust users of another PKI, and vice versa.
		- the cross-certification can take place between a root or subordinate CA from one PKI and a root or subordinate CA from the other PKI.

## Example PKI Hierarchical Environment
- the root CA's certificate is signed by its own private key
- two subordinate CAs are created below the root
	- during the initialization of these subordinate CAs, the public keys are sent to the root CA to be written to X.509 certificates and signed by the root CA's private key
- if you wanted to add more subordinate CAs, the superior sub CA signs the certificate

<img src="https://raw.githubusercontent.com/ryancranie/cybersecurity-osint/refs/heads/main/Attachments/Pasted%20image%2020240608153959.png" width="700">

For Entities:
- Trust the root CA and all certificates signed by its signing key
	- Verifies data integrity of the certificate
	- Authenticates the signature
		- Verifies data integrity of the document
		- Authenticates the signature
- The **Chain of Trust** is a series of associated certificates, each one relying on the other for trust, in which the chain eventually leads to the root CA, the **Source of Trust**
	- example: certificate of another CA in a hierarchical structure (document sent from A -> B)
		1. B's app retrieves the certificate associated with A's document
		2. B's app can use A's public key (found in certificate) to verify signature on the document
			- **B still does not trust A's certificate**
		3. B's app retrieves the certificate of the CA that issued A's certificate
		4. B's app uses the public key (found in the certificate)
			- **B still does not trust A's certificate**
		5. B's app retrieves the certificate associated with the CA, that issued that lower CA's certificate
		6. Because this CA is the root CA, and the source of trust for Mia
			- she trusts the certificates of the A and the subordinate CA.
			- **Trust the root CA and all certificates signed by its signing key**

## Storing Certificates in the Directory Server

There are two ways to be the recipient of a certificate
1. The sender's application sends both the **Signed Document** and the **Document** to the recipient
2. The sender's application sends only the **Signed Document**, the recipient retrieves their associated certificate from a **Directory Server**

PKI standards anticipate that end entities would retrieve certificates from an **X.500** and Lightweight Directory Access Protocol (**LDAP**) compliant directory server.

**LDAP** is an open, vendor-neutral, industry-standard application for accessing and maintaining **distributed directory information services** over an IP network.

The Directory Server stores
- User Certificates
- Device Certificates
- All Certificates in the PKI
	- Policy
	- CRLs
		- A CRL is a certificate that lists revoked **end entity certificates**
	- Authority Revocation Lists (**ARLs**)
		- An ARL is a certificate that lists revoked **CA certificates**, and revoked **cross certificates**.
	- Cross-Certificates
	- CA Certificates

X.500 is a series of standards for directory services developed by the International Telecommunication Union (**ITU-T**)

## Registration Authority (RA)
A registration authority (**RA**) is a function for certificate enrolment used in PKIs.
The RA verifies and forwards certificate requests to the CA.
- Certificates can be issued to
	- Persons
	- Devices
	- Applications
- Registration can be in person, or online (can be automated/manned)
	- High-assurance CA = probably manned in person
	- Other types of certificates can be registered for and purchased online
		- SSL certificates

1. End Entity sends request for certificate to the RA
2. RA approves/denies
3. The RA registers approved requests with the CA
4. Generates a one-time authorization code to the requestor
5. CA issues certificate and private key to end entity
6. CA stores issued certificates in the Directory Server

Using a certificate at the Endpoint

At the endpoint, the user needs a cryptographic application to
- generate key pairs
- generate and submit a Certificate Service Request (**CSR**)
	- can be manual/automated to a degree
- encrypt and sign
	- Microsoft CSP is built into the operating system

## Review
1. The RA vets and registers end entities for certificates
2. The CA creates, distributes and revokes certificates
3. The Directory Server stores certificates
4. The End Entity uses this privilege to
	- Generate keys
	- Submit CSRs
	- Validate Certificates
	- Use keys and certificates for
		- encrypting
		- decrypting
		- signing
		- verifying
5. All these entities carry out their functions in accordance with policy, which is prescribed by PKI authorities

## Glossary

### Digital Certificate Components

<details><summary><b>Digital Certificate</b></summary>An electronic document issued and signed by a trusted entity, binding an entity's identity to a public key.<br><br></details>

<details><summary><b>Encryption Certificate</b></summary>A certificate containing a public key specifically used for encrypting data.<br><br></details>

<details><summary><b>Verification Certificate</b></summary>A certificate containing a public key specifically for verifying digital signatures.<br><br></details>

<details><summary><b>Policy Certificate</b></summary>A certificate holding policy information, without a public key.<br><br></details>

<details><summary><b>Serial Number</b></summary>A unique identifier assigned by the issuing authority to each certificate.<br><br></details>

<details><summary><b>Algorithm</b></summary>A specific mathematical procedure used to create the certificate's hash and signature.<br><br></details>

<details><summary><b>Validity Period</b></summary>The defined timeframe during which a certificate is considered valid and operational.<br><br></details>

<details><summary><b>Issuer Name</b></summary>The identity of the trusted entity, often a CA, that issued the certificate.<br><br></details>

<details><summary><b>DN (Distinguished Name)</b></summary>A unique identifier mapping to an entity's entry in a hierarchical directory server.<br><br></details>

<details><summary><b>DIT (Directory Information Tree)</b></summary>A tree-like structure organizing entries in a directory server.<br><br></details>

<details><summary><b>Subject Name</b></summary>Identifies the entity bound to the certificate, potentially a person, device, or application.<br><br></details>

<details><summary><b>FQDN (Fully Qualified Domain Name)</b></summary>The complete domain name of a website or server.<br><br></details>

<details><summary><b>Key Value</b></summary>The alphanumeric code, often the public key, embedded within a certificate.<br><br></details>

<details><summary><b>Key Usage</b></summary>Defines the specific allowed use of the key within a certificate.<br><br></details>

<details><summary><b>keyCertSign Attribute</b></summary>An attribute required for a CA to use its keys to sign and verify other certificates.<br><br></details>

### PKI Infrastructure and Trust

<details><summary><b>PKI (Public Key Infrastructure)</b></summary>A system of policies, procedures, and technologies used to manage digital certificates.<br><br></details>

<details><summary><b>CA (Certification Authority)</b></summary>A trusted entity that issues and manages digital certificates.<br><br></details>

<details><summary><b>CRL (Certificate Revocation List)</b></summary>A list containing information about revoked certificates, making them untrusted.<br><br></details>

<details><summary><b>X.509 version 3</b></summary>The most important standard for managing and producing digital certificates.<br><br></details>

<details><summary><b>CSP (Cryptographic Service Provider)</b></summary>A software module providing cryptographic services, like key storage, often from an operating system.<br><br></details>

<details><summary><b>MS CAPI</b></summary>The Microsoft Cryptographic Service Provider, also known as the MS CAPI key store.<br><br></details>

<details><summary><b>HSM (Hardware Security Module)</b></summary>A physical device offering enhanced security for generating and storing private keys.<br><br></details>

<details><summary><b>PKCS (Public-Key Cryptography Standards)</b></summary>A set of standards for public-key cryptography, including PKCS#12, which defines a file format for storing private keys.<br><br></details>

<details><summary><b>Root CA</b></summary>The top-level CA in a hierarchical PKI structure, serving as the ultimate source of trust.<br><br></details>

<details><summary><b>Subordinate CA</b></summary>A CA that issues certificates to end entities and may be under a root CA in a hierarchical structure.<br><br></details>

<details><summary><b>Cross-Certifying</b></summary>Establishing trust between two CAs from different PKIs, enabling trust between their users.<br><br></details>

<details><summary><b>Lateral Structure</b></summary>A PKI structure where trust relationships exist between CAs of different PKIs.<br><br></details>

<details><summary><b>Chain of Trust</b></summary>A series of certificates, each relying on the one above it for trust, ultimately leading back to the root CA.<br><br></details>

<details><summary><b>Source of Trust</b></summary>The root CA in a PKI hierarchy, representing the foundation of trust for all certificates within that PKI.<br><br></details>

### PKI Processes and Management

<details><summary><b>RA (Registration Authority)</b></summary>An entity responsible for verifying and forwarding certificate requests to the CA.<br><br></details>

<details><summary><b>X.500</b></summary>A series of standards for directory services, commonly used to store and manage certificates.<br><br></details>

<details><summary><b>LDAP (Lightweight Directory Access Protocol)</b></summary>A standard protocol for accessing and managing distributed directory information services over an IP network.<br><br></details>

<details><summary><b>ARL (Authority Revocation List)</b></summary>A list specifically containing revoked CA certificates and cross-certificates.<br><br></details>

<details><summary><b>CSR (Certificate Service Request)</b></summary>A formal request generated and submitted to a CA to obtain a digital certificate.<br><br></details>

<details><summary><b>ITU-T (International Telecommunication Union)</b></summary>The organization responsible for developing the X.500 standards for directory services.<br><br></details>

## QnA
<details><summary><b>What does PKI stand for and what is it comprised of?</b></summary>PKI stands for <b>Public Key Infrastructure</b>. It is an ecosystem comprised of <b>policies</b>, <b>procedures</b>, hardware, and software used to manage digital certificates.<br><br></details>
<details><summary><b>What are the two types of certificates that contain public keys?</b></summary>The two types of certificates containing public keys are <b>Encryption Certificates</b> and <b>Verification Certificates</b>.<br><br></details>
<details><summary><b>What is the purpose of a Certificate Revocation List (CRL)?</b></summary>A CRL contains a list of <b>revoked certificates</b>, signifying that they are no longer trusted.<br><br></details>
<details><summary><b>What is a common file extension for storing private keys?</b></summary>Private keys are typically stored in files with a <code>.p12</code> (for PKCS#12) or <code>.key</code> extension.<br><br></details>
<details><summary><b>What is the difference between a hierarchical and lateral PKI structure?</b></summary><ul><li>In a <b>hierarchical structure</b>, there is a single <b>root CA</b> at the top, with one or more subordinate CAs below it. Subordinate CAs can further have their own subordinate CAs, creating a tree-like structure. Trust flows downwards from the root CA.</li><li>In a <b>lateral structure</b>, trust relationships are established between CAs of different PKIs through <b>cross-certification</b>. This allows users of one PKI to trust users of another PKI. Cross-certification can occur between root or subordinate CAs from different PKIs.</li></ul><br></details>
<details><summary><b>Explain the process of how an end entity obtains a certificate from a CA, involving the RA.</b></summary><ol><li>The end entity sends a request for a certificate to the <b>Registration Authority (RA)</b>.</li><li>The RA verifies the end entity's information and either approves or denies the request.</li><li>If approved, the RA registers the request with the <b>CA</b>.</li><li>The RA then generates a one-time authorization code for the requestor.</li><li>The CA issues the certificate and private key to the end entity.</li><li>The CA stores the issued certificates in the <b>Directory Server</b>.</li></ol><br></details>
<details><summary><b>Describe how the Chain of Trust works to establish trust in a digital certificate, using an example of a document being sent from entity A to entity B.</b></summary>The Chain of Trust is a mechanism for verifying the <b>authenticity</b> and <b>trustworthiness</b> of a digital certificate. It works by linking the certificate in question to a trusted root CA through a series of intermediate certificates.<br><b>Example:</b><ol><li>Entity A sends a signed document to entity B.</li><li>Entity B's application retrieves the certificate associated with A's document to verify the signature. However, B doesn't inherently trust A's certificate yet.</li><li>B's application then retrieves the certificate of the CA that issued A's certificate. This is the next link in the chain.</li><li>B's application uses the public key in this CA's certificate to verify the signature on A's certificate. Again, B still doesn't fully trust A's certificate.</li><li>This process repeats, with B retrieving the certificate of the CA that issued the previous CA's certificate, and so on.</li><li>Eventually, the chain leads to the root CA's certificate. Since the root CA is the ultimate source of trust, B trusts its certificate.</li><li>Because each certificate in the chain has been verified by the one above it, trust flows down the chain, and B can now trust A's certificate and the signed document.</li></ol><b>Key point:</b> The chain of trust relies on each entity trusting the certificate of the issuing CA in the chain, ultimately leading back to the trusted root CA. This ensures the authenticity and integrity of the final certificate and any associated data.<br><br></details> 

## Links
### Cryptography

- [Cryptography Overview](https://notes.ryancranie.com/Notes/Cryptography/Cryptography%20Overview)


### Revision History
001: 2024-09-25 - Initialized PKI.md
002: 2024-10-14 - Created Audio Overview, Glossary, and QnA

---
<b>[Cryptography Contents](https://notes.ryancranie.com/Contents/Cryptography%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)