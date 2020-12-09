# Moderation

One of the most acute problems with centralized platforms is the need to develop one-size-fits-all moderation policies for billions of users. Decentralizing moderation puts decisions about what content to block or allow in the hands of users and communities. Community-level moderation can happen through servers, chatrooms, or topic-based communities. User level moderation can allow users to opt into different content preferences, or control their interactions with other users.

### Matrix

[Moderation in Matrix](https://matrix.org/docs/guides/moderation) happens at three levels: Server administrators, chatroom moderators, and users.

Servers in Matrix have terms of use that users agree to when they join. Chatroom moderators can remove people from individual rooms. Users themselves can filter out content they don't want to see.

Most moderation takes place at the room level. Rooms have moderators who can remove undesirable content. A redaction, or 'remove message', can request all participating servers to remove a message. Users can also remove their own messages. However, due to the decentralized nature of Matrix, if the client does not remove the redaction, the message will not be removed. Moderators can kick or ban users from rooms. They can also ban servers from rooms, in cases where malicious traffic must be avoided.

Users in a room have 'power levels', a number between 0 and 100 that indicates how much power the user has in the room. A higher level has more permissions. Moderators have ability to change power level permissions. Users can report abusive content to the server admin, which has the ability to remove it. Users can also block other users to prevent them from contacting them.

### Mastodon

Moderation rules in Mastodon are local to each server. Each server admin can create their own moderation rules as well as a theme for their server. Their TOS may include rules about whether data can leave the server, etc. Users choose which server to join, opting into the moderation policy, theme, and TOS they prefer. [Moderation actions](https://docs.joinmastodon.org/admin/moderation/) can be applied to individuals, or entire instances.

Community created [guides](https://noelle.codes/so_youre_a_mastodon_moderator.pdf) for new Mastodon moderators provide a sample of problems faced and tools available to address them. Some documented [challenges with moderation in Mastodon](https://nolanlawson.com/2018/08/31/mastodon-and-the-challenges-of-abuse-in-a-federated-system/amp/) include heavy burdens on admins and moderators to address harassment and spam. Open APIs to help with moderation were [added to Mastodon in 2019](https://github.com/tootsuite/mastodon/pull/9387), to help admin and developers build custom tooling for moderation.

### Reddit

Reddit is a centralized social platform, but takes a decentralized approach to moderation by allowing moderators of individual subreddits to make the majority of moderation decisions. The creator of a subreddit becomes the head moderator, and can delegate to other moderators. Reddit Admins, are employees of the company with "supreme mod" authority over content on the site.

To help community moderators, Reddit has developed an automated tool, [AutoModerator](https://www.newamerica.org/oti/reports/everything-moderation-analysis-how-internet-platforms-are-using-artificial-intelligence-moderate-user-generated-content/case-study-reddit/), to help proactively identify, filter, and remove objectionable content. Many moderators also create or use [custom moderation bots](https://www.reddit.com/r/modguide/comments/et39hl/custom_moderation_bots/).

[Notabug](https://notabug.io/t/notabug/comments/59382d2a08b7d7073415b5b6ae29dfe617690d74/welcome-to-notabug), a decentralized Reddit alternative, has a different moderation structure. Posts are created in any "topic", without requiring a subreddit for a topic to be created beforehand. "Spaces" are more like subreddits, and can accept or block content, but they do not affect the original posts in the topics. A given topic can have multiple spaces which curate and moderate differently. The automated rules for how spaces filter posts can be publicly inspected.

### Aether

Each community has its own moderators, which communities elect or impeach themselves. Like subreddits, communities largely govern themselves. Mods (moderators) can approve or delete posts.

Users can report posts. Reports go to mods of the community. Users elect moderators in communities, and can impeach them. If a user votes to impeach a mod, the mod's actions will be ineffective for that user, even if the vote wasn't won. All mod decisions are public history.

Moderator elections begin once a community has more than 100 active users within the last 2 weeks. Voting thresholds for moderators is a 51% positive vote of at least 5% of the community's members. To prevent hostile takeovers, the population of communities is determined on a 2-week trailing basis, and mods can temporarily lock a community to new entrants.

Users who wish to be mods can enable 'mod mode', which allows them to proactively attempt to approve or delete posts. Their moderation history can be observed by others considering electing them as an official mod.

### Ssb

Moderation in ssb depends on a bottom-up approach driven by users. In the ssb ecosystem, users block, flag, or ignore other users that they find objectionable. An ignore will simply not show that data to the user's node, although their node will continue to pass their data through the network. A block will cause the user's node to refuse to replicate data from that feed, segmenting it off from their portion of the network. If enough people block a user or group of users, their part of the network will become partitioned from the rest.

### Steemit

Steemit takes a bottom-up approach to moderation. Content is moderated](https://steemit.com/steem-standards/@arhag/moderation-standard) through the up and down votes of users, instead of through the actions of a moderator. Users can earn a curation reward for upvoting posts that later become popular. Low voted content will earn less rewards, and may be hidden. Votes are weighted by reputation, which accumulates with age, so older accounts of early adopters have more power in the network.

Steemit's approach to spam, plagiarism, and abuse relies on a single mechanism: user voting to downgrade undesirable posts. To add a negative signal to a post and downgrade its rewards, users could ["flag" or "downvote"](https://steemit.com/utopian-io/@steemcleaners/understanding-flagging-downvoting) - two terms used for the same function of downgrading a post. A discussion of the [downvoting and flagging problems](https://steemit.com/community/@baah/a-solution-to-the-downvoting-flagging-problems-on-steemit) on Steemit goes into the drawbacks of various approaches.

Users can flag content they find objectionable, but they [cannot block other users](https://github.com/steemit/steem/issues/382), due to the potential for abuse of the block feature in a system that monetizes upvotes. For example, a user might block users with high reputation who might downvote them, so they could upvote their own posts within a circle of participating accounts.

## Experimental Approaches
