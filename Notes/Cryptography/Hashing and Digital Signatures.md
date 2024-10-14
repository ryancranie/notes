# Hashing and Digital Signatures

<audio controls>
    <source src="https://github.com/ryancranie/notes/raw/refs/heads/main/Attachments/Audio/Hashing and Digital Signatures.mp3" type="audio/mpeg">
    Your browser does not support the audio tag.
</audio>
↑ AI-Generated Audio Overview via @<a href="https://notebooklm.google/">NotebookLM</a>

## Hashing
**Hashing** is the process of converting data of an arbitrary size to a unique value of a fixed size.
1. The **output value** is a fixed length
	- determined by hashing algorithm
	- denies the bad actor information about the nature of the input data
	- aka: hash value, hash code, hash result, digests, hashes, or images.
		- input value can be called pre-image, and image is the hashing output value.
2. Output value is **unique** for every input value
	- detects changes to input data when output is different
	- denies the bad actor the ability to alter data and get away with it
	- avoids **collisions** (same hash value from different inputs)
3. Hashing is **non-reversible** and one way
	- cannot reproduce the input by using output
	- denies the bad actor access to input value with hash value

Hashing and asymmetric cryptography are needed to produce a **digital signature**

Digital signatures ensure:
- **Integrity**
- **Authentication**
- **Non-repudiation**

## The Signing Process
1. The information that is to be signed, is hashed
	- Produces a hash value
2. The signer's asymmetric private key encrypts that hash value with an asymmetric algorithm
	- Produces a digital signature
3. The digital signature is attached to the information

In order to verify the signed information, you need the signers corresponding public asymmetric verification key.
- In asymmetric cryptography, public keys are written to certificates.
- certificates are issued and signed by a Certification Authority (**CA**) 

The **Verifying Process**
1. Receiver's application hashes the information, producing a new hash value
2. Ensure the signers verification certificate is valid
3. The asymmetric public key (from the certificate) decrypts the digital signature, into a hash value
4. Compare both the hashes from step 1/3, if they're the same then the information has not changed

## Common hash functions
- Message digest 5 (**MD5**) and **MD6**
	- MD5 (128-bit output) was widely used until collisions were common
- Secure Hashing Algorithm one (**SHA-1**), **SHA-2**, and **SHA-3**
	- SHA-1 is 160-bit fixed
	- SHA-2 includes SHA-224, SHA-256, SHA-384, and SHA-512
	- SHA-3 has various output lengths
- Microsoft **LANMAN** and NT LAN Manager (**NTLM**) algorithm
	- LANMAN is legacy windows for storing passwords
		- used DES which proved not secure (brute force)
	- NTLM succeeded LANMAN 
- **HAVEL** and **RIPEMD**
	- popular outside NA

## Common Attacks Against Hacking
1. **Brute force**
	- more effective if hacker has clues about the nature of input data
		- example: hacker knows password has to be above 10 chars, below 20, other policies
		- example: hacker knows that its a 4digit pin encrypted using MD5
	- will try different input values until they get the same hash value
	- hacker does not care if the password is the same: just the hashes. Will exploit hashing algorithms susceptible to collisions
		- servers do not store user passwords, just the hashes
	- BEST DEFENCE: ensure that the hash function supports an output that is long enough to be computationally infeasible to break
		- increase the size of the hash output length
		- also, increase entropy via key stretching
			- add salt value to key

## Glossary

Here’s the reformatted glossary based on your specified conditions:

### Hashing 

<details><summary><b>Hashing</b></summary>Hashing is a process that transforms data of any size into a fixed-size unique value.<br><br></details>
<details><summary><b>Hash value</b></summary>A hash value is a fixed-length output produced by a hashing algorithm, used for data integrity verification.<br><br></details>
<details><summary><b>Collisions</b></summary>Collisions occur when different input data produces the same hash value.<br><br></details>
<details><summary><b>Pre-image</b></summary>A pre-image is the original input data used in hashing.<br><br></details>
<details><summary><b>Image</b></summary>An image is the output, or hash value, produced from the input data.<br><br></details>
<details><summary><b>Brute force</b></summary>Brute force is a method of cracking passwords by trying every possible combination.<br><br></details>

### Digital Signatures

<details><summary><b>Digital signatures</b></summary>Digital signatures use hashing and asymmetric cryptography to ensure the integrity, authentication, and non-repudiation of data.<br><br></details>
<details><summary><b>Integrity</b></summary>Integrity in digital signatures refers to ensuring that the data has not been altered.<br><br></details>
<details><summary><b>Authentication</b></summary>Authentication in digital signatures verifies the identity of the sender.<br><br></details>
<details><summary><b>Non-repudiation</b></summary>Non-repudiation in digital signatures prevents the sender from denying they sent the data.<br><br></details>
<details><summary><b>Certification Authority (CA)</b></summary>A Certification Authority (CA) issues and signs digital certificates, which contain public keys used for verification.<br><br></details>

