# Blindsend

Blindsend is an open source tool for private, end-to-end encrypted file exchange between two agents. Current use case requires a requesting party (file Receiver) to initiate the file exchange by generating a link via blindsend and transmiting that link to the file Sender; after the Sender uploads the file to blindsend, the Receiver is able to use the same link to download the file. 

When exchanging files via blindsend, encryption and decryption always take place on Sender's/Receiver's local machines. To learn more about blindsend and how it works, read our documentation [here](https://developer.blindnet.io/blindsend/). A demo is available at [https://blindsend.xyz](https://blindsend.xyz).

Blindsend conists of two main parts:
1. [Server](https://github.com/blindnet-io/blindsend-be), which provides a REST API for managing file exchange workflows. It is designed in such a way that it can never decrypt exchanged files.
2. [Web UI](https://github.com/blindnet-io/blindsend-fe), which handles encryption and decryption of files on agents' local machines, and provides a web client for the server.

Appart from Server and Web UI, there is a separate [file retention service](https://github.com/blindnet-io/file-retention) in charge of file and link deletion.

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