# Matrix

Matrix is a protocol for replicating a signed history of JSON objects in realtime across a set of nodes with (optional) end-to-end encryption. It was started in 2014.

### Identity

A Matrix [identifier](https://matrix.org/docs/spec/appendices#common-identifier-format) takes the form of `*localpart:homeserver`, where \* is a “sigil” character which is used to identify the entity’s type. The sigil character "@" states that the entity is a Matrix user ID, and the "localpart" is an identity allocated by that homeserver. For example:

`@bob:matrix.org`

Other sigil IDs include "!" for Room ID, "$" for Event ID, "+" for Group ID, and "#" for room alias.

User accounts, once created on a homeserver, cannot be migrated. To change servers, a user must make a new account. [Automated tooling](https://modular.im/tools/matrix-migration) exists to help with inviting the new account into rooms the previous account was in. There are eventual plans to switch to user keys which will enable [portable identities](https://gist.github.com/neilalexander/90cf1fa6980ade4b3385ebf536f634fe). A Matrix account could also [map to a DID](https://github.com/friedger/matrix-doc/blob/3pid_did_association/proposals/1781-3pid-did-association.md).

Users have a Matrix user ID, but can also use [3rd party IDs (3PIDs)](https://matrix.org/docs/spec/appendices#pid-types). Matrix identity servers map 3rd party IDs such as email addresses and phone numbers to Matrix ids. The use of this service is optional. A globally federated cluster of trusted identity servers verify and replicate the mappings, although this is considered a stopgap solution until a fully decentralized identity solution is adopted.

User IDs used in conversations will soon be decoupled from permanent IDs, allowing one to decorrelate users from their messages.

### Network

Matrix has a federated and a p2p version.

#### Federated

In the federated version of Matrix, all messages are currently sent out full-mesh within a conversation. A node broadcasts in parallel to every other node present in the room. Experimental work on stochastic spanning tree "fan-out" approaches to improve efficiency are being researched.

#### P2p

Matrix has released a p2p version that runs client-side. P2p Matrix avoids the problem of homeservers accumulating metadata, and simplifies signup by not requiring new users to pick a homeserver. The new p2p implementation runs the homeserver on the client. The p2p network is currently separate from the federated network, but the end goal is to connect the two in a hybrid federated/p2p model. Network transports being considered for p2p Matrix include libp2p, Yggdrasil, or hyperswarm. In p2p Matrix, the hostname of the user is its libp2p or yggdrasil public key.

https://matrix.org/blog/2020/06/02/introducing-p-2-p-matrix

### Data

Matrix messages are stored in per-conversation Merkle DAG data structures, and conversations are replicated across all participating servers. Matrix is architecturally most similar to Git.

A Matrix "room" refers to a persistent pubsub topic. Users who want to chat join the same room, and their conversation history is then replicated on each user's server.

### Moderation & Reputation

The Matrix team has been working intensively on tools for moderation, detailed [here](https://matrix.org/docs/guides/moderation/).

Most moderation takes place at the room level. Rooms have moderators who can remove undesirable content. A redaction, or 'remove message', can request all participating servers to remove a message. Users can also remove their own messages. However, due to the decentralized nature of Matrix, if the client does not remove the redaction, the message will not be removed. Moderators can kick or ban users from rooms. They can also ban servers from rooms, in cases where malicious traffic must be avoided.

Users in a room have 'power levels', a number between 0 and 100 that indicates how much power the user has in the room. A higher level has more permissions. Moderators have the ability to change power level permissions. Users can report abusive content to the server admin, which has the ability to remove it. Users can also block other users to prevent them from contacting them.

### Social & Discovery

All conversations on Matrix take place through rooms, which people either join (if public), peek into (if viewable), or are invited to.

Features supporting more advanced social functionality are being developed, such as this proposal for tracking events related to existing events:
[Proposal for Aggregation via Relations](https://github.com/matrix-org/matrix-doc/blob/matthew/msc1849/proposals/1849-aggregations.md)

### Privacy & Access Control

Matrix homeservers have access to metadata about conversations, because the homeservers of all users in a given conversation have to store that conversation's metadata. P2p Matrix mitigates this privacy issue.

Matrix recently introduced end-to-end encryption by default for private messages. This was on the roadmap since the beginning, because conversations are replicated over every server participating in a room, and there is no guarantee against servers looking into conversations. For Matrix E2E encryption, each device has its own identity key, and per-user keys are gossipped between devices when the user logs in.

[olm e2e encryption](https://matrix.org/blog/2016/11/21/matrixs-olm-end-to-end-encryption-security-assessment-released-and-implemented-cross-platform-on-riot-at-last)
[encryption for private messages](https://matrix.org/blog/2020/05/06/cross-signing-and-end-to-end-encryption-by-default-is-here)

All Matrix APIs are protected behind GDPR consent flows. Users can specify history retention per-room, per-server, and per-message.

### Monetization & Governance

Matrix is governed by [The Matrix.org Foundation CIC](https://matrix.org/foundation) (a UK non-profit foundation), with the largest contributor being [Element](https://vector.im) (previously called New Vector), a startup formed by the original Matrix dev team, has raised a total of $18M. Income primarily comes from contracts with governments and large clients. The Matrix standard evolves under the stewardship of a Spec Core Team that manages contributions.

### Interoperability

Matrix can be bridged with IRC, Slack, Discord, Telegram and others.

### Implementations & Applications

Matrix is designed to support multiple communication cases - Chat, VoIP, IoT, VR/AR, Social etc, but so far effort has been concentrated on providing a federated chat experience with good UX at scale.

Matrix supports multiple clients (most notably [Element](https://element.io) (previously called Riot), the flagship app from the core team), and has bridges to many other chat systems (IRC, Slack, Discord, Telegram etc).

The majority of traffic is currently instant messaging.

### Scalability & Metrics

Matrix “rooms” replicate message content across all participating servers, but lazily-replicates file content. A server only retrieves a file if a user on that server requests it.

The public network currently (Feb 2020) has 17.9M known addressable users (as of June 2020), with more in private federations or on servers which don't report stats. 30% of publicly visible users are on the matrix.org homeserver. There are many smaller servers as well.

Scalability study: https://arxiv.org/pdf/1910.06295.pdf

There are over 1,500 active developers in the community.

### Links

- [matrix.org](https://matrix.org/)
