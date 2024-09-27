# Keys and Cryptographic Algorithms

## Digital Keys

A digital key is a value, that can be expressed alphanumerically, that is used to perform cryptographic operations
- Digital signature
- Message authentication code (**MAC**)
	- A **MAC** is a hash function that uses a digital key to ensure the integrity and authenticity of the message.
- Encryption
	- flow of information between two devices
	- bulk stationary data
	- small piece of data
		- another key
		- hash value

Keys have a lifetime, which can be set from years to seconds, depending on the task.

Keys can be private/secret, but can also be public:
- Public keys are often used for [digital signatures](https://github.com/ryancranie/cybersecurity-osint/blob/main/Notes/Cryptography%20and%20the%20Public%20Key%20Infrastructure/Hashing%20and%20Digital%20Signatures.md)

Key Lengths and Strengths

| Variable   | Large Key (1024+)                   | Small Key (128/256 bits)                                    |
| ---------- | ----------------------------------- | ----------------------------------------------------------- |
| Encryption | Encrypt small pieces of data        | Encrypt bulk data                                           |
| Usage      | Key transfer and digital signatures | Streams of data or bulk data, where performance is critical |
| Strength   | Length + Complexity                 | Length + Complexity                                         |
## Key Stretching

**Key Stretching** is a method used to strengthen keys or passwords that are either too short or predictable via feeding the key/pasword into a type of hashing algorithm, producing an enhanced key/password.
Example key stretching algorithms 
- password-based key derivation function two (**PBKDF2**)
- **BCRYPT**
	- default alg used in OpenBSD and various Linux distros
	- key/password is hashed multiple times before a 128-bit **salt** is added, then hashed once more

A **salt** is a random value added to another value to increase **entropy**

## Symmetric Algorithm
A **symmetric algorithm** is a cipher used to encrypt and decrypt data using the same key
Example symmetric algorithms:
- Data Encryption Standard (**DES**)
	- 48-bit key | encrypts 64-bit blocks
	- developed by IBM, approved by NIST in the 1970s
- Triple DES (**3DES**)
	- DES but 3 times
- International Data Encryption Algorithm (**IDEA**)
	- 128-bit key | encrypts 64-bit blocks
	- developed in 1991
- Advanced Encryption Standard (**AES**)
	- 128/192/256-bit key | encrypts 128-bit blocks
	- renamed to AES from Rijndael after it was adopted by NIST in 2001
- Rivest Ciphers
	- **RC4**
		- 40-bit - 2048-bit key
		- stream cipher
	- **RC5**
		- configurable key and block sizes
		- developed in 1994
	- **RC6**
		- derived from RC5, except encrypts 128-bit blocks
		- won the AES algorithm competition
- Blowfish / Twofish
	- **Blowfish**
		- configurable key size | encrypts 64-bit blocks
		- released 1993
	- **Twofish**
		- 128, 192, 256-bit key sizes | encrypts 128-bit blocks
		- published 1998: derived from Blowfish

Symmetric Key Cryptography Table

| Advantages                                                                | Disadvantages                             |
| ------------------------------------------------------------------------- | ----------------------------------------- |
| Faster to encrypt/decrypt (smaller key sizes, uses the same key for both) | Relies on a **secret** key                |
| Efficient to encrypt and decrypt large amounts of data                    | Need to protect and secure the secret key |
How does Symmetric Cryptography Work?
1. A crypto app uses a symmetric key and cipher to convert the plaintext into ciphertext. 
2. To decrypt it, the same symmetric key and cipher will convert the ciphertext back into plaintext
## Asymmetric Algorithm
An **asymmetric algorithm** is a cipher used for cryptographic operations using a pair of mathematically related keys.
- Encryption
- Key exchange
	- enables 2 parties to exchange a key over a public channel
- Digital signature

In asymmetric key cryptography:
- The public key encrypts
- The private key decrypts

Asymmetric algorithms are also known as public-key algorithms
Examples of asymmetric algorithms:
- Diffie-Hellman
	- published in 1976 by Whitfield Diffie and Martin Hellman
- Rivest, Shamir and Adleman (**RSA**)
	- RSA can encrypt, decrypt, sign and verify.
- Elliptic Curve Cryptography (**ECC**)
	- smaller keys than other forms of cryptography
- Pretty Good Privacy (**PGP**)
	- developed in 1991 by Phil Zimmerman
	- popular email encryption tool
- **GPG**
	- PGP but open source
- Digital Signing Algorithm (**DSA**)
	- in 1994, NIST adopted DSA as its digital signature standard (**DSS**) as **FIPS 186**

Asymmetric Key Cryptography Table

| Advantages                                                     | Disadvantages                                                     |
| -------------------------------------------------------------- | ----------------------------------------------------------------- |
| Increased data security: users do not share their private keys | Operations are relatively slow compared to symmetric cyrptography |
| Public key encrypts, private key decrypts                      | Not efficient to encrypt and decrypt large amounts of data        |

How does asymmetric cryptography work?
1. The sender's crypto application converts the plaintext data into cipher text using the recipients asymmetric public key and asymmetric cipher. 
2. To decrypt it, the recipient's crypto application converts the ciphertext to plaintext using their private decryption key, and the asymmetric cipher that was used.
## Where to use symmetric/asymmetric algorithm
Considering both advantages and disadvantages of symmetric encryption and asymmetric encryption:
- symmetric encryption secures bulk data to resolve the slowness of asymmetric encryption
- asymmetric encryption secures the symmetric key to resolve the problem of safely transferring the secret

example: send from A to B
ENCRYPTION:
1. Symmetric key encrypts the bulk data
	- + A's one-time generated symmetric key
	- + **symmetric** algorithm
	- = ciphertext
2. B's public encryption key secures the symmetric key
	- + A's one-time generated symmetric key
		- + B's public key
		- + **asymmetric** algorithm
	- = encrypted one-time generated symmetric key
3. A's encrypted key and ciphertext are sent to B
DECRYPTION:
4. B's private decryption key decrypts the symmetric key
	- + B's private key (decrypts B's public key)
	- + same **asymmetric** algorithm
	- = that one-time **symmetric** key
5. The symmetric key decrypts the bulk data
	- + ciphertext
	- + same **symmetric** algorithm
	- = plaintext!!

## Links

### Revision History
001: 2024-09-25 - INITIALIZE

---
<font size=3><b>[CRYPTOGRAPHY AND THE PUBLIC KEY INFRASTRUCTURE CONTENTS](https://github.com/ryancranie/cybersecurity-osint/blob/main/Contents/-%20Cryptography%20and%20the%20Public%20Key%20Infrastructure%20Contents.md)<br>
[README](https://github.com/ryancranie/cybersecurity-osint/blob/main/README.md)<br>
[LICENSE](https://github.com/ryancranie/cybersecurity-osint/blob/main/LICENSE)</b></font>