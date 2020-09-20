# Blindsend

Blindsend is an open source tool for private, end-to-end encrypted file exchange between two agents.  

Currently, there are two use cases:
- uploading a file and providing a link to download the file
- making a file request where requesting party obtains a link which is used to upload a file (think of a doctor requesting a blood analysis results from a patient)

Blindsend demo is available at [https://blindsend.work/](https://blindsend.work/).

Blindsend conists of two parts:
1. [Server](https://github.com/blindnet-io/blindsend-be), which provides a REST API for managing file exchange workflows. It is designed in such a way that it can never decrypt exchanged files.
2. [Web UI](https://github.com/blindnet-io/blindsend-fe), which handles encryption and decryption of files on agents' local machines, and provides a web client for the back-end.

Blindsend is an open source tool and can be deployed as a SaaS application, or as a stand alone API to be used in external software projects. 

## Installation

To run blindsend stand-alone back-end API, follow the instructions given in the [back-end](https://github.com/blindnet-io/blindsend-be/) project.  
If you want to add a web client, follow the instructions given in the [front-end](https://github.com/blindnet-io/blindsend-fe/) project. 

## Protocol

In the first use case, file sender types a password and uploads a file to obtain the download link. Link is transmitted to receiving party through any authenticated channel (basically any popular chat app) and the password is communicated (ideally through some other channel). If the password is left blank, make sure the channel is e2e encrypted so the third party having access to communication channel contents can't read the link. Receiving party, upon opening the link, types the password and downloads and decrypts the file.  

URI fragment (part of a link after hash symbol) does is not sent to the server so it can be used to store the secret data like encryption keys. This technique is used in the protocol.

High level protocol:
- The file sender generates a random seed using a secure randomness API.
- This seed is sent through a HKDF to obtain the fileMetadata encryption key.
- A random salt is generated.
- The password is sent through PBKDF using the salt to obtain seed2.
- seed1 and seed2 are concatenated, hashed and sent through HKDF to obtain the file ecryption key.
- File is encrypted with the file encryption key and a random nonce1.
- File metadata is encrypted using the fileMetadata key and a random nonce2.
- After providing the two ciphertexts (together with salt, nonce1 and nonce2) to the server, the server associates a UUID with them and stores them in a database.
- The file sender then creates a link composed of the UUID and secret seed (the latter of which is base64 encoded and stored in the uri fragment) and sends it using an authenticated channel to the file recipient.
- The file recipient extracts the uuid and seed from the link, and uses the uuid to request the ciphertexts.
- Recipient regenerates the encryption keys in the same way, and use it to decrypt the metadata and the file.
```
--------------------------------------------------------------------------------------
File Sender                         Server                              File Recipient
                            
seed1       = random()
fileMetaKey = HKDF(seed1, "fileMetadata")
salt        = random()
seed2       = PBKDF(salt, password)
fileKey     = HKDF(H(seed1 || seed2), "file")

nonce1 = random()
nonce2 = random()
encMetadata = E(fileMetaKey, nonce1, fileMetadata)
encFile = E(fileKey, nonce2, file)


{salt, nonce1, nonce2, encMetadata, encFile} ------------>

                                uuid = uuid()
                                Store {uuid, salt, nonce1, nonce2, encMetadata, encFile}

             <---------------- { uuid }


Generate link:
https://.../{uuid}#{seed1}


{link}
  |
  ------------------------------ Out of band channel --------------------------------|

                                                                            Extract uuid, seed1

                                           <-------------------------------- {uuid}

                                           {salt, nonce1, nonce2, encMetadata, encFile} ----------->

                                                                        fileMetaKey = HKDF(seed1, "fileMetadata")
                                                                        fileMetadata = D(fileMetaKey, nonce1, encMetadata)

                                                                            Prompt for password

                                                                        seed2       = PBKDF(salt, password)
                                                                        fileKey     = HKDF(H(seed1 || seed2), "file")
                                                                        file = D(fileKey, nonce2, encFile)


--------------------------------------------------------------------------------------
```

In the second use case
The requesting party (which is also a file receiver) providing a password to generate a file exchange link via blindsend, and transmits the link to the file Sender through any authenticated channel. The Sender then uses the link to encrypt and upload a file. After successful upload, the Receiver opens the link and uses the password to download the encrypted file. Once downloaded, the file is decrypted locally on Receiver's machine.

High level protocol:
- Link id is obtained from the server.
- A random salt is generated.
- The file requester submits a password which is sent through PBKDF using the salt to obtain a seed.
- A random key pair PKa, SKb is generated using the seed.
- The salt is sent to the server and link https://.../{link_id}#{PKa} is generated. Link is sent using an authenticated channel to the file sender.
- The file recepient opens a link and extracts PKa.
- The file recepient generates a key pair PKb, SKb.
- An encryption key is generated using PKa, PKb and SKb with Diffie-Hellman algorithm.
- A random nonce is generated and the file is encrypted used the encryption key and nonce.
- Encrypted file content, nonce and PKb are communicated to the server together with file metadata.
- The file requester opens a link and extracts PKa.
- Salt, PKb and nonce are fetched from the server.
- The file requester submits a password which is sent through PBKDF using the salt to obtain a seed.
- A random key pair PKa, SKb is generated using the seed.
- Generated PKa is checked against PKa extracted from the link
- If they match, an encryption key is generated using PKb, PKa and SKa with Diffie-Hellman algorithm.
- Encrypted file is obtained from the server and decrypted using the encryption key and nonce.

## Blindsend clients

An example of Java front-end library for blindsend is available [here](https://github.com/blindnet-io/blindsend-examples-java).

## Concerns

- Server knows the encrypted file size. There should be a way to obfuscate the file size.
- In a case server is compromised, it can serve malicious javascript code to the clients and obtain the decrypted file after decrypting in locally. Users should have a way to authenticate the received javascript.
- Currently, in the second use case, file metadata is not encrypted.

## Current status

Blindsend is under development by a small team of software engineers at [blindnet.io](https://blindnet.io/) and several independant cryptography experts. First release to follow soon.

### TODOS
- add more storage options
- add authentication
- implement file retention policy
- encrypt file metadata in the second use case
- tests
- code and design documentation
- document crypto primitives used in protocol