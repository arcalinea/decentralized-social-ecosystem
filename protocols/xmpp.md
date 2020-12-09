# XMPP

XMPP, the Extensible Messaging and Presence Protocol, is a protocol designed for instant messaging and generalized routing of XML data. It was originally developed in the Jabber open source community in 1999 to provide an alternative to closed instant messaging services.

XMPP uses a federated client-server architecture that is similar to email, but introduced several modifications to facilitate real-time communication.

XMPP is a large protocol, with many Extension Protocols that have been added over time. Due to the maturity of the protocol, it is well documented, and has many implementations and applications.

### Identity

Every user has a unique XMPP address, called a JID (Jabber ID). A JID is a username followed by the homeserver, for example:

```
alice@example.com
```

Users can choose a [global, memorable nickname,](https://xmpp.org/extensions/xep-0172.html) but these are not globally unique.

### Network

XMPP is implemented in a distributed client-server architecture.

### Data

The basic protocol data unit in XMPP is an XML "stanza", a fragment of XML that is sent over a stream.

An XMPP client may store data on the server. Whether it is accessible to others or not depends on the client implementation.

Default settings for a Jabber server [store information](https://jabber.at/p/privacy/) including: saved contacts ("buddies"), offline messages, error logs, and perhaps chat messages and file uploads for a limited number of days.

### Moderation & Reputation

Due to the federated nature of XMPP, moderation actions are only respected if they are supported by the client.

Some examples of moderation in XMPP:
[An experimental spec for groupchat moderation](https://xmpp.org/extensions/xep-0425.html)

### Social & Discovery

XMPP includes the ability for users to advertise their network availability or "presence." This is not strictly necessary for the exchange of data, but facilitates real-time interaction by indicating the the recipient is online and available. Users can subscribe to each other's presence statuses.

Users have contact lists called "rosters". Rosters are [often stored on the server](https://www.blikoontech.com/tutorials/xmpp-made-simple-roster-and-presence-explained) to protect the user's social graph.

### Privacy & Access Control

XMPP specifies that channel encryption should be used for the XMPP and HTTP channel via SSL or TLS.

The original protocol did not include end-to-end encryption by default. Encryption methods were later added as extensions to the core protocol.

XMPP includes a method for authenticating a stream using the [Simple Authentication and Security Layer (SASL)](https://tools.ietf.org/html/rfc6120#ref-SASL).

### Interoperability

XMPP was designed with the goal of connecting users through multiple instant message systems. This was done through transports or gateways to other IM protocols, but also to protocols such as SMS and email.

In February 2010, the social-networking site Facebook opened up its chat feature to third-party applications via XMPP.[13] Some functionality was unavailable through XMPP, and support was dropped in April 2014.[14]

Similarly, in December 2011, Microsoft released an XMPP interface to its Microsoft Messenger service.[15] Skype, its de facto successor, also provides limited XMPP support.[16]

### Scalability

[A single ejabberd node with 2+ million concurrent users](https://www.process-one.net/blog/ejabberd-massive-scalability-1node-2-million-concurrent-users/)

### Metrics

### Implementations & Applications

A list of [XMPP clients](https://xmpp.org/software/clients.html). XMPP has use cases beyond chat, such as [IoT, gaming, social, and WebRTC](https://xmpp.org/uses/).

Apple and Google [use XMPP for push notifications](https://xmpp.org/uses/social.html).

WhatsApp, Zoom, Grindr, and Jitsi [use XMPP for chat](https://xmpp.org/uses/instant-messaging.html).

### Governance & Business Models

XMPP was developed by the open source Jabber community without funding.

Applications that use XMPP are varied, and have many different business models, from advertising to SASS.

### Links

[XMPP About](https://xmpp.org/about/technology-overview.html)
[XMPP Wikipedia](https://en.wikipedia.org/wiki/XMPP)
[XMPP Core Memo](https://xmpp.org/rfcs/rfc3920.html)
