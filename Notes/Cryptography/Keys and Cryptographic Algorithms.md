# Keys and Cryptographic Algorithms

<audio controls>
    <source src="https://github.com/ryancranie/notes/raw/refs/heads/main/Attachments/Audio/Keys and Cryptographic Algorithms.mp3" type="audio/mpeg">
    Your browser does not support the audio tag.
</audio>
↑ AI-Generated Audio Overview via @<a href="https://notebooklm.google/">NotebookLM</a>

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
- Public keys are often used for digital signatures

## Key Lengths and Strengths

| Variable   | Large Key (1024+)                   | Small Key (128/256 bits)                                    |
| ---------- | ----------------------------------- | ----------------------------------------------------------- |
| Encryption | Encrypt small pieces of data        | Encrypt bulk data                                           |
| Usage      | Key transfer and digital signatures | Streams of data or bulk data, where performance is critical |
| Strength   | Length + Complexity                 | Length + Complexity                                         |

## Key Stretching

**Key Stretching** is a method used to strengthen keys or passwords that are either too short or predictable via feeding the key/password into a type of hashing algorithm, producing an enhanced key/password.
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
	- 48-bit key: encrypts 64-bit blocks
	- developed by IBM, approved by NIST in the 1970s
- Triple DES (**3DES**)
	- DES but 3 times
- International Data Encryption Algorithm (**IDEA**)
	- 128-bit key: encrypts 64-bit blocks
	- developed in 1991
- Advanced Encryption Standard (**AES**)
	- 128/192/256-bit key: encrypts 128-bit blocks
	- renamed to AES from Rijndael after it was adopted by NIST in 2001
