# Identity

Centralized identities are administered and controlled by a single authority. Centralized social networks offer users identities that are administered and controlled by the service. Decentralized social networks offer forms of identity that give users varying degrees of control. Decentralized identities may be [federated, user-centric, or self-sovereign](https://github.com/WebOfTrustInfo/self-sovereign-identity/blob/master/ThePathToSelf-SovereignIdentity.md).

We will call entities with identities "actors", because non-human entities such as companies, organizations, and bots may have identities on a social network.

Identity allows an actor to:

- control an account and access private data
- communicate with another actor
- establish visible reputation and credibility

Desirable qualities for decentralized identities:

- Allow authentication and migration between services
- Allow communication across services
- Unique, global, and memorable

## Decentralized Identity

OpenID was originally developed by the creator of LiveJournal in 2005 as a decentralized identity system. The goal was for users to own their identity, and to make logins easier, but the protocols has since mostly [been abandoned](https://meta.stackexchange.com/questions/307647/support-for-openid-ended-on-july-25-2018) due to its complexity and poor user experience.

OAuth is currently the most successful identity standard. OAuth was created as an authorization protocol to securely transfer user credentials from one site to another. OpenID Connect is an authentication layer built on top of OAuth 2.0. OAuth allows different identity providers, but in practice identity providers became centralized because users could not run their own identity provider, or manage to choose among a long list of identity providers.

### Identity in federated applications:

Email is the most successful federated social application. As a result, many user identifiers in federated applications look similar to email addresses. Federated identity systems rely on DNS.

- [Diaspora](../applications/diaspora.md) - User identities in Diaspora are tied to their pod, and cannot be migrated. Diaspora uses the Webfinger protocol to discover users from other pods. User information is returned via hCard, an open microformat standard for identity.

- [Mastodon](../applications/mastodon.md) - User identities in Mastodon are the username followed by the instance the user belongs to: `@alice@mastodon.social`

- [Matrix](../protocols/matrix.md) - User identity in Matrix is a username followed by the homeserver: `@bob:matrix.org`

- [Solid](../protocols/solid.md) - Solid uses WebID URIs as universal usernames. The WebID URI's function is to point to the location of a public WebID Profile document: `https://alice.databox.com/profile/card#me`

- [XMPP](../protocols/xmpp.md) - User identity in XMPP is a username followed by the homeserver: `alice@example.com`

### Identity in p2p applications:

P2p systems that put identity entirely in the hands of users must deal with [key management](##key-management), key verification, and key backup. Account recovery is usually not possible, because there is no third party to recover an identity if a user loses their password or key. The following p2p identity systems are independent of DNS.

- [Aether](../applications/aether.md) - Identities in Aether are keypairs. Users can choose a custom nickname, but it is not unique. Multi-device usage is possible, but difficult, and requires manually porting a user config file across devices.

- [Gun](../protocols/gun.md) - Gun's [User System](https://gun.eco/docs/Auth) creates a username and password, with usernames are global but not unique. [Multi-device login](https://gun.eco/docs/Auth) is handled by encrypting a user's crytographic keypair, which is stored in the GUN graph. Keypairs are not derived from the password. PBKDF2 proof is derived from the password, and AES keys are derived from that to encrypt the keypair. GUN treats this method as "secure enough" for applications in which private keys do not control financial information. "Auth" is doing a GUN query for that account, subscribing to it, and then attempting to brute force decrypt the keys of all accounts that match that username. Once an account has been loaded once, it's cached on that device, loading from localstorage or the local harddrive.

- [Peergos](../protocols/peergos.md) - Peergos users are identified by unique usernames linked to public keys. The uniqueness of usernames is ensured through a global append-only log for [public key to username](https://book.peergos.org/architecture/pki.html) mappings that is mirrored on every node in the Peergos system. Names are taken on a first come first served basis. Currently, a single server determines the canonical state of this log, and other nodes sync to it. Long-term considerations include decentralizing the name server through a blockchain architecture. Peergos allows [multi-device login](https://book.peergos.org/features/multi.html) through a password-based interface. A user's private keys are derived every time they log in using their username, password and a published salt.

- [Ssb](../protocols/ssb.md) - Ssb user identities are cryptographic keypairs, stored locally. Multi-device login is not possible because keys are stored on user devices. Users can pick a human-readable nickname that is associated with their key, but nicknames are not unique because there is no global registry.

### Blockchain Identity Systems

In 2001, Zooko Wilcox-O'Hearn named three desirable properties of decentralized network identifiers: human-meaningful (memorable), decentralized (global), and secure (unique). This became known as [Zooko's triangle](https://en.wikipedia.org/wiki/Zooko%27s_triangle). Prior to the invention of cryptocurrency blockchains, which enabled decentralized global consensus, it was thought that only two of these three properties could be achieved at one time. Now, many projects have created blockchain-based protocols for naming systems that fulfill all three properties.

- [Namecoin](https://www.namecoin.org/) - One of the first forks of Bitcoin to create a blockchain with an alternative use case, [Namecoin](https://coincentral.com/namecoin-nmc-beginners-guide/) functions as a blockchain-based key/value pair registration and transfer system. The Namecoin chain is used for NameId, which combines identities on NameCoin with OpenID, and Dot-Bit DNS, which distributes a DNS registry over the network.

- [ENS](https://ens.domains/) - The Ethereum Name Service gives users a `.eth` domain associated with an Ethereum address, or allows them to manage DNS names they already own. It is managed by a smart contract on the Ethereum blockchain. Names are allocated through an [auction process](https://medium.com/the-ethereum-name-service/a-beginners-guide-to-buying-an-ens-domain-3ccac2bdc770).

- [Blockstack](https://www.blockstack.org/) - Blockstack originally built a DNS system for decentralized app development on the [Bitcoin blockchain](https://bitcoinmagazine.com/articles/how-blockstack-uses-bitcoin-base-their-decentralized-app-ecosystem), and later [migrated to to a custom blockchain](https://www.blockstack.org/p/roadmap) with smart contract programming functionality.

- [Handshake](https://handshake.org/) - Handshake is a blockchain designed for allocating ownership right to top level domains through an [auction process](https://www.namebase.io/blog/tutorial-3-basics-of-handshake-auction-and-bidding/). It uses a UTXO model, like Bitcoin, and implements smart contract programming functionality on top.

## Decentralized Identifiers (DIDs)

[DIDs](https://w3c-ccg.github.io/did-primer/) are a new type of globally unique identifier that do not require a centralized registration authority, and can serve as a decentralized public key infrastructure. The concept is being formalized into an [emerging W3C standard](https://www.w3.org/TR/did-core/). The term "DIDs" was [originally coined](https://github.com/WebOfTrustInfo/rwot5-boston/blob/master/topics-and-advance-readings/did-primer.md) by the W3C Verifiable Claims Task Force in the spring of 2016. The groups researching decentralized identity that converged on the concept of DIDs were inspired by the potential of using blockchains as decentralized, yet global identity registrars.

DIDs aspire to be a [self-sovereign identity](http://www.lifewithalacrity.com/2016/04/the-path-to-self-soverereign-identity.html). They differ from other globally unique identifiers in that they are globally resolvable, decentralized, and cryptographically verifiable. DIDs require a global key-value database in which the database is a blockchain, distributed ledger, or decentralized network.

The format of a DID is: a scheme identifier, followed by the DID method, followed by a method-specific identifier. A simple example: `did:example:123456789abcdefghi`

DIDs point to DID documents. Entities holding DIDs may be authenticated via proofs, or verifiable credentials.

As of 2020, a [Peer DID Method Specification](https://openssi.github.io/peer-did-method-spec/) is under development, which does not require any central source of truth, and is suitable for private relationships.

#### DID Implementations

DID implementations can store DID documents directly on the blockchain, construct them dynamically based on a blockchain record, or store a pointer on the blockchain to a document in a decentralized storage network like IPFS or STORJ.

There are DID implementations, but few applications, as it is still new and untested. The current biggest user of DIDs are applications using [3Box](https://3box.io/), as 3Box creates a DID for the user that is associated with their Ethereum address.

- [3ID/Ceramic](https://www.notion.so/3ID-Identity-System-fac2f47862a84602b366af1cd64f3523) - [3ID](https://docs.3box.io/faq/data-identity) is an identity system that links a user's Ethereum address to a DID. They are in the process of migrating to a blockchain-agnostic DID network called [Ceramic](https://github.com/ceramicnetwork/specs#index).
- [Sovrin](https://sovrin.org/) - Sovrin is a permissioned blockchain identity network that implements DIDs. Consensus in the Sovrin network is maintained by approved validator nodes.
- [uPort](https://www.uport.me/) - Uport is a DID implementation built on Ethereum.
- [ION](https://techcommunity.microsoft.com/t5/identity-standards-blog/ion-booting-up-the-network/ba-p/1441552) is a Microsoft-led DID system. It is an implementation of [Sidetree](https://github.com/decentralized-identity/sidetree), a blockchain-agnostic DPKI protocol, that runs on Bitcoin. It stores transaction data in IPFS.
- IBM - IBM is helping to create, operate and maintain [permissioned decentralized identity networks](https://www.ibm.com/blockchain/solutions/identity/networks) that implement DIDs, built using Hyperledger, IBM's permissioned ledger.

## Key Management

Systems that place identities fully in the hands of users, such as p2p systems, blockchain identity systems, and DIDs, encounter the problem of key management. Providing a key management method that is secure yet convenient for users is a major design challenge. Users commonly lose and forget both passwords and cryptographic keys.

The increasing popularity of cryptocurrencies has created new solutions for secure private key management. The most secure solutions, such as [hardware wallets](https://coinfunda.com/best-cryptocurrency-hardware-wallets/) and [third-party custody services](https://www.investopedia.com/news/what-are-cryptocurrency-custody-solutions/#:~:text=Put%20simply%2C%20cryptocurrency%20custody%20solutions,of%20bitcoin%20or%20other%20cryptocurrencies.), are appropriate for high stakes keypairs that may control large amounts of money, but not suitable for social applications that are accessed more frequently and casually. Web wallets, such as the [Metamask](https://metamask.io/) browser extension for Ethereum, provide a more usable solution for decentralized applications. Many decentralized applications built on Ethereum perform authentication through Metamask. In the long term, a better interface for decentralized applications would rely on key management being handled by the user's browser. New browsers with support for cryptocurrency, such as [Brave](https://brave.com/), handle [key management for multiple wallets](https://support.brave.com/hc/en-us/articles/360035488071-How-do-I-manage-my-Crypto-Wallets-) natively in the browser.

To recover lost keys, users must turn to a third-party. Applications striving for decentralization have attempted to split that trust across multiple parties. Social recovery systems give users a process to place key backups in the hands of trusted friends and family. Some examples of social key recovery implementations include [Dark Crystal](https://darkcrystal.pw/), a user interface for splitting keys into shards that are shared with trusted friends and family, and [Argent](https://medium.com/argenthq/decentralised-and-seedless-wallet-recovery-5fcf7dddd78d), an Ethereum wallet that allows users to back up wallets among "Guardians", which can be trusted people, devices, or third-party services.

The decentralized app ecosystem has also attempted to come up with usability improvements beyond password-based authentication. [Torus](https://tor.us/) is a key management system that allows users to use OAuth with existing user accounts to authenticate with decentralized applications. It uses a Distributed Key Generation protocol and distributes key shards across a network of nodes running a private BFT network, which sends the shards back to be reassembled when the user authenticates. [Magic Link](https://magic.link/) is a service that provides an SDK for applications to easily build email "magic link" logins compatible with private keys. On the backend, it uses DID tokens and delegates key storage to an AWS HSM.

## Reputation & Trust

Reputation in decentralized networks is established using many of the same [mechanisms](http://www.lifewithalacrity.com/2005/12/collective_choi.html) as reputation in centralized networks: ratings, peer connections, and metrics such as follower counts. Reputation systems in decentralized networks also suffer from sybil attacks, spam, and impersonation, addressed below.

## Failure modes

- Sybils and spam - Spam, and the creation of many fake users to carry out attacks or misinformation campaigns, are problems for existing centralized social networks. These problems are also present in decentralized networks, and approaches to combat them are still evolving. Federated architectures allow server administrators to intervene and block or filter malicious accounts. However, ongoing harassment and abuse through sockpuppet accounts in Mastodon has motivated research into directions such as [OCapPub](https://gitlab.com/spritely/ocappub/blob/master/README.org), an object-capability based version of ActivityPub. Steemit, a blockchain social network, requires new user registrations to be approved by a centralized service in order to combat the problem of fake accounts created to rig the voting system that determines monetary rewards for posts. Aether requires a hash computation to be performed for every event posted, raising the computational power required to mass spam the network.

- Impersonation - Attempts to impersonate users for purposes of fraud or defamation are widespread on centralized social networks. This threat also exists in decentralized social networks. Networks that do not have an unique human-readable identifier, such as ssb, are likely more vulnerable to this form of attack, as comparison of public keys is a complex behavior that users are not familiar with. In federated naming systems that include a server address in the username, the full address including the server name must be displayed to users in cases where there may be possible collisions.

## Links

- [OAuth](https://en.wikipedia.org/wiki/OAuth)
- [Decentralizing the Social Web](https://hal.inria.fr/hal-01966561/document)
- [What are Decentralized Identifiers](https://www.evernym.com/blog/what-are-decentralized-identifiers-dids/)
- [DIDs](https://github.com/didecentral/didecentral.github.io)
- [DID Primer](https://github.com/WebOfTrustInfo/rwot5-boston/blob/master/topics-and-advance-readings/did-primer.md)
- [Rebooting the Web of Trust Papers](https://decentralized-id.com/literature/rebooting-web-of-trust/)
- [DKMS](https://github.com/hyperledger/aries-rfcs/blob/master/concepts/0051-dkms/dkms-v4.md)
