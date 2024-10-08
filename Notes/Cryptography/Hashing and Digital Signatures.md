# Hashing and Digital Signatures

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

## Links
### Revision History
001: 2024-09-25 - Initialized Hashing and Digital Signatures.md

---
<b>[Cryptography Contents](https://notes.ryancranie.com/Contents/Cryptography%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)