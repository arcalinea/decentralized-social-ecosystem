# Diaspora

Diaspora is a federated social network released in 2010. It uses a server to server federation protocol, and is compatible with Friendica and Hubzilla.

Diaspora nodes, called "pods", are hosted by different individuals and institutions. User accounts, which are tied to pods, are called "seeds".

### Identity

Users joining Diaspora pick a pod to register their identity with. User identities contain the username, the hostname, and the port if their server does not listen on the default ports. An example username:

`alice:example.org`

Diaspora uses [Webfinger](https://tools.ietf.org/html/rfc7033) to discover users from other pods. User information is returned via hCard, an open microformat standard for identity.

It is not possible to move a user account to another pod once created.

### Network

Messages sent between servers are serialized to XML, then signed using the [Salmon Magic Signatures](https://cdn.rawgit.com/salmon-protocol/salmon-protocol/master/draft-panzer-magicsig-01.html) protocol.

### Data

User data in Diaspora is tied to their pod server. A user can export all of their data at any time, to reduce the risk of their pod going down.

### Moderation & Reputation

Like Mastodon, moderation is done at the server level. When ISIS joined the Diaspora network in 2014 after they were censored by Twitter, all the larger pods moved to block their server.

In this [Github issue about admin reports](https://github.com/diaspora/diaspora/issues/7316), it is possible to see how communities struggle when tools for moderation are not built into federated networks from the start. This user requested a way to forward spam reports to the source pod, as that was the only way to remove content from the entire network. Without this feature, pod administrators resorted to manually searching for and contacting other administrators.

### Social & Discovery

Like Twitter, Diaspora uses hashtags and mentions for content discovery.

### Privacy & Access Control

Profiles in Diaspora can be public or private.

Private messages in Diaspora are encrypted.

Diaspora allows users to group their contacts into "aspects". Limited posts will only be shown to contacts in the selected aspect.

Communication between pods is encrypted, but data stored on pods is not. Administrators have access to all profile and post data.

### Monetization

Diaspora was initially funded through a kickstarter that raised $200,000. It has not developed a business model.

### Interoperability

Friendica instances are a part of the diaspora network, and natively support the Diaspora protocol.

Diaspora posts can be propagated to accounts on [WordPress, Twitter, and Tumblr](https://wiki.diasporafoundation.org/Integrating_other_social_networks).

Diaspora [has not integrated with ActivityPub](https://discourse.diasporafoundation.org/t/lets-talk-about-activitypub/741). A Diaspora developer [reasoned](https://schub.wtf/blog/2018/02/01/activitypub-one-protocol-to-rule-them-all.html) that although ActivityPub tried to make an extensible protocol that could work for everything, it still did not cover some Diaspora use cases. A stricter specification that could be expand definitions as use cases were offered would be better for ensuring interoperability, in his opinion.

### Metrics

[Number of Diaspora users](https://fediverse.party/en/diaspora)

### Links

- [Diaspora Federation Protocol](https://diaspora.github.io/diaspora_federation/)
- [Diaspora Discourse Forum](https://discourse.diasporafoundation.org/)
