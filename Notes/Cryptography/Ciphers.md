# Ciphers

<audio controls>
    <source src="https://github.com/ryancranie/notes/raw/refs/heads/main/Attachments/Audio/Ciphers.mp3" type="audio/mpeg">
    Your browser does not support the audio tag.
</audio>
↑ AI-Generated Audio Overview via @<a href="https://notebooklm.google/">NotebookLM</a>

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

## Glossary

Here’s the reformatted content based on your specifications:

### Cipher Types 

<details><summary><b>Ceaser’s Cipher</b></summary>A substitution cipher where the secret key is the number of letter shifts in the alphabet.<br><br></details>

<details><summary><b>Transpositional Cipher</b></summary>A cipher that rearranges letters to disguise the message.<br><br></details>

<details><summary><b>Rail Fence Cipher</b></summary>A transposition cipher that writes a plaintext message in a zigzag pattern between rows, where the number of rows is the secret key.<br><br></details>

<details><summary><b>One-Time Pad Cipher</b></summary>A cipher that introduces randomness to the substitution method, using a random value to shift each letter and a shared secret that is only used once.<br><br></details>

<details><summary><b>Stream Cipher</b></summary>Encrypts plaintext data one bit or byte at a time.<br><br></details>

<details><summary><b>Block Cipher</b></summary>Breaks plaintext into blocks of a set size for encryption, determined by the size of the key.<br><br></details>

### Specific Ciphers 

<details><summary><b>FISH</b></summary>A type of stream cipher used by computers.<br><br></details>

<details><summary><b>RC4</b></summary>A type of stream cipher used by computers.<br><br></details>

<details><summary><b>DES (Data Encryption Standard)</b></summary>A type of block cipher used by computers.<br><br></details>

<details><summary><b>3DES</b></summary>A type of block cipher used by computers.<br><br></details>

<details><summary><b>AES (Advanced Encryption Standard)</b></summary>A type of block cipher used by computers.<br><br></details>

<details><summary><b>Blowfish</b></summary>A type of block cipher used by computers.<br><br></details>

### General Terms 

<details><summary><b>Cipher</b></summary>A secret or disguised way of writing code.<br><br></details>

<details><summary><b>Cryptographic Algorithms</b></summary>Methods used to encrypt and decrypt information.<br><br></details>

<details><summary><b>Digital Keys</b></summary>Secret pieces of information used with cryptographic algorithms to encrypt and decrypt data.<br><br></details>

<details><summary><b>Algorithms</b></summary>Publicly available instructions for carrying out a process.<br><br></details>

<details><summary><b>Plaintext</b></summary>A message before it has been encrypted.<br><br></details>

## QnA
<details><summary><b>What is a cipher?</b></summary>A cipher is a <b>secret</b> or <b>disguised</b> way of writing code.<br><br></details>
<details><summary><b>What is the difference between a stream cipher and a block cipher?</b></summary>A <b>stream cipher</b> encrypts plaintext data one <b>bit</b> or <b>byte</b> at a time, while a block cipher breaks the plaintext into blocks for encryption.<br><br></details>
<details><summary><b>What is the key in a Caesar cipher?</b></summary>The key in a <b>Caesar cipher</b> is the number of letter-shifts used to encrypt the message.<br><br></details>
<details><summary><b>What are the two main types of ciphers?</b></summary>The two main types of ciphers are <b>substitution</b> ciphers and <b>transposition</b> ciphers.<br><br></details>
<details><summary><b>What makes the One-Time Pad cipher unique and considered highly secure?</b></summary>The One-Time Pad cipher is unique because it introduces <b>randomness</b> to the <b>substitution</b> method. Each letter in the plaintext is shifted by a random value between 1 and 26, determined by the shared secret key. This randomness ensures no <b>repetitive</b> patterns in the ciphertext, making it very difficult to break. The key is also only used once, further enhancing <b>security</b>.<br><br></details>
<details><summary><b>How does the rail fence method work as a transposition cipher?</b></summary>The rail fence method involves writing the plaintext message in a <b>zigzag</b> pattern across a set number of rows. The recipient must know the number of rows used to decipher the message. This number of rows serves as the shared secret key. For example, if three rows are used, the message "HELLO WORLD" would be written as:<br><code>H . . . O . . . L .<br>. E . L . W . R . D<br>. . L . . . O . . </code><br>The ciphertext would then be read sequentially, resulting in "HOLWLREDELO".<br><br></details>
<details><summary><b>Explain how the size of the key in a block cipher affects the encryption process and the security of the encrypted message.</b></summary>In a block cipher, the size of the key directly determines the size of the blocks into which the plaintext message is divided. If a key is 256 bits long, the message will be divided into blocks of 256 bits each. For example, a 1-megabyte message would be split into many 256-bit blocks. Larger key sizes generally offer <b>stronger security</b> as they create a larger number of possible key combinations, making it exponentially harder for attackers to guess the correct key and decrypt the message. A larger key size also means larger blocks, which can provide greater <b>diffusion</b> and <b>confusion</b>, making it harder to discern patterns in the ciphertext. However, larger key sizes can also require more <b>computational</b> resources and time for encryption and decryption. Therefore, choosing the appropriate key size involves balancing <b>security</b> needs with performance considerations.<br><br></details>

## Links
### Cryptography

- [Keys and Cryptographic Algorithms](https://notes.ryancranie.com/Notes/Cryptography/Keys%20and%20Cryptographic%20Algorithms)


### Revision History
001: 2024-09-25 - Initialized Ciphers.md
002: 2024-10-14 - Created Audio Overview, Glossary, and QnA

---
<b>[Cryptography Contents](https://notes.ryancranie.com/Contents/Cryptography%20Contents)<br>[Home Page](https://notes.ryancranie.com)<br></b>[Ryan Cranie](https://www.ryancranie.com)