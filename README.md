# Blindsend

Blindsend is an open source tool for private, end-to-end encrypted file exchange between two agents. Blindsend works with the following workflow:
1. A file requesting party (which is also a file Receiver) provides a password to generate a file exchange link via blindsend
2. The Receiver transmits the link to the file Sender
3. The Sender uses the link to upload the file, which is first encrypted on Sender's local machine before it is uploaded to blindsend
4. After successful upload, the Receiver uses the password and the same link to download the encrypted file. Once downloaded, the file is decrypted locally on Receiver's machine.

A demo is available at [https://blindsend.xyz](https://blindsend.xyz).

Blindsend conists of two parts:
1. [Server](https://github.com/blindnet-io/blindsend-be), which provides a REST API for managing file exchange workflows. It is designed in such a way that it can never decrypt exchanged files.
2. [Web UI](https://github.com/blindnet-io/blindsend-fe), which handles encryption and decryption of files on agents' local machines, and provides a web client for the server.

Blindsend is an open source tool and can be deployed as a SaaS application, or as an API to be used in external software projects. 

## Installation

To run blindsend server, follow the instructions given in the [server](https://github.com/blindnet-io/blindsend-be/) project.  
If you want to add a web client, follow the instructions given in the [Web UI](https://github.com/blindnet-io/blindsend-fe/) project. 

## What's coming next?

We are currently working to include a new use case scenario in which file the Sender initiates the file exchange and sends the download link to the Receiver.

## Blindsend clients

An example of Java client library for blindsend is available [here](https://github.com/blindnet-io/blindsend-java-lib).

## Considerations

- Server knows the encrypted file size. There should be a way to obfuscate the file size.
- Currently, in the use case when Receiver initiates file exchange by requesting a link, file metadata is not encrypted.

## Current status

Blindsend is under development by a small team of software engineers at [blindnet.io](https://blindnet.io/) and several independant cryptography experts. First release to follow soon.