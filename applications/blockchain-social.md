# Blockchain Social Applications

This section covers some aspects of social applications that store data on a blockchain and/or use an associated cryptocurrency for monetization.

# Steemit

The Steem cryptocurrency was created for content monetization in social sites. Steemit, a Reddit/Medium-style social network, was the first site built to use Steem. User identities and post data are stored on the Steem blockchain. There are over a million accounts on Steemit.

### Identity

User identities are stored on the Steem blockchain. There is a monetary incentive to create many accounts to upvote posts, so as a spam and sybil prevention mechanism, new account creation requires an email and phone number, and must go through a centralized review process.

A Steemit account functions as a cryptocurrency wallet, and users are responsible for their own key management. There is no account recovery available, and funds can be lost or stolen if the key is compromised. Accounts [cannot be deactivated or deleted](https://github.com/steemit/condenser/issues/787), since they are permanently stored on the Steem blockchain.

### Data

Text data is stored on the Steem blockchain, but larger data like images are stored off-chain in a database.

### Moderation

Steemit takes a bottom-up approach to moderation. Content is [moderated](https://steemit.com/steem-standards/@arhag/moderation-standard) through the up and down votes of users, instead of through the actions of a moderator. Low voted comments may be hidden. User reputation determines the weight of votes in the network, and since reputation accumulates with age, older accounts have more voting power.

Steemit's approach to spam, plagiarism, and abuse relies on a single mechanism: user voting to downgrade undesirable posts. To add a negative signal to a post and downgrade its rewards, users could ["flag" or "downvote"](https://steemit.com/utopian-io/@steemcleaners/understanding-flagging-downvoting) - two terms used for the same function of downgrading a post. A discussion of the [downvoting and flagging problems](https://steemit.com/community/@baah/a-solution-to-the-downvoting-flagging-problems-on-steemit) on Steemit goes into the drawbacks of various approaches.

Users can flag content they find objectionable, but they [cannot block other users](https://github.com/steemit/steem/issues/382), due to the potential for abuse of the block feature in a system that monetizes upvotes. For example, a user might block users with high reputation who might downvote them, so they could upvote their own posts within a circle of participating accounts.

### Monetization

Steemit mined 80% of Steem in the first week. Steemit benefited from the appreciation of Steem during the 2017 cryptocurrency run-up, but had to lay off much of its staff when the price of cryptocurrency declined.

Other than depending on Steem price appreciation, Steemit monetizes through users promoting their posts. When users perform certain actions on Steemit, they earn Steem. Creating posts that get upvoted qualifies users to earn from a rewards pool. Upvoting posts that later become popular can earn voters a curation reward. Votes are weighted by reputation, which accumulates with age, so older accounts of early adopters have more power in the network. This, as well as the fact that Steem tokens could be mined easily early on, means that Steemit’s incentives are geared towards early adopters.

### Links

[Steemit frontend application](https://github.com/steemit/condenser)

# Peepeth

[Peepeth](https://peepeth.com/welcome) is a “permanent Twitter” built on Ethereum and IPFS. Users log in to Peepeth through [Metamask](https://metamask.io/), and their identities are registered by posting account information to a smart contract that associates it with their address. Account information is stored on the Ethereum blockchain.

Post data is stored in IPFS, and a link to the IPFS address is stored on Ethereum. Permanent posts are embraced as a design choice meant to encourage users to be more mindful of what they write.

The main Peepeth client has a few monetization strategies, including a 10% transaction fee on user tips, and charging for verifications of social accounts.

# Bitcoin Fork Social Networks

Memo is a Twitter-style social network that stores each action in a transaction on the Bitcoin Cash blockchain. Data stored on the blockchain is immutable and public. Memo uses a reputation system based on the ratio of how many people a user follows are mutuals, compared with how many have muted the user.

Twetch is a Twitter alternative that stores post data on the Bitcoin SV blockchain. Almost all actions are monetized, and have a transaction cost. Users create a [moneybutton](https://www.moneybutton.com/money) account for sending and receiving money, and must fund their account to write, comment, like, or follow. There is a small charge to create a post, as a spam prevention mechanism. A cross-post to Twitter feature exists in Twetch.

# Links

[Blockchain Social Networks](https://medium.com/decentralized-web/blockchain-social-networks-c941fb337970)
[When Blockchain meets Online Social Networks](https://www.sciencedirect.com/science/article/abs/pii/S1574119220300195)
