# IPFS

IPFS is a [content-addressed protocol](https://docs.ipfs.io/concepts/content-addressing/) for peer-to-peer hypermedia storage and distribution. The IPFS network is mainly used as a storage layer for decentralized applications. Its main purpose is to add, share, find, and transfer files in a globally distributed file system. Components of IPFS, such as [libp2p](https://libp2p.io/), the p2p networking library, and [IPLD](https://ipld.io/), the data model for content-addressed Merkle DAGs, are used separately by applications that do not participate in the IPFS network.

The IPFS Alpha launched in February 2015. IPFS protocols are designed for upgradeability.

### Identity

IPFS nodes have a peer ID, the [hash of their public/private keypair](https://docs.libp2p.io/concepts/peer-id/). When peers connect, they exchange public keys and check to make sure they match the node IDs. Communications are encrypted using these keys. Node IDs are pseudonymous, and can be reset as needed to maintain privacy. Node private keys are stored in the IPFS config by default.

IPFS can serve as a content addressed storage system for decentralized identity solutions (such as [Microsoft's ION](https://techcommunity.microsoft.com/t5/identity-standards-blog/ion-booting-up-the-network/ba-p/1441552)). [IPID](https://github.com/jonnycrunch/ipid) is an implementation of the DID (decentralized identifiers) specification over IPFS, using [IPNS](https://docs.ipfs.io/concepts/ipns/).

### Network

IPFS is a distributed protocol. Every node gets to participate in the network in any configuration, enabling different kinds of network topologies to emerge. Nodes can connect to each other independently of the client (browser, mobile, desktop, command line).

When nodes join the network, they bootstrap off of long-lived peers or those in their local area network. IPFS comes with a [default list of trusted peers](https://docs.ipfs.io/how-to/modify-bootstrap-list/), which can be modified. A node can opt-in to be part of the main public network and/or an alternative network. Nodes joining the main public network will join the [DHT](https://docs.ipfs.io/concepts/dht/) as either clients (consumers) or servers (providers of the content routing service). Nodes can also directly connect to peers they’re interested in either through peer ID or through subscribing to relevant pubsub channels.

All nodes in the IPFS network use [libp2p](https://libp2p.io/), the modular networking library, to make peer-to-peer connections. Libp2p is [transport agnostic](https://docs.libp2p.io/concepts/transport/), leaving the choice of transport protocol up to the developer, and allowing an application to support many different transports at the same time. Peers dial to each other using a [multiaddr](https://multiformats.io/multiaddr), a self-describing network address that lets peers know a node’s preferred way to be dialed. The use of multiaddrs is intended to future-proof addresses, and allow multiple transport protocols and addresses to coexist. All connections in IPFS are end-to-end encrypted and authenticated using public/private key cryptography.

[Gateways](https://docs.ipfs.io/concepts/ipfs-gateway/) allow IPFS to be accessed over HTTP, which makes content stored in IPFS accessible through a standard browser.

### Data

IPFS is commonly known as a distributed file system, but the data layer is closer to a graph database with elements of linked data. IPFS uses [IPLD](https://ipld.io/) for representing any piece of data available in the network. The IPLD data model treats all hash-linked data structures as subsets of a unified information space. Higher level abstractions can be built on top of it, like the OrbitDB database, or Textile threads.

Files are located in the IPFS network by their [CID](https://docs.ipfs.io/concepts/content-addressing/), a content identifier that is based on the hash of the file. The hash of a file does not change, so this form of addressing does not allow updates. However, mutable addresses can be built on top by using the hash of a public key as an address. IPFS's native version is [IPNS (InterPlanetary Name System)](https://docs.ipfs.io/concepts/ipns/), although other versions (such as [ENS](https://gist.github.com/PhyrexTsai/cffcbfa1d752b9cf817d920dfcd1ec9f)) are compatible. The keypair associated with the address is used to sign content that is published under it. [DNSLink](https://docs.ipfs.io/concepts/dnslink/) is also used to map a domain name to an IPFS address. It is currently faster than IPNS and has the advantage of being human-readable and memorable.

IPFS does not have built-in incentives for nodes to persist content to the network. Users store their own data by [‘pinning’](https://docs.ipfs.io/concepts/persistence/) it to their local IPFS nodes, communities collaborate in backing up data through tools like [‘collaborative clusters’](https://blog.ipfs.io/2020-01-09-collaborative-clusters/), and enterprises pay 3rd party pinning services like Infura to ensure availability and reliability. Projects like [Storj](https://storj.io/blog/2019/10/ipfs-now-on-storj-network/) and [Filecoin](https://filecoin.io/) are building blockchain networks for incentivizing persistent IPFS storage.

If all users stop hosting a piece of data, it is removed from the network. However, if a node chooses to continue hosting it, [it can still be located](https://github.com/ipfs-inactive/faq/issues/9) by its content ID.

### Moderation & Reputation

IPFS is mostly a layer below identity and reputation, but libp2p has some low-level primitives around connection management which can be used to encode peer reputation. Peer reputation is based on how reliable the peer is at returning requested data. Both libp2p and IPFS support the explicit configuration to avoid or block known bad IPs.

Each IPFS node is in full control of the data it pins. Nodes can add a denylist to their configuration, optionally using the one [used by public gateways](https://github.com/ipfs/infra/blob/master/ipfs/gateway/denylist.conf) to block DMCA takedown content, malware, and other illegal or pernicious content. There is a proposed design for an autonomy-preserving content moderation system by which nodes can subscribe to denylists from entities they trust to help avoid or filter unwanted content.

### Social & Discovery

IPFS uses a DHT for finding content in the network. Each host advertises the data they’re storing once per day, which can be looked up through the DHT. Nodes also discover peers through their local area network, and by bootstrapping with other nodes. The DHT is also used for bootstrapping pubsub channels which groups can subscribe to for topic-based updates to content they care about.

### Privacy & Access Control

Content published to IPFS is public by default. Encryption can be used to add privacy and access control layers on top of IPFS (see [Peergos](peergos.md)).

### Interoperability

IPFS [Gateways](https://docs.ipfs.io/concepts/ipfs-gateway/) allow the network to be accessed over HTTP in browsers without native IPFS support.

The IPLD data structure is designed to allow any kind of hash linked data can be ingested into IPFS, including blockchains like [Bitcoin](https://github.com/ipld/go-ipld-btc) and [Ethereum](https://github.com/ipfs/go-ipld-eth), and [git repos](https://github.com/ipfs-shipyard/git-remote-ipld).

### Scalability

The IPFS public network currently has hundreds of thousands of nodes. Private networks also run IPFS without connecting to the main DHT, and are not included in the node count.

IPFS nodes have historically had [high resource consumption](https://hackernoon.com/ipfs-a-complete-analysis-of-the-distributed-web-6465ff029b9b), although improvements and ['low power' settings](https://www.reddit.com/r/ipfs/comments/7sfcbq/im_running_ipfs_on_my_raspberry_pi/) for weaker devices have since been added.

### Metrics

- [Automated metrics about IPFS related projects](https://github.com/ipfs/metrics)
- [100ks of nodes as of May 2020](https://youtu.be/RxJSUBeqOKU?t=392)
- [~4000 contributors](https://github.com/ipfs-shipyard/get-gh-contributors)

### Governance & Business Models

IPFS is developed by [Protocol Labs](https://protocol.ai/), a VC-funded company that [raised over 200 million](https://filecoin.io/blog/sale-completed/) in a token sale for Filecoin. The core implementations working group, consisting of both employees of the company and external contributors, has decision-making authority over contributions to the IPFS protocol. Libp2p, IPLD, and Filecoin are stewarded by separate working groups.

Contributors from the open source community either volunteer their time, or are funded through companies that have raised money to build on top of IPFS.

### Implementations & Applications

Implementions of IPFS exist in go and javascript, and a [Rust implementation is under development](https://blog.ipfs.io/2020-03-18-announcing-rust-ipfs/). Projects like [Textile](https://textile.io/), [OrbitDB](https://orbitdb.org/), and [3box](https://www.3box.io/) have built additional layers of tooling on top of IPFS to support a wider range of applications.

Examples of tools that have expanded the use cases of IPFS include:

- Textile [buckets](https://docs.textile.io/concepts/buckets/) - dynamic folders for decentralized applications, distributed over IPFS
- [OrbitDB](https://github.com/orbitdb/orbit-db) is a serverless, p2p database that uses IPFS to store data, and IPFS PubSub to sync databases with peers. It uses CRDTs to resolve conflicts.
- 3box has a [web application framework](https://docs.3box.io/build/web-apps) that stores data in IPFS

Libp2p is used, independently of IPFS, by other decentralized networks such as Polkadot, ETH2, and [Matrix](matrix.md), which is experimenting with as a transport layer for the p2p version.

A list of applications that use IPFS: https://awesome.ipfs.io/

![Ecosystem](https://ipfs.io/ipfs/QmSKi1BCzVmiPPSFNbTN5FafZxA1kLxsKyLQtQrAQEVS3H?filename=Ecosystem%20Diagram.png)

Notable p2p applications include:

- [OpenBazaar](https://openbazaar.org/), a marketplace
- [Peepeth](https://peepeth.com/welcome), a social network on IPFS and Ethereum
- [Peergos](https://peergos.org/), a private distributed file sharing protocol and application
- [Dtube](https://about.d.tube/), a Youtube alternative
- [Everipedia](https://everipedia.org/), a wikipedia alternative [built on IPFS](https://qz.com/1151073/wikipedias-cofounder-on-how-hes-creating-a-bigger-better-rival-on-the-blockchain/)
- [Audius](https://github.com/AudiusProject), a music streaming service
- [Anytype](https://anytype.io/), a locally hosted Notion-like writing platform

Enterprise adoptions and integrations include:

- [Microsoft](https://techcommunity.microsoft.com/t5/identity-standards-blog/ion-booting-up-the-network/ba-p/1441552) uses IPFS for their DID implementation
- [Cloudflare](https://developers.cloudflare.com/distributed-web/ipfs-gateway/) runs a popular IPFS gateway hosted at https://cloudflare-ipfs.com/
- [Netflix](https://blog.ipfs.io/2020-02-14-improved-bitswap-for-container-distribution/) switched to IPFS for docker container distribution, improving performance 2x
- [Opera](https://blog.ipfs.io/2020-03-30-ipfs-in-opera-for-android/) IPFS is supported by default in the Opera browser for Android

### Links

- [Docs](https://docs.ipfs.io/)
- [Mapping the Interplanetary Filesystem](https://arxiv.org/pdf/2002.07747.pdf)
- [Comparing IPFS and Dat](https://medium.com/decentralized-web/comparing-ipfs-and-dat-8f3891d3a603)
