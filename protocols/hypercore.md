# Hypercore Protocol

Hypercore Protocol (abbreviated Hyper), formerly known as Dat, is a distributed append-only log.

Dat was created in 2013 as a protocol designed to help data analysts collaborate on changes to data sets, but has since expanded to other use cases. Dat was renamed Hypercore Protocol in 2020 to reflect the extent of the changes. The older Dat protocol was "a p2p hypermedia protocol that provides public-key-addressed file archives which can be synced securely and browsed on-demand." Hyper places the core of the protocol at a lower level of abstraction. Hyperdrive, a public-key-addressed file drive, is implemented at a layer above. The focus of Hyper is the p2p distribution of an append-only log, and its main purpose is to be a building block for other applications. The switch from Dat to Hyper was motivated by performance and scaling improvements, and the addition of [mounts](https://blog.hypercore-protocol.org/posts/announcing-hyperdrive-10/) for nested hyperdrives. In addition, a DHT was added for peer lookup.

### Identity

In Hyper, nodes are identified using a connection ID which helps avoid duplicate connections. This ID is largely abstracted from the application layer.

Users publish their information in a data structure identified by a public key. Data in the structure is signed using the matching private key. There is no means for revoking lost keys, as in PGP, and no backing central identity provider that can re-issue a private key, but the community is actively working on solutions which include [DNS](https://github.com/beakerbrowser/beaker/discussions/1576) and identity providers.

In the applications space, Hyperdrives are often used to identify users, behaving as "profiles" ([see Beaker Documentation](https://docs.beakerbrowser.com/intermediate/your-profile-drive)).

In a recent [roadmap](https://beakerbrowser.com/2020/06/10/roadmap-summer-2020.html), the core team has identified plans for authenticating messaging channels by proving ownership of Hypercore datastructures.

### Networking

Hypercore data-structures are identified by a public key. They may also be identified by URLs which use the `hyper://` scheme and the base64-encoded public key as the domain. A `hyper://` URL may reference any kind of Hyper-based data structure, including Hyperdrives.

Hyper uses the [Hyperswarm](https://hypercore-protocol.org/#hyperswarm) networking module. Hyperswarm combines a Kademlia-based DHT for global discovery with MDNS to discover peers on local networks. Users [join the swarm](https://pfrazee.hashbase.io/blog/hyperswarm) for a “topic” and query periodically for other peers who are in the topic. The topic is comprised of the hash of the public key which identifies the data being shared. When ready to connect, Hyperswarm helps create a socket between them using either UTP or TCP. A set of bootstrap servers is used to help connect a new node to the network.

The Hyperswarm DHT includes a hole-punching protocol to help nodes establish connections through NATs and firewalls.

Hyperswarm is also used to bootstrap messaging channels between nodes in order to host services and communicate ephemeral application state. An API called [PeerSockets](https://docs.beakerbrowser.com/apis/beaker.peersockets) piggybacks on existing Hypercore replication channels to send messages. The core team has outlined plans in the [roadmap](https://beakerbrowser.com/2020/06/10/roadmap-summer-2020.html) to add connection-topic swarming which should be implemented by mid-summer 2020.

### Data

Hyper focuses on mutable data by organizing content under keys. Hyper's primary data structure is Hypercore, a signed append-only log. In contrast to ssb, which uses a linked-list append-only log, Hypercore uses a Merkle-tree-based append-only log where each entry generates a new leaf and new root. A cryptographic keypair is used to sign the root of the Merkle tree as data is appended to it. Once published, data in the log is immutable.

Hyperdrive is a filesystem datastructure implemented on top of two Hypercores. The first Hypercore (the "metadata core") records file metadata in the form of POSIX-like "stat" objects. It uses a hash trie to provide efficient lookup of entries in a folder. The second Hypercore (the "content core") records file content as a sequence of blobs. The metadata core references the entries in the content core. Unlike IPFS, entries in a Hyperdrive can not be shared individually; they are referenced as paths beneath the Hyperdrive's public key.

Files stored in a Hyperdrive can easily change at any time. Only the original creator of a Hypercore datastructure can append new data, making it a [single-writer data structure](https://mafinto.sh/blog/introducing-hypercore-8.html). Multi-writer data structures can be built by using multiple hypercores that reference each other's elements.

An improvement added to Hyperdrive that was not present in Dat is the ability to mount hypercore-based structures like a folder of files, creating composed filesystems from multiple different authors.

Applications approach data schemas using a loose consensus and will often publish JSON, Markdown, and other forms of media. Paths within Hyperdrives will sometimes be considered significant; for instance, social media feeds leverage the `/microblog/` folder to identify user posts.

A node must be actively sharing a topic for it to be available. To ensure persistence, nodes can host their own data on a server that stays online, or pay a hosting service.

### Moderation & Reputation

Hyper is a layer below moderation and reputation. There is no concept of "leeches" to evaluate whether nodes are being good seeders of content.

### Social & Discovery

Nodes create "discovery keys" for topics by hashing the public key of the Hypercore. Data they want to associate with that topic can be added to the signed log. The community amd core team is actively working on [DNS](https://github.com/beakerbrowser/beaker/discussions/1576) to give short-names to keys, so that users can have a "foo.com" address for their topic.

Beaker browser has implemented an informal [social protocol](https://docs.beakerbrowser.com/joining-the-social-network) on Hyper based on [profile drives](https://docs.beakerbrowser.com/intermediate/your-profile-drive) and private [address books](https://docs.beakerbrowser.com/intermediate/your-address-book). Users publish "microblog posts" by writing files to the `/microblog/` folder of their profile drive which applications then merge and sort by the `ctime` metadata of the files. (This system functions similarly to RSS.) Social relationships are not currently public.

### Privacy & Access Control

By default, Hypercore Protocol connections are encrypted using the [Noise protocol](https://noiseprotocol.org/) encryption framework. Hyperswarm does not hide user IPs or mask metadata.

Hyper datastructures can only be accessed by users with knowledge of the public key. Because public keys are unguessable, this means they must be shared out of band prior to access (it is not possible to access the data by watching the network).

Fine-grained access within Hypercore datastructures, such as access to individual files or folders in a Hyperdrive, is not yet implemented.

### Interoperability

Hyper is not interoperable with any other protocol, and has not prioritized implementing access over HTTP through gateways, though it would be possible.

The Hyperdrive data-structure is designed to be POSIX-compatible which enables Hyperdrive access via a [FUSE interface](https://en.wikipedia.org/wiki/Filesystem_in_Userspace).

### Scalability

Scalability statistics are not available for the Hyper network.

### Metrics

There are about 300 to 400 nodes in the Hyperswarm DHT as of June 2020.

On Github, Hyper has [38 contributors](https://github.com/hypercore-protocol/hypercore/graphs/contributors) on the Hypercore module, and 781 dependent projects. The Hyperdrive module has [27 contributers](https://github.com/hypercore-protocol/hyperdrive/graphs/contributors) and 531 dependent projects.

### Governance & Business Models

The Hypercore protocol's development is led by two companies, Blue Link Labs (which develops Beaker browser), and Hyperdivision consulting. Development is coordinated through community effort in the [Hypercore Protocol GitHub Org](https://github.com/hypercore-protocol). A biweekly "community consortium" call is conducted between Blue Link Labs, Hyperdivision consulting, and 6-10 other companies in the community.

Blue Link Labs's business model is to run cloud Hypercore hosting services. Hyperdivision consults with companies implementing Hyper in their software. Many of the community projects are grant-funded.

### Implementations & Applications

The [Beaker browser](https://beakerbrowser.com/) is a p2p web browser for Hyper sites.

[Kappa DB](https://github.com/kappa-db/kappa-core/blob/master/intro.md) is an append-only log DB that uses hypercores.

Applications using Hyper include:

- [Blahbity Blog](https://youtu.be/zwR6YyConQI?t=878), a twitter clone built in Beaker browser
- Sneaky Wiki, a wiki application built in Beaker browser
- [Peer-to](https://peer-to.peer-to-peer-web.com/), an online art exhibition only available through the Beaker browser.
- [Mapeo](https://www.digital-democracy.org/mapeo/), an offline-friendly mapping software to support indigenous land rights.
- [Cabal](https://cabal.chat/), a p2p chat platform using Hypercore
- [Ara](https://ara.one/), a blockchain content platform
- [Sonar](https://arso.xyz/sonar), a p2p database and search engine that stores data in hypercores
- [Cobox](https://cobox.cloud/), a distributed, encrypted, offline-enabled data hosting cloud platform.
- [Wireline](https://www.wireline.io/), a network stack for peer-to-peer and serverless cloud computing.

### Links

- [Hypercore Protocol](https://hypercore-protocol.org/)
- [Beaker Browser](https://beakerbrowser.com/)
- [Comparing IPFS and Dat](https://medium.com/decentralized-web/comparing-ipfs-and-dat-8f3891d3a603)
