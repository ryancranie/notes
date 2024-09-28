# Ciphers

A **cipher** is a secret or disguised way of writing code.

**Cryptographic algorithms** and **digital keys** allow you to encrypt and decrypt information.

**Algorithms** are public information and keys are usually secrets.

## Types of Ciphers

1. **Substitution Cipher Type**
	- Ceaser's Cipher:
		- The number of letter-shifts is the secret/key
		- The method of shifting is the cipher algorithm
2. **Transpositional Cipher Type**
	- rearranging letters
	- Rail fence method
		- plaintext message wrote in a zigzag form between 3 rows
			- receiver of cipher would have to know the no. of rows (this is the shared secret/key)
3. **The One-Time Pad Cipher Type**
	- introduces randomness to the substitution method
		- randomness is powerful because it ensures that there is an equal chance of converting all letters: no repetitive patterns
	- each letter has a random value (1-26) so it knows how much to shift itself by
	- the shared secret is used only once, and pads of paper were needed to decrypt it, hence the name One-Time Pad

## Ciphers Used by Computers

1. **Stream Cipher**
	- encrypts a stream of plaintext data one bit/byte at a time
		- **FISH**
		- **RC4**
	- faster than block ciphers
1. **Block Cipher**
	- breaks plaintext into blocks for encryption
		- Data Encryption Standard (**DES**)
		- **3DES1**
		- Advanced Encryption Standard (**AES**)
		- Blowfish
	- size of block is dependant on the size of the key
		- if key = 256 bits: blocks will also be 256 bits
			- if messages is 1 megabyte, message would be split into blocks of 256 bits

## Links
### Cryptography

- [Keys and Cryptographic Algorithms](https://notes.ryancranie.com/Notes/Cryptography/Keys%20and%20Cryptographic%20Algorithms)


### Revision History
001: 2024-09-25 - Initialized Ciphers.md

---
<b>[Cryptography Contents](https://notes.ryancranie.com/Contents/Cryptography%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)