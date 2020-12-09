# Data

### Data Models

#### Solid

Solid uses [RDF](https://www.w3.org/RDF/) (Resource Description Framework), which is a standard model for data exchange on the web. RDF was adopted as a W3C recommendation in 1999 as a standard for graph-based data, intended for decentralized information sharing. RDF extends the linking structure of the web to use URIs to name the relationship between things, allowing structured data to be shared across different applications. RDF is based on the idea of [making statements about resources](https://en.wikipedia.org/wiki/Resource_Description_Framework) of the form _subject-predicate-object_, known as triples. It is an abstract model with several serialization formats, so the particular encoding for resources or triples varies.

RDF has all the advantages and generality of structuring information using graphs. It is the foundation for publishing and linking data in the [Semantic Web](https://www.w3.org/standards/semanticweb/). If it were fully adopted, advantages of using RDF would include being able to have the same API everywhere, as opposed to requiring code to integrate with each custom web API. Disadvantages include being non-human-readable and historically [complex for developers to use](https://hal.inria.fr/hal-01966561/document).

#### XMPP

XMPP is an XML (Extensible Markup Language) protocol for near-real-time messaging. The basic protocol data unit in XMPP is an XML "stanza", a fragment of XML that is sent over a XML stream.

#### Matrix

Matrix transports messages using JSON, instead of XML like its most similar predecessor, XMPP. In the years since XMPP was standardized, JSON has become a [popular alternative to XML](https://blog.cloud-elements.com/json-better-xml) because it's less verbose, parses quickly, and is human-readable. Conversation history in Matrix is tracked through DAGs.

#### ActivityPub

ActivityPub uses streams of JSON-LD, based on the ActivityStreams data format.
AcvitityPub implementations mostly use a [subset of JSON-LD](https://stephank.nl/p/2018-10-20-a-proposal-for-standardising-a-subset-of-json-ld.html), related to namespacing, versioning, and extensions. JSON-LD allows for extensibility, so that ActivityStreams do not have to contain all the vocabulary needed for future applications.

#### IPFS

IPFS uses a custom data structure, [IPLD](https://ipld.io/), which is designed to treat hash-linked data structures as subsets of a unified information space. IPLD is a single namespace for all hash-based protocols, such as git and cryptocurrencies. IPLD objects can be expressed in various serialized formats such as JSON, CBOR, YAML, and XML.

#### Ssb

Ssb uses append-only logs of signed JSON. Each ssb user has their own [signed hash-based linked list](https://spec.scuttlebutt.nz/feed/messages.html), called a _sigchain_. This design ensures that messages are not lost, the order is not confused, data can be moved across untrusted machines, and replication of data is simple. Drawbacks include: inability to subscribe to only parts of the data, no support for mutable data, and inability to update from multiple machines, resulting in no multi-device usage.

#### Hypercore

The Dat protocol renamed itself to Hypercore to reflect a transition that placed the core of the protocol at a lower level of abstraction. The primary data structure of Hypercore is a signed append-only log that is p2p distributed, and intended as a building block for other applications. The former Dat protocol's main structure, a public-key-addressed file drive, is now implemented as Hyperdrive on top of Hypercore. In contrast to ssb, which uses a linked-list append-only log, Hypercore uses a Merkle-tree-based append-only log where each entry generates a new leaf and a new root.

### Mutability

Federated applications allow users to edit and delete content, as these operations are handled at the server level. To ensure content is deleted across the entire network, applications must honor delete messages.

P2p applications have more variance around mutability, as some data structures can lead to immutable content.

**Ssb & Hypercore** - Messages added to the append-only logs used by ssb and hypercore are immutable. Applications can choose not to display messages indicated as deleted, but the data cannot be overwritten.

**IPFS** - Data is content-addressed, so once added to a network, content is discoverable by its hash. If users stop hosting it and no copies are left online, it will be inaccessible. However, if a copy remains stored on the network, it is re-discoverable by its immutable reference hash.

**Aether** - Aether attempts to design a more ephemeral p2p content network. "Stale" threads that have not been referenced for 6 months get dropped by clients in the network.

**Blockchain social applications** - Many [blockchain social applications](../applications/blockchain-social.md) do not allow deletion of content, as data posted on a public blockchain is immutable.
