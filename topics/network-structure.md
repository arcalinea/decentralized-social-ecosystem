# Network Structure

### Federated networks

Federated approaches to decentralized network architectures can be categorized as federation where you pass messages between systems (SMTP, XMPP, ActivityPub), and federation where you replicate data between systems (Git, Matrix, NNTP).

#### Passing messages between systems

Federated networks that pass messages between systems include SMTP (email), XMPP (chat), and ActivityPub (Mastodon). This is a straightforward approach for systems mainly concerned with one-to-one communications, such as email and chat, because messages do not get replicated across servers that do not concern them.

ActivityPub, a federated social protocol that passes messages between participating servers, is used in one-to-many social applications like Mastodon, a decentralized Twitter alternative. Unlike Twitter, there is [no global shared state across all servers](https://docs.joinmastodon.org/user/network/), so there is no way to browse all public posts. The federated timeline shows public posts that the user's server knows about. The majority of posts surfaced are from people other users of the server follow.

[Delta Chat](https://delta.chat/en/) is a chat application that uses email servers to send messages, via Push-IMAP.

#### Replicating data between systems

Matrix is a federated protocol that replicates data between systems. Messages in Matrix are replicated over all the servers whose users are participating in a given conversation - similarly to how commits are replicated between Git repositories. The conversation history of the room being replicated across servers allows nodes to resync to the conversation if they go down. To ensure privacy of conversations in rooms, since those conversations are stored across many servers, Matrix has prioritized e2e encryption.

If bluesky adopts a federated architecture, further work should be done to assess how message passing and message replicating systems affect scalability. Certain types of messages, such as popular topics, could be widely replicated to ensure global availability, and others, such as messages between two users, could be shared only with participating servers.

### P2p networks

P2p networks can be analyzed on a spectrum of fully distributed and p2p, to partially distributed. Many p2p systems have supernode architectures, and points of logical centralization.

Ssb is a fully distributed p2p social protocol that avoids [logical centralization and singletons](https://handbook.scuttlebutt.nz/stories/design-challenge-avoid-centralization-and-singletons), as a design experiment and matter of principle. Ssb was designed for social applications, where data can be passed along follow relationships among users. Data is propagated through a gossip protocol among people who follow each other. As part of its commitment to full decentralization, ssb has no DHT, no universal human-readable namespace, and comes with no bootstrap nodes. New users seeking to connect to the network must often use a [pub server](https://github.com/ssbc/ssb-server/wiki/pub-servers), which is a peer that is always online, has a public IP address, and offers invite codes to new users.

In IPFS and Hypercore, the focus is on content and not social relationships, so there are central directories to aid in discoverability of content, and nodes that guarantee persistence of content if the original host is offline. 'Gateways' are essentially supernodes, and DHTs used for discovery of content are points of logical centralization.

Content in IPFS and Hypercore is accessible over HTTP through gateways. These are nodes that run the p2p protocol and allow HTTP requests to access content in the p2p network through it. Some of these gateways, like the one provided by the IPFS developers themselves, are quite large and become points of centralization. However, anyone can [configure a gateway](https://medium.com/@rossbulat/introduction-to-ipfs-set-up-nodes-on-your-network-with-http-gateways-10e21ea689a4) and run one themselves, as the system is permissionless.

DHTs, distributed hash tables, keep track of who has what data in the network. The data itself may be spread out across many different nodes, but the [DHT](https://docs.ipfs.io/concepts/dht/) serves as the central index of where it is.

### Hybrid networks

Hybrid federated/p2p approaches also exist, such as a client-server architecture where you can optionally run the server client-side.

Matrix's new p2p implementation takes a hybrid approach, moving away from a strictly federated model towards p2p functionality. The new p2p matrix client essentially runs the server client-side, allowing it to participate in the existing federated Matrix ecosystem as a p2p node.

Light clients in blockchain architectures could also be considered an example of a hybrid approach to p2p network participation. Full nodes participate fully in the blockchain network, storing the entire history and validating all blocks and transactions as they come in. Light clients do not store the full history of the chain, which can be many GBs, but request the relevant history from a full node. This allows light clients to transact without having to store the entire history themselves.
