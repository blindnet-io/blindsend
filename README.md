# Blindsend

Blindsend is an open source tool for private, end-to-end encrypted file exchange between two agents.

Blindsend conists of two parts:
1. [Back-end](https://github.com/blindnet-io/blindsend-be), which provides a REST API for managing file exchange workflows. It is designed in such a way that it can never decrypt exchanged files.
2. [Front-end](https://github.com/blindnet-io/blindsend-fe), which handles encryption and decryption of files on agents' local machines, and provides a web client for the back-end.

Blindsend is an open source tool and can be deployed as a SaaS application, or as a stand alone API to be used in external software projects. 

## Installation

To run blindsend stand-alone back-end API, follow the instructions given in the [back-end](https://github.com/blindnet-io/blindsend-be/tree/upd-protocol#installation-instructions) project. If you want to add a web client, follow the instructions given in the [front-end](https://github.com/blindnet-io/blindsend-fe/tree/upd-protocol#installation-instructions) project. 

## Blindsend libraries and clients

An example of Java library for blindsend is available [here](https://github.com/blindnet-io/blindsend-examples-java).

## Demo

Blindsend demo is available at [https://blindsend.xyz/](https://blindsend.xyz/).

## Current status

Blindsend is under development by a small team of software engineers at [blindnet.io](https://blindnet.io/) and several independant cryptography experts. First release to follow soon.

