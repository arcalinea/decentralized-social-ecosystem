# Discovery

Being able to discover great content is key to an engaging social network. This section covers methods of content discovery in decentralized social networks.

Decentralized social networks take two main approaches to content discovery: "local is better", or "share everything". "Local is better" applications, such as Mastodon and ssb apps, embrace a small-world approach as part of their design philosophy. They do not attempt to offer universal search or content recommendation, as they are not trying to replicate the experience of centralized social apps. "Share everything" approaches, such as Aether, or applications that treat a blockchain as the database, instead choose to share all of the data among all of the peers.

## Curation

Most decentralized social networks use a chronological feed, or rely up on upvotes and downvotes to surface content.

### Mastodon

Mastodon's feed is chronological. Users are presented with three timelines: a home timeline with posts from accounts the user follows, a local timeline with posts from the local instance, and a federated timeline with all posts that have been retrieved from remote instances. Servers store content from users followed by members of the server.

Mastodon [public relays](https://source.joinmastodon.org/mastodon/pub-relay) rebroadcast anything sent to it to anyone who subscribes to the pub.

### Ssb

Content is propagated and discovered through follow relationships in the ssb network. When a follow relationship is initiated, the posts of the user being followed begins to be synced to the follower's node. Those messages and files are stored locally on the user's computer, indefinitely, for applications running ssb to read.

## Search

In decentralized networks, whether federated or p2p, there is often no global search functionality, as no node has a unified view of the network.

### Mastodon

Mastodon has no global search functionality. A Github issue discussing global indexing: https://github.com/tootsuite/mastodon/issues/9529

To overcome the difficulties of new users finding people to follow to get connected to the network, a community-built tool [Trunk](https://communitywiki.org/trunk/), exists to helps users find and follow people by category. Users have requested a directory for [importing friends from other networks](https://github.com/tootsuite/mastodon/issues/11886). Mastodon users used to be able to find their Twitter friends using `bridge.joinmastodon.org`, but the service was shut down after Twitter API keys were lost and not reissued.

### Ssb

A search of the network will yield results from the data set the user's node has access to.

### Decentralized Search Engines

[Yacy](https://yacy.net/) is a decentralized search engine created in 2003. Users can define their own web index and start a web crawl.

Blockchain-based search engines include [Almonit](https://gateway.ipfs.io/ipfs/QmeQRcd3fQzTdL5U5RXUVvSAa8mvCcWHzwNT29VDjR5nh1/search/engine/almonit-search-engine-preview.html), Presearch, and BitClave.
