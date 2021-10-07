# Blindsend

Blindsend is an open source tool for private, end-to-end encrypted file exchange between two agents. It supports two use cases:
1. Uploading a file and obtaining a link for file download. In this case, file Sender initiates the file exchange and provides the download link to file Receiver.
2. Requesting a file and obtaining a link which is transmited to the Sender to use for uploading the requested file. In this case, the Receiver initiates file exchange and provides the upload link to the Sender (think of a doctor, a Receiver, requesting a blood analysis results from a patient, the Sender). The same link is used by the Receiver to download the file.

When exchanging files via blindsend, encryption and decryption always take place on Sender's/Receiver's local machines. A demo is available at [https://blindsend.io](https://blindsend.io).

Blindsend consists of two parts:
1. [Server](https://github.com/blindnet-io/blindsend-server), which provides a REST API for managing file exchange workflows. It is designed in such a way that it can never decrypt exchanged files.
2. [Web UI](https://github.com/blindnet-io/blindsend-fe), which handles encryption and decryption of files on agents' local machines, and provides a web client for the server.

Blindsend is an open source tool and can be deployed as a SaaS application. 

## Installation

To run blindsend server, follow the instructions given in the [server](https://github.com/blindnet-io/blindsend-server) project.  
If you want to add a web client, follow the instructions given in the [Web UI](https://github.com/blindnet-io/blindsend-fe/) project. 

## Considerations

- Server knows the encrypted file size. There should be a way to obfuscate the file size.
- Currently, in the use case when Receiver initiates file exchange by requesting a link, file metadata is not encrypted.

## Current status

Blindsend is under development by a small team of software engineers at [blindnet.io](https://blindnet.io/) and several independant cryptography experts.