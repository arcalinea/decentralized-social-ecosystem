# Ssb Social Applications

This section is a more in-depth examination of the user experience of social applications in the ssb ecosystem.

Desktop applications include [Patchwork](https://handbook.scuttlebutt.nz/applications#patchwork), and mobile applications include [Manyverse](https://handbook.scuttlebutt.nz/applications#manyverse) and [Planetary](https://planetary.social/).

Manyverse is the first mobile application for ssb, and the first ssb app that implemented [DHT invites](https://gitlab.com/staltz/ssb-dht-invite). Users can connect through upbs, over LAN, through a DHT, or over bluetooth.

## User Experience

Key management is one of the biggest challenges of ssb, as users often lose and forget their passwords. Users are in complete control of their identity. That means if they lose their cryptographic key, they can permanently lose access to their account. To address the problem of key management in a decentralized manner, a project in the ssb ecosystem, [Dark Crystal](https://darkcrystal.pw), has implemented a social key recovery system. It splits keys into shards to store with family and friends who can be trusted to help reconstruct a lost key.

The p2p bootstrapping process introduces frictions for new users. First, new users typically join a pub to get connected to the network after they download an ssb application. Then, there is a period of waiting time during the initial sync when logs are being downloaded, like the syncing time of a blockchain. A user that has not opened an ssb application in awhile will encounter this synchronization delay again while their node catches up to the state of the network.

The inability to edit or delete content also runs contrary to user expectations. Because of the append-only nature of ssb feeds, there is no ability to permanently delete a piece of content. Applications can work around this by honoring edit or delete messages appended to the feed, but the original content stays in the append-only log that is shared among all nodes, and other applications could choose not to honor such messages. An example of a workaround is [ssb-revisions](https://github.com/regular/ssb-revisions), a basic API that enables applications to use mutable messages by displaying the updated version.
