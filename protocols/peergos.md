# Peergos

Peergos is a p2p end-to-end encrypted storage and application protocol on top of IPFS. It is also a social file-sharing application by the same name. The goal of Peergos is to provide a global multi-user filesystem that provides privacy, identity, login, and secure sharing for decentralized applications. It is designed to be independent of DNS and TLS certificate authorities, and to protect privacy through quantum resistant encryption.

Peergos was [started in 2013](https://peergos.org/about) with the aim of a secure decentralized file storage and sharing network with a secure email replacement.

### Identity

Peergos users are identified by unique usernames linked to public keys. The uniqueness of usernames is ensured through a global append-only log for [public key to username](https://book.peergos.org/architecture/pki.html) mappings that is mirrored on every node in the Peergos system. Names are taken on a first come first served basis. Currently, a single server determines the canonical state of this log, and other nodes sync to it. Long-term considerations include decentralizing the name server through a blockchain architecture.

Peergos allows [multi-device login](https://book.peergos.org/features/multi.html) through a password-based interface. A user's private keys are derived every time they log in using their username, password and a published salt. Specifically, a signing keypair, boxing keypair, and symmetric key is derived. Users store their friend's keys in their encrypted storage space in a TOFU keystore. Users can verify their friend's keys out of band using QR codes or fingerprints.

### Network

Each user must be registered to at least one Peergos server (A server can host any number of users and any server can choose to mirror data for any user). Peergos servers run an instance of IPFS, which handles networking and connection management. A server is any device storing user data, and could be a mobile phone, a cloud server, or hardware plugged in at home.

### Data

Data in Peergos is content-addressed: stored in mappings from hash to data. During upload the client splits files into 5 MiB chunks which are each independently encrypted (along with encrypted metadata) and stored in a [merkle-CHAMP](https://book.peergos.org/architecture/champ.html) (compressed hash array mapped prefix trie) in IPFS. Directories can't be distinguished from small files. To hide file sizes and split files up into 5 MiB chunks which aren't linkable, the size within a chunk is also rounded up (padded before encryption) to a multiple of 4 KiB. This means all chunks can only have 1 of 1280 possible sizes. Servers do not have visibility into the file sizes, the number of files, directory structure, or which users have access

The user publishes the IPFS node ID (hash of its public key) of their server in the PKI. This allows writes and new friend requests to be securely tunneled to their server. It synchronizes their writes and publishes their latest root hashes.

### Social & Discovery

Users can follow each other. [Follow requests](https://book.peergos.org/architecture/follow.html) are sent through a user’s storage server, which is contacted via its public key. Follows are one-way, and allow sharing files and sending messages. Critically, the server never sees who is following who (even follow requests are blinded). Users store their own social graph encrypted in their Peergos space.

### Privacy & Access Control

All encryption happens on the client, which could be a native Peergos client or a browser. Data is always encrypted on the servers. Servers do not have access to metadata or sensitive information.

Access is controlled through cryptographic capabilities. Access is hierarchical, and stored in an encrypted structure called [cryptree](https://book.peergos.org/security/cryptree.html).

A read-only capability consists of the hash of the file owner's public key, the hash of the writer's public key, a random label, and a symmetric encryption key. Access to files gained through social follows can be revoked by rotating cryptographic keys, but the interface does not display keys to users. Users simply click "revoke access to <user>".

To make a file or folder publicly visible, a user can publish its capability. A user can also share secret links to files, like a google doc "share" link, which lets anyone who views it view the file. These [secret links](https://book.peergos.org/features/secret.html) don't expose the file to the server. The file is not transmitted unencrypted over the network, as the key to decrypt it is in the URL itself (in the hash fragment which isn't sent to the server), and is interpreted locally in the browser.

A writable capability includes the private key corresponding to the writer key, which is used to sign updates.

Peergos has undergone an independent security [audit by Cure53](https://peergos.org/posts/security-audit).

### Scalability

Peergos can handle arbitrarily large files, including random access, upload and download, and on under-powered devices like mobile phones. This is largely due to the independent encryption of each 5 MiB section, as well as the "[zero IO](https://peergos.org/posts/fast-seeking)" seeking within a file.

### Governance & Business Models

Peergos was developed by the [core team](https://peergos.org/about) on a [self-funded](https://donorbox.org/peergos) volunteer basis for years. It has received grants from [Protocol Labs](https://peergos.org/posts/dev-update), the company that stewards IPFS, and from [Santander](https://twitter.com/oxfoundry/status/1232766848816549888).

### Implementations

Peergos is a private and access controlled layer on top of IPFS which can be used to build applications. Other than the Peergos reference implementation client which allows users to store and share private files, there are a few demo [applications](https://peergos.org/posts/applications), including a read-only viewer for PDF files, and an editor for text or code.

The goal of allowing people to deploy apps on Peergos that are viewable from the browser is currently limited by [sandboxing constraints](https://kevodwyer.github.io/sandbox/) within browsers.

### Links

- [Peergos](https://peergos.org/)
- [Peergos book](https://book.peergos.org)
- [Source code](https://github.com/peergos/peergos)
