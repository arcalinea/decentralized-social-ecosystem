# GUN

GUN is a decentralized graph database with a conflict resolution algorithm (CRDT) and synchronization protocol. It includes a library of tools for merging conflicting data and handling routing, security, and storage.

In GUN's graph store, entries are [javascript objects under UUID keys](https://gun.eco/docs/Porting-GUN). Objects can be data of any type, including files, JSON, or other documents. Data is stored in the browser by default, with backup "superpeers" to ensure persistence. Peers connect to other peers, and choose what data to synchronize and persist.

### Identity

Gun's [User System](https://gun.eco/docs/Auth) allows the creation of a human-readable username and password. Usernames are global but not unique. As with many other decentralized systems, there is no password recovery mechanism.

[Multi-device login](https://gun.eco/docs/Auth) is handled by encrypting a user's crytographic keypair, which is stored in the GUN graph. Keypairs are not derived from the password. Instead, a PBKDF2 proof is derived from the password, and AES keys are derived from that to encrypt the keypair. GUN treats this method as "secure enough" for applications in which private keys do not control financial information.

Authentication is performed by doing a GUN query for the account username, subscribing to it, and then attempting to brute force decrypt the keys of all accounts that match the username until a match is found. Once an account has been loaded once, it's cached on device, loading from localstorage or the local harddrive.

GUN's [SEA](https://gun.eco/docs/SEA) (Security, Encryption, Authorization) module provides the capability to directly create a [public/private keypair](https://gun.eco/docs/SEA) for a user, without a username and account.

### Network

GUN uses a [gossip-based protocol](https://gun.eco/docs/DAM) along with a topic-based PubSub protocol to sync data between peers. GUN peers fall back to the gossip protocol when the more optimized PubSub [routing](https://gun.eco/docs/Routing) protocol fails. Messages can be routed across different transport layers (websockets, WebRTC, multicast UDP, etc).

Peers subscribe to graphs relevant to their application's logic, although the global GUN graph is accessible to all peers.

Planned future network upgrades include the addition of a DHT. A [tokenized incentivized mesh proposal](https://web.stanford.edu/~nadal/A-Decentralized-Data-Synchronization-Protocol.pdf) is also on the roadmap.

### Data

Peers subscribe to the data they need and the network retrieves it from any peer (including browsers, where GUN stores data in localStorage). Running always-online peers, a "superpeer", is recommended for most applications to ensure availability of data when most browser-based peers may be offline. A superpeer is an IP addressable machine running node.js that persists data to disk.

There is a public space, and a subset of that is the user space. In the public space are all graphs without a public key as their ID. In the user space, graphs are signed with the user's keys, and their IDs must include the user's public key.

GUN uses a CRDT (Conflict-free Replicated Data Type) to merge data. Conflicts are handled by a [conflict resolution algorithm](https://gun.eco/docs/Conflict-Resolution-with-Guns) that uses lexical sort. GUN is [strongly eventually consistent](https://pages.lip6.fr/Marc.Shapiro/slides/CRDTs%20Google%20Zurich-2011-09.pdf), meaning that peers will eventually converge upon the last updated value when nodes that are offline eventually receive updates.

GUN focuses on mutability by not using an append-only log (which implements updates, insertions, and deletion as a layer on top of the immutable log). [Deletion](https://stackoverflow.com/questions/37758618/how-to-delete-data-in-gun-db) in GUN works by overwriting bytes with `null`, or by de-referencing portions of a graph. A content-addressed graph space is used to implement immutable, append-only data.

### Privacy & Access Control

Access control is built into the [User system](https://gun.eco/docs/Auth) and can be combined with SEA, GUN's encryption utilities, for more advanced use cases.

Cryptographic keypairs are assigned to roles, groups, or data points. This information is either used to derive a shared ECDH secret to decrypt (read), or to load collaborative multi-writer edits (signed).

[Iris-lib](https://github.com/irislib/iris-lib) is a decentralized social networking library built on GUN. It provides an API for end-to-end encrypted chat channels and private contact list management.

### Interoperablity

Plugins, such as backup storage on centralized databases or file systems, can be used to extend GUN.

### Scalability

Test relays (superpeers) on GUN can handle about 10k simultaneous connections: http://guntest.herokuapp.com/stats.html

### Metrics

- 10M ~ 30M monthly [downloads](https://www.jsdelivr.com/package/npm/gun)

### Monetization

The GUN protocol is developed by a [VC-funded company](https://era.eco/#step1), which funds the development of Iris as well. The business model is based on consulting and integrations. Future business models include a proposed paid service through a blockchain-based [tokenized bandwidth incentive network](https://web.stanford.edu/~nadal/A-Decentralized-Data-Synchronization-Protocol.pdf).

### Implementations

GUN is used for p2p chat/social apps, encrypted video conferencing, realtime GPS tracking, and AR/VR multiplayer games, among other applications.

- [Internet Archive](https://news.ycombinator.com/item?id=17685682) uses GUN for their [dWeb library](https://github.com/internetarchive/dweb-transports) metadata
- [HackerNoon](https://hackernoon.com/state-of-hacker-noon-2019-2020-8w1ls3axx) integrated GUN for annotations
- [Meething](https://meething.space/) is a Mozilla backed secure & decentralized video conferencing that uses GUN
- [Party](https://party.lol/) and [Maskbook](https://maskbook.com/), encrypted browser extensions, use GUN
- [Notabug](https://notabug.io/t/notabug/comments/59382d2a08b7d7073415b5b6ae29dfe617690d74/welcome-to-notabug), a decentralized Reddit clone, uses GUN
- [Unstoppable Domains](https://unstoppabledomains.com/chat) and [DTube](https://d.tube/) use GUN for messaging
- [Iris](https://irislib.github.io/), is a web-of-trust based social network built on GUN

# Iris

[Iris-lib](https://github.com/irislib/iris-lib) is a library built on Gun that allows the integration of decentralized social networking features into applications. An experimental social network application, [Iris](https://github.com/irislib/iris), was built to demonstrate its features: public messaging, [private chats](<(https://iris.to/), web of trust, and contact management. Iris-lib uses [Gun](../proocols/gun.md) for networking and data storage, and [IPFS](../protocols/ipfs.md) for attachments and message backups. The team is funded by Gun, and also accepts donations.

Iris uses WoT (Web-of-Trust) attestations to link human readable names to key-pair and other identity attributes. Users only see messages in their WoT, from users who have been upvoted by someone in a chain from someone they upvoted. Downvotes are also possible. [Reputation](https://medium.com/@mmalmi/learning-to-trust-strangers-167b652a654f) is not represented by a static score, but by how a user's personal web of trust regards them. A percentage threshold of confidence in a person's identity is calculated by the number of attestations relative to the size of the network.

For interoperability, Iris allows [importing content from other sources](https://porter.io/github.com/irislib/iris). "Message author and signer can be different entities, and only the signer needs to be on Iris. For example, a crawler can import and sign other people's messages from Twitter. Only the users who trust the crawler will see the messages."

### Links

- [Site](gun.eco)
- [3box comparison of p2p DBs: GUN, OrbitDB, Scuttlebutt](https://medium.com/3box/3box-research-comparing-distributed-databases-gun-orbitdb-and-scuttlebutt-2e3b5da34ef3)
