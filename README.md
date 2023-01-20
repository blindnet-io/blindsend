<h1 align="center">
  blindsend
</h1>

<!-- IF branding -->

<p align=center><img src="https://user-images.githubusercontent.com/7578400/163277439-edd00509-1d1b-4565-a0d3-49057ebeb92a.png#gh-light-mode-only" height="80" /></p>
<p align=center><img src="https://user-images.githubusercontent.com/7578400/163549893-117bbd70-b81a-47fd-8e1f-844911e48d68.png#gh-dark-mode-only" height="80" /></p>

<!-- END IF branding -->

<p align="center">
  <strong>End-to-end encrypted file sharing</strong>
</p>

<p align="center">
  <a href="https://blindsend.io"><strong>blindsend.io</strong></a>
</p>

<p align="center">
  <a href="https://github.com/blindnet-io/blindsend-web-client">Web client</a>
  &nbsp;•&nbsp;
  <a href="https://github.com/blindnet-io/blindsend-server">Server</a>
  &nbsp;•&nbsp;
  <a href="https://github.com/blindnet-io/blindsend/issues">Submit an Issue</a>
  <br>
  <br>
</p>

## About

blindsend is an open source tool for private, [end-to-end encrypted](https://en.wikipedia.org/wiki/End-to-end_encryption) file exchange.   

It supports two use cases - [sharing files](https://github.com/blindnet-io/blindsend#sharing-files) and [requesting files](https://github.com/blindnet-io/blindsend#requesting-files).  

You can find the protocol description in the [protocol.pdf](./protocol.pdf) file.

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
1. [Web UI](https://github.com/blindnet-io/blindsend-web-client), which handles encryption and decryption of files on agents' local machines, and provides a web client.
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

## Community

> All community participation is subject to blindnet’s [Code of Conduct][coc].

Stay up to date with new releases and projects, learn more about how to protect your privacy and that of our users, and share projects and feedback with our team.

- [Join our Slack Workspace][chat] to chat with the blindnet community and team
- Follow us on [Twitter][twitter] to stay up to date with the latest news
- Check out our [Openness Framework][openness] and [Product Management][product] on Github to see how we operate and give us feedback.

## License

The blindsend-server is available under [MIT][license] (and [here](https://github.com/blindnet-io/openness-framework/blob/main/docs/decision-records/DR-0001-oss-license.md) is why).
You are free to deploy your own instances. Contact us if you need any help at _blindsend@blindnet.io_

<!-- project's URLs -->
[new-issue]: https://github.com/blindnet-io/blindsend/issues/new/choose
[fork]: https://github.com/blindnet-io/blindsend/fork

<!-- common URLs -->
[openness]: https://github.com/blindnet-io/openness-framework
[product]: https://github.com/blindnet-io/product-management
[request]: https://github.com/blindnet-io/devrel-management/issues/new?assignees=noelmace&labels=request%2Ctriage&template=request.yml&title=%5BRequest%5D%3A+
[chat]: https://join.slack.com/t/blindnet/shared_invite/zt-1arqlhqt3-A8dPYXLbrnqz1ZKsz6ItOg
[twitter]: https://twitter.com/blindnet_io
[docs]: https://blindnet.dev/docs
[changelog]: CHANGELOG.md
[license]: LICENSE
[coc]: https://github.com/blindnet-io/openness-framework/blob/main/CODE_OF_CONDUCT.md
