# PKI

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

## Links
### Revision History
001: 2024-09-25 - Initialized PKI.md

---
<b>[Cryptography Contents](https://notes.ryancranie.com/Contents/Cryptography%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)