### Hashing Algorithms

<details><summary><b>MD5 (Message Digest 5)</b></summary>MD5 (Message Digest 5) is a hashing algorithm that produces a 128-bit hash value but is now considered insecure due to vulnerabilities to collisions.<br><br></details>
<details><summary><b>SHA-1 (Secure Hashing Algorithm 1)</b></summary>SHA-1 (Secure Hashing Algorithm 1) is a hashing algorithm that produces a 160-bit hash value but is also considered less secure than newer algorithms.<br><br></details>
<details><summary><b>SHA-2 (Secure Hashing Algorithm 2)</b></summary>SHA-2 (Secure Hashing Algorithm 2) is a family of hashing algorithms, including SHA-224, SHA-256, SHA-384, and SHA-512, offering stronger security than SHA-1.<br><br></details>
<details><summary><b>SHA-3 (Secure Hashing Algorithm 3)</b></summary>SHA-3 (Secure Hashing Algorithm 3) is the latest generation of SHA algorithms with various output lengths.<br><br></details>
<details><summary><b>LANMAN</b></summary>LANMAN is a legacy Windows algorithm for storing passwords using DES, which is no longer considered secure.<br><br></details>
<details><summary><b>NTLM (NT LAN Manager)</b></summary>NTLM (NT LAN Manager) is a password hashing algorithm that succeeded LANMAN, offering improved security.<br><br></details>
<details><summary><b>HAVAL</b></summary>HAVAL and RIPEMD are hashing algorithms that are popular outside of North America.<br><br></details>

### Security Concepts

<details><summary><b>Key stretching</b></summary>Key stretching is a technique used to strengthen passwords by making them more computationally expensive to crack.<br><br></details>
<details><summary><b>Salt value</b></summary>A salt value is a random string added to a password before hashing to increase entropy and make brute force attacks more difficult.<br><br></details>

## QnA
<details><summary><b>What is hashing?</b></summary>Hashing is the process of converting data of any size into a unique, fixed-size <b>value</b>.<br><br></details>
<details><summary><b>What are two common hash functions?</b></summary>Two common hash functions are <b>MD5</b> (Message Digest 5) and <b>SHA-1</b> (Secure Hashing Algorithm 1).<br><br></details>
<details><summary><b>What does a digital signature ensure?</b></summary>A digital signature ensures <b>integrity</b>, <b>authentication</b>, and <b>non-repudiation</b> of data.<br><br></details>
<details><summary><b>What is the purpose of a salt value in hashing?</b></summary>A salt value is added to a password before hashing to increase <b>entropy</b>, making brute force attacks more difficult.<br><br></details>
<details><summary><b>Describe the process of creating a digital signature.</b></summary><ul><li>The information to be signed is hashed, producing a hash value.</li><li>The signer's private key encrypts the hash value, creating a digital signature.</li><li>The digital signature is then attached to the information.</li></ul><br></details>
<details><summary><b>What are some common attacks against hashing, and how can they be mitigated?</b></summary>A common attack against hashing is a <b>brute force attack</b>, where a hacker tries different input values until they get the same hash value as the target. This is easier if the hacker has information about the input data. Another attack exploits <b>collisions</b>, where different input values produce the same hash value.<br>To defend against these attacks, ensure the hash function supports a long output length to make it computationally infeasible to break. Also, increase <b>entropy</b> via <b>key stretching</b>, such as adding a salt value to the key.<br><br></details>
<details><summary><b>Explain the process of verifying a digital signature, including the role of certificates and Certification Authorities.</b></summary>The verification process ensures the integrity and authenticity of the signed information. Here's how it works:<br><ul><li><b>Hashing the received information:</b> The receiver's application hashes the received information, creating a new hash value.</li><li><b>Certificate Validation:</b> The receiver must ensure that the signer's verification certificate is valid. Certificates contain the signer's public key and are issued and signed by a trusted Certification Authority (<b>CA</b>). This step ensures the public key used for verification is legitimate and associated with the claimed sender.</li><li><b>Decryption:</b> The receiver uses the signer's public key, extracted from the validated certificate, to decrypt the digital signature. This decryption process yields the original hash value that the signer created.</li><li><b>Comparison:</b> Finally, the receiver compares the newly generated hash value (from step 1) with the decrypted hash value (from step 3). If the two hash values match, it confirms that the information has not been altered since it was signed and that the signature is valid.</li></ul>This process relies on the principles of <b>asymmetric cryptography</b>, where the signer's private key is used for encryption (signing), and the corresponding public key is used for decryption (verification). The use of certificates and CAs provides a trusted mechanism for associating public keys with identities, ensuring the authenticity of the signature.<br><br></details>

## QnA

## Links
### Revision History
001: 2024-09-25 - Initialized Hashing and Digital Signatures.md
002: 2024-10-14 - Created Audio Overview, Glossary, and QnA

---
<b>[Cryptography Contents](https://notes.ryancranie.com/Contents/Cryptography%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)