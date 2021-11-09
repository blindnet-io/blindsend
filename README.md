# Blindsend

blindsend is an open source tool for private, [end-to-end encrypted](https://en.wikipedia.org/wiki/End-to-end_encryption) file exchange.   

It supports two use cases - [sharing files](https://github.com/blindnet-io/blindsend#sharing-files) and [requesting files](https://github.com/blindnet-io/blindsend#requesting-files).  

A demo can be found on https://blindsend.io.

## Sharing files

You will start by uploading files and obtaining a link to download. You can then share the link with anyone and they will be able to download the files.  

We call this the **Share file** use case.

If you are sharing the link via an unsecure channel like facebook messenger, a password can be set to additionally improve the security.  You should share the password using a different secure channel, for example, in person.

## Requesting files

If you need someone to send you a file, you will generate a request link and send it to that person. They will open the link and upload the requested files, after which you can access the same link to download the files.

We call this the **Request file** use case.

Similar to the [sharing files](https://github.com/blindnet-io/blindsend#sharing-files) use-case, you can set a password which needs to be input again when downloading files.

This use-case is suitable for various professional services, such as doctors asking for blood results or a lawyer asking for a subpoena.  
Traditionally, those documents were shared using an insecure channel such as email.

## Architecture

Blindsend consists of four parts:
1. [Server](https://github.com/blindnet-io/blindsend-server), which provides the REST API for managing file exchange workflows.
1. [Web UI](https://github.com/blindnet-io/blindsend-fe), which handles encryption and decryption of files on agents' local machines, and provides a web client.
1. [Cloud Storage](https://cloud.google.com/storage) on the [Google Cloud platform](https://cloud.google.com/) where the encrypted files are stored.
1. [PostgreSQL](https://www.postgresql.org/) where link data is stored.

## Security

Files uploaded to blindsend are encrypted using an [end-to-end encrypted](https://en.wikipedia.org/wiki/End-to-end_encryption) protocol, meaning neither blindsend nor any third party can decrypt them.

Only the persons possessing the link (and an optional password) can decrypt the files.  
It is important to share the link using an authenticated channel, meaning the link wasn’t changed during the transfer and the other party receives the same link you sent them.  
If the channel is not secure, a password can be set to prevent the third party from decrypting the files.

To keep sensitive information away from the blindsend servers, links use the [URL fragments](https://en.wikipedia.org/wiki/URI_fragment). They are the parts of the URL after the # symbol which are not sent to the server when the URL is opened in the browser.

Protocol diagrams are coming soon!

## Considerations

1. Currently, the files are deleted after 7 days. 
    - Add the option to set custom file expiration date.
    - File deletion policy can be based on the number of times the file was opened.
1. Specify your email address to receive the access logs.
1. Specify another user’s email address who will receive a link.

## Current status

blindsend is under development by a team of software engineers at [blindnet.io](https://blindnet.io) and several independent cryptography experts.

## License

blindsend has the MIT license. You can read this license in the [LICENSE](https://github.com/blindnet-io/blindsend/blob/develop/LICENSE) file.  
You are free to deploy your own instances. Contact us if you need any help at _blindsend@blindnet.io_