# ActivityPub

ActivityPub is a federated protocol that defines a set of interoperable social network interactions through specific APIs. Any server that implements this protocol can communicate with the rest of the network. It reached W3C recommendation status in 2018. It is one of several related specs produced by the [Social Web Working Group](https://www.w3.org/wiki/Socialwg).

ActivityPub consists of two layers: A server-to-server federation protocol, and a client-to-server protocol. In order to federate with the ActivityPub ecosystem, a service only has to implement the server-to-server protocol.

### Identity

Users in ActivityPub are conceptualized as actor objects. Actor to actor communication bears a resemblance to email. To be spec compliant, each actor _must_ have an "inbox" and an "outbox" endpoint, as URLs which are accessible on the server. They also _should_ have "following" and "followers". They _may_ have "liked" collections, and many other predefined possibilities.

Although not part of the ActivityPub spec, in practice Webfinger is used to discover actor profiles.

Server to server federation is [authenticated](https://www.w3.org/wiki/SocialCG/ActivityPub/Authentication_Authorization) using HTTP Signatures. The server creates a public and private keypair for each actor, and a publicly accessible JSON-LD document retrievable over HTTP which contains its public key. Each message the server sends on behalf of an actor is signed by its key. When a remote server receives a POST to its inbox, it verifies the signature on the HTTP request. To verify object integrity, linked data signatures are used to sign the object with the publicKey of the actor who authored it.

A [paper](https://github.com/WebOfTrustInfo/rwot5-boston/blob/master/final-documents/activitypub-decentralized-distributed.md) from the 2017 Rebooting the Web of Trust conference describes how distributed, cryptographic identities could be added to ActivityPub.

### Networking

ActivityPub is a federated server-to-server protocol that passes messages between systems.

ActivityPub servers do not proactively network with each other, so they are unaware of each other's presence until a user finds and follows someone on another server. Servers maintain a list of remote accounts its users follow and subscribe to their posts.

ActivityPub messages are not limited to HTTP only. This allows it to potentially be extended in more [p2p directions](https://nbviewer.jupyter.org/github/WebOfTrustInfo/rebooting-the-web-of-trust-fall2017/blob/master/final-documents/activitypub-decentralized-distributed.pdf).

### Data

ActivityPub messages are objects wrapped in an "activity", indicating what it is. There is an [Activity Vocabulary](https://www.w3.org/TR/activitystreams-vocabulary/) that defines Activity, Object, and Actor types common to social web applications.

ActivityPub is not opinionated about how messages are persisted on the server as long as each server follows the protocol message requirements. Server implementions may [cache](https://flak.tedunangst.com/post/what-happens-when-you-honk) frequent requests, such as follower actor objects, public keys of other servers, and images and attachments on posts.

### Moderation & Reputation

Moderation is primarily handled by server implementations. Server admins can block individuals or entire instances. Banning instances can lead to the isolation of an ActivityPub server instance if it is banned by many others, limiting its users to communication with each other.

ActivityPub defines a "block" activity to help users control their experience.

ActivityPub adoption has reached a threshold where spam and harassment have become ongoing problems that protocol developers currently seek to address. [Keeping Unwanted Messages off the Fediverse](https://github.com/WebOfTrustInfo/rwot9-prague/blob/master/topics-and-advance-readings/ap-unwanted-messages.md#org2158b95) contains a list of suggested solutions. [OCapPub](https://gitlab.com/spritely/ocappub/blob/master/README.org), a proposed object-capability based upgrade of ActivityPub, is a direction being pursued by one of the ActivityPub authors.

### Social & Discovery

Messages are addressed to a user at their home server, or published to a public inbox. Normal DNS and IP address routing are used to find the server addressed.

If posts are limited in visibility (followers only, direct message), they will be delivered to a user's inbox, such as `https://example.com/users/alice`. The "outbox" is a URL where an actor's recent activities can be retrieved from.

Servers also may accept delivery of messages addressed as 'public' to a shared inbox available to all on the server, but are not required to. Social network implementations with public feeds may publish posts to the public inbox, such as `https://example.com/inbox`.

"Like"s and "Follow"s may be used by servers to determine which public messages to accept/retrieve.

There is no global search capability, as each server monitors a different set of messages.

### Privacy & Access Control

Mastodon is currently [adding e2e encryption to ActivityPub](https://github.com/tootsuite/mastodon/pull/13820). Previously, messages were unencrypted on the server.

### Interoperability

Any service that implements the ActivityPub server-to-server protocol can interoperate with the ecosystem. A service like Twitter would need to add Webfinger and JSON-LD representations of users and tweets.

The client-to-server protocol defines a standard way for user client software to connect to ActivityPub servers. In practice, it is rarely used. None of the major Fediverse services implement it. The vision of how it would work if it were widely used is that a user application could mix and match different servers like Mastodon, Pleroma, PixelFed, and any new service that implemented it.

[Bridgy Fed](https://github.com/snarfed/bridgy-fed) is a project to connect IndieWeb sites with the ActivityPub and OStatus federated networks.

[Diaspora](../applications/diaspora.md), another federated social network, chose not to adopt ActivityPub.

### Scalability

The ActivityPub ecosystem scales up by adding more server capacity to the network. This [study on the Mastodon ecosystem](https://emilianodc.com/PAPERS/mastodonIMC19.pdf) analyzes the emergence of points of centralization as the network scales up.

### Metrics

[fediverse.network](https://fediverse.network/) maintains statistics of the known oStatus/ActivityPub fediverse.

### Implementations & Applications

[W3C Implementation Report](https://activitypub.rocks/implementation-report/)
[Watchlist for ActivityPub Apps](https://git.feneas.org/feneas/fediverse/-/wikis/watchlist-for-activitypub-apps)

Notable applications:

- [Mastodon](https://mastodon.social/about) (the largest federated network built on ActivityPub) has 2699 nodes and 2.6M users as of 5/2020
- [Pleroma](https://pleroma.social/) is another federated social network. According to stats at [the-federation.info](the-federation.info), Pleroma has 620 nodes with 35K users as of 5/2020.
- [PixelFed](https://pixelfed.org/) is an ActivityPub based image-sharing platform.
- [Friendica](https://friendi.ca/) is a decentralized social network with support for ActivityPub, as well as the OStatus and diaspora protocols.
- [PeerTube](https://joinpeertube.org/) is a free and decentralized video platform.
- [Plume](https://joinplu.me/) is a federated blogging application

### Related

ActivityPub inherits from a few other protocols that will not be covered in full, but are briefly summarized here.

ActivityPub was based on pump.io and ActivityStreams. Conceptually, these were preceded by OStatus. Pump.io is an activity streams social networking server. [OStatus](https://en.wikipedia.org/wiki/OStatus) is an open standard for federated microblogging, and describes how a suite of protocols can be used together.

The IndieWeb protocols and community are also related to ActivityPub through a shared vision of social federation developed around the same time period. The IndieWeb was [inspired by the Federated Social Web Summit in 2010](https://www.manton.org/2019/07/08/interview-with-indieweb.html), and formed around the idea of interconnecting individual websites rather than federating social platforms. A co-founder described the vision as, "someone should be able to take their web site and be able to use their web site to participate in the same distributed social network — federated social network."

### Links

- [W3C ActivityPub Spec](https://www.w3.org/TR/activitypub/)
- [SocialHub, ActivityPub discussion forum](https://socialhub.activitypub.rocks/)
- [Notes from an ActivityPub implementator](https://flak.tedunangst.com/post/ActivityPub-as-it-has-been-understood)
- [Reading ActivityPub](https://tinysubversions.com/notes/reading-activitypub/)
