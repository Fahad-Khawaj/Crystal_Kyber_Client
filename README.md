# Crystal_Kyber_Client

Kyber is a post-quantum cryptographic algorithm designed to secure communications in a world where quantum computers could break traditional encryption methods. Developed as part of the NIST Post-Quantum Cryptography Standardization Project, Kyber ensures that data remains secure against the computational power of future quantum computers.

This repository contains the client-side implementation of the Kyber post-quantum cryptographic algorithm. The client side is responsible for initiating secure communications, generating public/private key pairs, and encrypting messages to be sent to the server.

# Purpose:
To securely generate and manage public/private key pairs.

![Client-Server Interaction](Kyber_WorkFlow.png)

The provided picture illustrates the interaction between the client-side and server-side implementations of Kyber using public key encryption and the KEM (Key Encapsulation Mechanism). The client initiates the communication by generating key pairs and encrypting data(C) cipher containing (encapsulated key) using public key of the server, after encryption which is then securely transmitted to the server. The server receives the encrypted (C) cipher data containing (encapsulated symmetric Key), the server side decrypts it using private key of their own, and processes the request, ensuring a secure end-to-end communication channel to get the same key on both ends. This setup is designed to safeguard against potential threats posed by quantum computing advancements, ensuring long-term data security.

## Installation
Instructions for setting up the server-side application.

## Usage
Using Node.js (v16.17.0):
```
npm install crystals-kyber
```
Import the module at the top of your index.js file.

```
const kyber = require('crystals-kyber');
```
To use in your code (768 can be replaced with 512 or 1024).
```
module.exports = {...require("./kyber512"), ...require("./kyber768"), ...require("./kyber1024")};  
const kyber = require('crystals-kyber');   //importing modules

console.time("Total Execution Time");  //Calculating total execution time of program

console.time("Key Generation");
// To generate a public and private key pair (pk, sk)
let pk_sk = kyber.KeyGen768();

let pk = pk_sk[0];
let sk = pk_sk[1];
console.timeEnd("Key Generation");     //calculating key generation time
console.log("Public Key------------",pk,"-----------");

console.log("\nSecret Key------------",sk,"-----------");

const pkReceivedBase64 = "Paste Public Key of Server side here in base64 form";    //paste the publick of the server side here in base 64 form

const pkReceived = Uint8Array.from(Buffer.from(pkReceivedBase64, 'base64'));   //reading and converting the received base64 key into buffer array
console.log("recivied key ---------",pkReceived);

console.time("Encryption");

let c_ss = kyber.Encrypt768(pkReceived);    //encryption on using public key of the server
let c = c_ss[0];
console.timeEnd("Encryption");

console.log("encapsulated key (cipher text) --------------",c);  //displaying the encrypted encapsulated C
const cBase64 = Buffer.from(c).toString('base64');    //converting the C buffer array into base 64 values 
console.log("Encapsulated Key (Base64) --------------", cBase64);  //Copy this base64 value and send (copy paste) it on the server side to be decrypted
let ss1 = c_ss[1];

console.log("SS1-------",ss1); //displaying the original 32 byte symmetric key hidden inside the C to compare the values on server side after decryption.

console.timeEnd("Total Execution Time");
```

to run the code write in console 
```
node index.js
```
## Further details about Kyber.
Crystal Kyber [https://pq-crystals.org/kyber/]
and this code was obtained from public repository of (Crystal Kyber) [https://github.com/antontutoveanu/crystals-kyber-javascript] 