- Rivest [Ciphers](https://notes.ryancranie.com/Notes/Cryptography/Ciphers)
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
		- configurable key size: encrypts 64-bit blocks
		- released 1993
	- **Twofish**
		- 128, 192, 256-bit key sizes: encrypts 128-bit blocks
		- published 1998: derived from Blowfish

## How does Symmetric Cryptography Work?
1. A crypto app uses a symmetric key and cipher to convert the plaintext into ciphertext. 
2. To decrypt it, the same symmetric key and cipher will convert the ciphertext back into plaintext

## Symmetric Key Cryptography Table

| Advantages                                                                | Disadvantages                             |
| ------------------------------------------------------------------------- | ----------------------------------------- |
| Faster to encrypt/decrypt (smaller key sizes, uses the same key for both) | Relies on a **secret** key                |
| Efficient to encrypt and decrypt large amounts of data                    | Need to protect and secure the secret key |

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

## How does asymmetric cryptography work?
1. The sender's crypto application converts the plaintext data into cipher text using the recipients asymmetric public key and asymmetric cipher. 
2. To decrypt it, the recipient's crypto application converts the ciphertext to plaintext using their private decryption key, and the asymmetric cipher that was used.

### Asymmetric Key Cryptography Table

| Advantages                                                     | Disadvantages                                                     |
| -------------------------------------------------------------- | ----------------------------------------------------------------- |
| Increased data security: users do not share their private keys | Operations are relatively slow compared to symmetric cryptography |
| Public key encrypts, private key decrypts                      | Not efficient to encrypt and decrypt large amounts of data        |

## Where to use symmetric/asymmetric algorithm
Considering both advantages and disadvantages of symmetric encryption and asymmetric encryption:
- symmetric encryption secures **bulk data** to resolve the slowness of asymmetric encryption
- asymmetric encryption secures the **symmetric key** to resolve the problem of safely transferring the secret

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

## Glossary

### Keys
<details><summary><b>Digital Key</b></summary>An alphanumeric value used for cryptographic operations like digital signatures and encryption.<br><br></details>
<details><summary><b>Key Lifetime</b></summary>The duration for which a key remains valid, ranging from seconds to years.<br><br></details>
<details><summary><b>Private/Secret Key</b></summary>A key kept confidential and used for decryption and digital signature generation.<br><br></details>
<details><summary><b>Public Key</b></summary>A key made available to everyone and used for encryption and digital signature verification.<br><br></details>

### Algorithms
<details><summary><b>Symmetric Algorithm</b></summary>An encryption method using the same key for both encryption and decryption.<br><br></details>
<details><summary><b>Asymmetric Algorithm</b></summary>An encryption method using a pair of related keys, one for encryption (public key) and one for decryption (private key).<br><br></details>
<details><summary><b>Key Stretching</b></summary>A technique to strengthen weak keys or passwords by processing them through hashing algorithms.<br><br></details>

### Cryptographic Techniques and Concepts
<details><summary><b>Digital Signature</b></summary>An electronic signature verifying the authenticity and integrity of a digital message.<br><br></details>
<details><summary><b>Message Authentication Code (MAC)</b></summary>A hash function using a key to ensure message integrity and authenticity.<br><br></details>
<details><summary><b>Encryption</b></summary>The process of converting plaintext into an unreadable format (ciphertext) for secure transmission.<br><br></details>
<details><summary><b>Key Exchange</b></summary>The secure exchange of cryptographic keys between two parties over a public channel.<br><br></details>
<details><summary><b>Salt</b></summary>A random value added to data before hashing, enhancing security by increasing entropy.<br><br></details>
<details><summary><b>Entropy</b></summary>The randomness or unpredictability of data, crucial for cryptographic security.<br><br></details>

### Specific Algorithms and Standards
<details><summary><b>DES (Data Encryption Standard)</b></summary>A symmetric encryption algorithm using a 48-bit key, encrypting 64-bit blocks.<br><br></details>
<details><summary><b>3DES (Triple DES)</b></summary>A symmetric encryption algorithm applying the DES algorithm three times for enhanced security.<br><br></details>
<details><summary><b>IDEA (International Data Encryption Algorithm)</b></summary>A symmetric encryption algorithm using a 128-bit key, encrypting 64-bit blocks.<br><br></details>
<details><summary><b>AES (Advanced Encryption Standard)</b></summary>A widely used symmetric encryption algorithm with key sizes of 128, 192, or 256 bits, encrypting 128-bit blocks.<br><br></details>
<details><summary><b>Blowfish/Twofish</b></summary>Symmetric encryption algorithms, with Blowfish encrypting 64-bit blocks and Twofish encrypting 128-bit blocks.<br><br></details>
<details><summary><b>RC4, RC5, RC6 (Rivest Ciphers)</b></summary>A family of symmetric encryption algorithms, with varying key and block sizes.<br><br></details>
<details><summary><b>Diffie-Hellman</b></summary>An asymmetric algorithm for key exchange over an insecure channel.<br><br></details>
<details><summary><b>RSA (Rivest, Shamir, and Adleman)</b></summary>A widely used asymmetric algorithm for encryption, decryption, digital signatures, and key exchange.<br><br></details>
<details><summary><b>ECC (Elliptic Curve Cryptography)</b></summary>An asymmetric encryption method offering strong security with smaller key sizes.<br><br></details>
<details><summary><b>PGP (Pretty Good Privacy)</b></summary>A popular email encryption tool using asymmetric encryption.<br><br></details>
<details><summary><b>GPG (GNU Privacy Guard)</b></summary>An open-source implementation of the PGP standard for email and file encryption.<br><br></details>
<details><summary><b>DSA (Digital Signature Algorithm)</b></summary>An algorithm used for creating and verifying digital signatures.<br><br></details>
<details><summary><b>DSS (Digital Signature Standard)</b></summary>A standard specifying the use of DSA for digital signatures.<br><br></details>
<details><summary><b>FIPS 186</b></summary>A U.S. government standard specifying the use of DSA as the Digital Signature Standard.<br><br></details>
<details><summary><b>PBKDF2 (Password-Based Key Derivation Function 2)</b></summary>A key stretching algorithm used to derive strong keys from passwords.<br><br></details>
<details><summary><b>BCRYPT</b></summary>A key stretching algorithm that is the default in OpenBSD and various Linux distributions.<br><br></details>

## QnA
<details><summary><b>What is a digital key?</b></summary>A digital key is an <b>alphanumeric</b> value used for <b>cryptographic</b> operations.<br><br></details>
<details><summary><b>What is the difference between a private key and a public key?</b></summary>A private key is kept <b>secret</b> and used for <b>decryption</b> and digital signature generation, while a public key is made available to <b>everyone</b> and used for encryption and digital signature verification.<br><br></details>
<details><summary><b>What is the purpose of key stretching?</b></summary>Key stretching is used to <b>strengthen</b> weak keys or passwords by making them <b>longer</b> and more complex.<br><br></details>
<details><summary><b>Name one advantage and one disadvantage of symmetric cryptography?</b></summary>One advantage is that it is <b>faster</b> for encrypting/decrypting data, and a disadvantage is that it relies on a <b>secret key</b>, which must be protected.<br><br></details>
<details><summary><b>Explain the concept of a salt in cryptography.</b></summary>A salt is a <b>random</b> value that is added to data before it is <b>hashed</b>. This process helps to increase entropy and make it more difficult for attackers to guess the <b>original</b> data. For example, if two users have the same password, adding a unique salt to each password before hashing will result in different hash values, even though the passwords are the same. This makes it more difficult for an attacker to crack multiple passwords at once.<br><br></details>
<details><summary><b>What are the key differences between DES and AES?</b></summary><b>DES</b> (Data Encryption Standard) is an older symmetric encryption algorithm that uses a <b>48-bit key</b> to encrypt <b>64-bit</b> blocks of data. <b>AES</b> (Advanced Encryption Standard), on the other hand, is a more modern algorithm that uses key sizes of <b>128</b>, <b>192</b>, or <b>256 bits</b> to encrypt <b>128-bit</b> blocks. AES is generally considered to be more <b>secure</b> than DES due to its larger key size and more complex algorithm.<br><br></details>
<details><summary><b>Describe how symmetric and asymmetric cryptography can be used together to securely transmit data.</b></summary>Symmetric and asymmetric encryption can be used together to overcome their individual weaknesses and enhance security. This is how they work together:<br><ul><li><b>Symmetric encryption</b> is used to encrypt the bulk data because it is faster and more efficient for large amounts of data.</li><li><b>Asymmetric encryption</b> is used to securely transmit the symmetric key that was used to encrypt the data.</li></ul>Here’s a step-by-step example:<br><br>1. <b>Encryption:</b><br><ul><li>Sender A generates a one-time symmetric key and uses a symmetric algorithm to encrypt the bulk data.</li><li>Sender A uses receiver B's <b>public key</b> and an asymmetric algorithm to encrypt the one-time symmetric key.</li><li>Sender A sends both the encrypted data and the encrypted symmetric key to B.</li></ul>2. <b>Decryption:</b><br><ul><li>Receiver B uses their <b>private key</b> and the same asymmetric algorithm to decrypt the symmetric key.</li><li>Receiver B uses the decrypted symmetric key and the same symmetric algorithm to decrypt the bulk data.</li></ul>This method combines the speed of symmetric encryption with the security of asymmetric encryption, enabling the secure transmission of large amounts of data.<br><br></details>

## Links
### Revision History
001: 2024-09-25 - Initialized Keys and Cryptographic Algorithms.md
002: 2024-10-14 - Created Audio Overview, Glossary, and QnA

---
<b>[Cryptography Contents](https://notes.ryancranie.com/Contents/Cryptography%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)