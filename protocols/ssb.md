# Scalable Secure Scuttlebutt (ssb)

Scalable Secure-Scuttlebutt (ssb) is a distributed gossip protocol designed for social sharing. Identities are cryptographic key pairs, feeds are a signed append-only log sequence of messages, and nodes use a gossip protocol to disseminate content. Feeds can be thought of as essentially personal blockchains, as they consist of immutable, timestamped content.

Ssb is based on the idea that your social network mirrors your actual communication network, and your network peers mirror your actual peers. Ssb focuses more on moving lightweight social data rather than large data, unlike protocols like BitTorrent, Hypercore, and IPFS. Ssb's design philosophy avoids [centralization and singletons](https://handbook.scuttlebutt.nz/stories/design-challenge-avoid-centralization-and-singletons.html), and strives for a maximally distributed architecture. Users are distributed across a few different client apps that work on desktop and mobile.

### Identity

A user's identity is their ed25519 key pair which is used to sign posts, verifying their authenticity. Messages are addressed to a users public key, for example:
`@3QHXrXl762sf7P/Q1RMtscA7IRipfUFnE5tpie5McvE=.ed25519`

Users can pick a human-readable nickname that is associated with their key, but nicknames are not unique because there is no global registry.

Ssb does not currently support multi-device login, because keys are stored on devices.

### Network

Nodes request all messages in the feed that are newer than the latest message they know about. The networking component of SSB maintains a table of known peers which it cycles through asking for updates for all followed feeds. Messages are passed through the ssb network via a gossip protocol. Messages may be passed through third parties, which improves availability.

Pubs, bot-user nodes with public IP addresses that stay online, ensure uptime and availability. Pubs are essentially the bootstrap nodes and mail-bots of ssb. Pubs offer invite codes to new users, follow users, and rebroadcast messages to other peers. Ssb has no NAT-traversal utilities, so users connect to a Pub to distribute their messages. Users can also sync over LAN. Identity is not tied to pubs, unlike homeservers in Matrix or ActivityPub, so a user can join one or multiple pubs. A [scuttlebutt DHT invite](https://gitlab.com/staltz/ssb-dht-invite) plugin that shares connection invites over a DHT was created in 2018.

### Data

Each post is appended to the last in a linked list append-only log establishing chronological ordering from the first post. Once appended, posts are immutable. Content cannot be easily edited or deleted, but the append-only log ensures that the network converges towards the same state despite partitions.

When a follow relationship is initiated, the posts of the user being followed begins to be synced to the follower's node. Those messages and files are stored locally on the user's computer, indefinitely, for applications running ssb to read. An ssb application is constantly sharing data with other nodes in the background.

Each message contains:

- A signature
- The signing public key
- A content-hash of the previous message
- A sequence number
- A timestamp
- An identifier of the hashing algorithm in use (currently only "sha256" is supported)
- A content object

Because of the append-only nature of ssb feeds, there is no ability to permanently delete a piece of content. Applications can work around this by honoring edit or delete messages appended to the feed, but the original content stays in the append-only log that is shared among all nodes, and other applications could choose not to honor such messages. An example of a workaround is [ssb-revisions](https://github.com/regular/ssb-revisions), a basic API that enables applications to use mutable messages by displaying the updated version.

### Moderation & Reputation

There is no global moderation, and no specialized moderators in ssb. A “flag” message is used to send a strong negative signal about bad actors. Applications built on top of ssb allow users to “block” and “ignore”. An ignore will simply not show that data to the user's node, although their node will continue to pass their data through the network. A block will cause the user's node to refuse to replicate data from that feed, segmenting it off from their portion of the network. If enough people block a user or group of users, their part of the network will become partitioned from the rest.

### Social & Discovery

There is no global feed of content in ssb. All content is surfaced through social discovery. Out-of-band sharing, sending an ssb link through another channel, can also surface new content.

Ssb clients decide the number of hops away from primary follow relationships to store or replicate data. For example, a client could store data from 2 hops away, but replicate data from 3 hops away to keep it available for others, but not show it in the user interface.

### Privacy & Access Control

Ssb applications can easily support encrypted DMs (with up to 7 participants) because user identities are cryptographic keypairs. Whoever controls the private key of an identity can publish to that feed. Messages canot be faked, omitted, or re-ordered, due to the signed append-only log nature of the feed.

There is ongoing research and development on providing ["Facebook Groups" style of access control](https://github.com/ssbc/ssb-private2) to larger private groups (in the dozens/hundreds of users).

### Governance & Business Models

The ssb ecosystem is supported through a variety of grants, donations, income from side projects and consulting, and a few companies that have raised money to build applications on ssb, including [Ahau](https://ahau.io/) and [Planetary](https://planetary.social/).

Pubs, the most resource-intensive nodes, are currently volunteer supported.

### Interoperability

[Ssb viewer](https://github.com/ssbc/ssb-viewer), an HTTP server for read-only views of ssb content, brings read-only interop from ssb to the web.

Ssb applications generally do not bridge to other applications. A proof-of-concept experiment involving [cross-posting to Twitter](https://github.com/arcalinea/twitter-ssb-import) and importing tweets into ssb demonstrates the possibility for a simple interop.

There has been community [discussion of using IPFS for blob data storage in ssb](https://github.com/ssbc/ssb-server/issues/454), but it has not been implemented as a feature.

### Scalability

New users add capacity to the network as they join as their nodes participate in hosting and sharing content.

A growing number of [room servers](https://github.com/staltz/ssb-room) help with scalability since rooms host no data, but allow tunnel connections between clients, so the more clients there are, the more connections there are available.

Pub servers would need to be expanded to keep up with a sudden influx of new users.

A potential scalability issue is the size of the append-only log feeds stored on a user's device growing over time.

### Metrics

[According to a network crawl](https://twitter.com/andrestaltz/status/1191821144040574984) run by a developer in Nov 2019, there were around 16,000 nodes on ssb visible to his node.

### Implementations & Applications

A list of [applications built on ssb](https://handbook.scuttlebutt.nz/applications).

Social application clients on the ssb network include:

- [Patchwork](https://handbook.scuttlebutt.nz/applications#patchwork), a desktop application
- [Patchbay](https://github.com/ssbc/patchbay), a fully compatible alternative to Patchwork
- [Oasis](https://github.com/fraction/oasis), a desktop application
- [Feedless](https://github.com/rogeriochaves/feedless) an iOS application
- [Manyverse](https://handbook.scuttlebutt.nz/applications#manyverse), a mobile application
- [Planetary](https://planetary.social/), a mobile application

Other applications include:

- [git-ssb](https://handbook.scuttlebutt.nz/applications#git-ssb), a git interface using ssb
- [Ticktack](https://handbook.scuttlebutt.nz/applications#ticktack), a blog publishing app with private messaging
- [Infinite Game](https://handbook.scuttlebutt.nz/applications#infinite-game), a calendar, event tool, and time picker.

### Links

- [Overview](https://scuttlebot.io/more/protocols/secure-scuttlebutt.html)
- [Ssb concepts](https://handbook.scuttlebutt.nz/concepts/)
- [Dark Crystal ssb protocol docs](https://darkcrystal.pw)
- [3box comparison of p2p DBs: GUN, OrbitDB, Scuttlebutt](https://medium.com/3box/3box-research-comparing-distributed-databases-gun-orbitdb-and-scuttlebutt-2e3b5da34ef3)
