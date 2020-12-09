# Aether

Aether is a Reddit-like p2p social network - a public discussion forum designed with ephemerality and mutability in mind. It is available as a desktop application. Aether's p2p protocol is called Mim. In practice, Aether is the only application using Mim, but it could support other applications.

### Identity

Identities in Aether are based on keypairs, like in ssb. As with ssb, users can choose a custom nickname, although it is not unique. Multi-device usage is currently possible, but difficult (requires manually porting a user config file). There is a strategy for multiple device logins under development that would allow users to store and sync encrypted keys from a remote backend.

### Network

Aether is a flood network where every node stores everything. All nodes download content as well as provide it to other nodes. Communities with objectionable content can optionally be blocked by the user, so that content is not stored on their machine.

Aether comes with a list of bootstrap nodes. Nodes find each other through a probabilistic search over a network table. This is a random process in which no patterns of data distribution can be discerned, so only the last hop of where data came from is visible, not the source. Once nodes are connected, they sync their data.

Syncing nodes request caches of data from others. Every node stores pre-packaged answers to common queries. Data from each endpoint (boards, threads, posts), will be cached every day, and stored for 7 days, after which it is no longer cached. Bootstrapping nodes do not download the entire history of the network - only the last 7 days by default.

New posts have a 0 to 10 minute delay, as it takes time to traverse the p2p network.

### Data

Aether's data structure is a DAG (directed acyclic graph). It allows insertion and updates of content. Deletion of content is achieved by inserting a blanck update. The backend keeps a local SQLite database up-to-date with the state of the network. The frontend compiles and presents the graph data.

Posts are ephemeral. After a certain period of inactivity on a post (the default is 6 months from the last reference), it automatically gets deleted by nodes on the network. If a post continues to referenced, it stays up, so actively discussed topics do not disappear.

### Moderation & Reputation

Each sub-community has its own moderators, which communities elect or impeach themselves. Like sub-reddits, communities largely govern themselves. Mods (moderators) can approve or delete posts.

Users can report posts. Reports go to mods of the community. Users elect moderators in communities, and can impeach them. If a user votes to impeach a mod, the mod's actions will be ineffective for that user, even if the vote wasn't won. All mod decisions are public history.

Moderator elections begin once a community has more than 100 active users within the last 2 weeks. Voting thresholds for moderators is a 51% positive vote of at least 5% of the community's members. To prevent hostile takeovers, the population of communities is determined on a 2-week trailing basis, and mods can temporarily lock a community to new entrants.

Users who wish to be mods can enable 'mod mode', which allows them to proactively attempt to approve or delete posts. Their moderation history can be observed by others considering electing them as an official mod.

An allow-list is used to moderate the collection of communities. When users join, the default is to only display SFW communities on a vetted allow-list, to ensure the safety and comfort of a user's initial experience.

### Spam

Aether combats spam by requiring users to complete a small "proof-of-work", a brief hashing function, before they post. This rate-limits the amount of posts that a user can send out to the network, making it more expensive to flood it with spam.

### Social & Discovery

Users can search communities, content, and people. The user interface has tabs to discover newest content and popular content.

There is no follow functionality for other users at present.

### Privacy & Access Control

All posts are public.

### Monetization

The Aether p2p version is supported through Aether Pro, a SaaS version of Aether designed for private work deployments.

### Interoperability

Aether p2p is working on implementing a chat that bridges to Matrix p2p.

### User Experience & Scalability

Aether is currently only available on desktop. There is no mobile app, as mobile devices have limited disk space and energy stores to run the p2p network.

However, a mobile app is under development that uses the following method: Running an Aether host in a docker image on a home server, and allowing mobile devices to connect to it.

Creating a mobile app in a way that requires users to run home servers, rather than simply providing full nodes that would allow a mobile app to download content, is a design decision that maximizes decentralization at the expense of user convenience. Every mobile user running a home server will also still be providing capacity to the network to help it scale in a purely p2p manner.

### Links

- [Mims protocol](https://getaether.net/mim-docs/)
