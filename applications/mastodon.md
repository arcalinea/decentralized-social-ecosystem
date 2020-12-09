# Mastodon

Mastodon is a federated Twitter alternative, released in 2016. It is the most popular client using the [ActivityPub](../protocols/activitypub.md) federation protocol.

Each server is called an "instance". The entire constellation of instances that can interoperate is called the “Fediverse”.

### Identity

Users create an account on an instance, but can communicate with users on other instances. A full username is a user’s handle plus the name of the instance the user belongs to, for example:

```
@alice@mastodon.social
```

Usernames are unique to each instance, not to Mastodon as a whole, so `@alice@mastodon.social` can coexist with `@alice@modern.town`.

If a user moves to a new instance, they can redirect or migrate their old account. Redirection sets up a redirect notice on the old profile which tells users to follow the new account. Migration forces all followers to unfollow the old account and follow the new, if the software on their instance supports this functionality. Previous posts will not be moved.

Since user identities are tied to instances, if [an instance goes down](https://indieweb.org/witches.town), user accounts and data go with it if they are unable to migrate.

Account credentials are managed by the user’s instance, so if users forget their password, they can ask for a password reset. Whether users can delete their own accounts or not is a setting dependent on the instance admin.

For user verification, there is no central authority to check identity documents, but link-based verification can help cross-reference links associated with a user. For example, a user can link to their Mastodon profile from their personal homepage, and receive a verification checkmark on their Mastodon profile by their personal homepage link, to confirm that they are the owner. An identity proof framework was added in 2019, which currently only supports Keybase. It allows users to [link their Keybase cryptographic identity](https://github.com/keybase/keybase-issues/issues/2948) to their Mastodon account.

Mastodon uses [Webfinger](https://docs.joinmastodon.org/spec/webfinger/) to translate user mentions to actor profile URIs. Webfinger specifies the path at which to find a user's profile information provided by the server. The Webfinger endpoint is always under `/.well-known/webfinger`, and it receives queries such as `/.well-known/webfinger?resource=acct:alice@mastodon.social`.

### Network

Mastodon started out using OStatus for federation, and later switched to ActivityPub. [ActivityPub](../protocols/activitypub.md) is both a server-to-server and client-to-server standard. Mastodon only implements the server-to-server protocol, which is all that is necessary for federation with other ActivityPub servers. The ActivityPub "activities" Mastodon implements are documented [here](https://docs.joinmastodon.org/spec/activitypub/).

### Data

Mastodon is a Ruby on Rails application that uses PostgreSQL and Redis for data storage.

### Moderation & Reputation

Moderation takes place at the server level in Mastodon. Each instance sets its own moderation policies, either through the unilateral decisions of an admin, or through collective discussion and agreement. Admins can ban entire instances, cutting off their visibility. If an instance gets banned by many others, its users can still talk with each other, but they will be isolated from the rest of the Fediverse.

Users can apply content warnings to their posts themselves, to indicate the nature of the content and hide it behind a click.

Users can report posts to moderators, submitting it for a moderation decision.

Some documented [challenges with moderation in Mastodon](https://nolanlawson.com/2018/08/31/mastodon-and-the-challenges-of-abuse-in-a-federated-system/amp/) include heavy burdens on admins to address harassment and spam. Open APIs to help with moderation were [added to Mastodon in 2019](https://github.com/tootsuite/mastodon/pull/9387), to help admin and developers build custom tooling for moderation.

### Social & Discovery

Mastodon users are presented with three timelines: a home timeline with posts from accounts the user follows, a local timeline with posts from the local instance, and a federated timeline with all public posts that the user's server has received from remote instances. Users essentially have access to posts of people followed by people on their instance. Feeds are chronological, non-algorithmic, and contain no ads.

There is [no universal search in Mastodon](https://github.com/tootsuite/mastodon/issues/9529), as each server monitors a different set of messages. Searching for the same keyword on different instances yields different results. To aid content discovery, Mastodon has [public relays](https://source.joinmastodon.org/mastodon/pub-relay) which rebroadcast anything sent to it to anyone who subscribes to the pub.

A "Profile Directory" tab allows users to browse recently active profiles or new arrivals from both their own instance or the fediverse to discover who to follow. [Trunk](https://communitywiki.org/trunk/) is a community-built tool that helps users find and follow people by category.

Users can select featured hashtags to be displayed on their public profile so people can browse their posts by hashtag.

### Privacy & Access Control

Posts can be public, unlisted, private, or direct. Public posts are shown on local, federated, and hashtag timelines. Unlisted posts are not, but can be accessed through the link. Private posts are only visible to followers. Direct posts can only be seen by the people mentioned in them. Users can also hide their network, so the lists of followers and people they're following are not visible. Servers only receive posts that are public or addressed to a person on that server.

Mastodon can be served through Tor as an onion service.

Mastodon is currently [adding e2e encryption](https://github.com/tootsuite/mastodon/pull/13820). Previously, direct messages were unencrypted.

### Monetization

Federated social networks require both hosting and development costs to maintain. Each instance is funded by its own administrator and community. Mastodon’s development is funded through a Patreon run by the main developer, Eugen Rochko. It currently brings in about 70k a year, which supports him working on it full time, and covers hosting costs and a moderation team for the popular mastodon.social instance.

### User experience

Mastodon's main initial selling point was its familiar interface that behaved like Tweetdeck. Prior to Mastodon, projects like GNU social and Diaspora tried and failed to get mainstream adoption. Mastodon succeeded in large part because it created a familiar user interface that looked and behaved like Twitter, allowing users who were dissatisfied to find a new place to land with minimum effort.

Notable design choices in Mastodon that differ from Twitter: Instead of a 280 character limit, there is a 500 character limit. "Likes" are not broadcast to third-parties. "Retweet" and "Like" numbers are not shown until a post is clicked on. If a user with an unlocked account gets a follow request from an account that has been silenced by the server's moderators (either manually, or at their domain), the user will get a follow request instead of automatically allowing the new account to follow.

User-level content controls include: Users can set content warnings on their own posts. Filtering on posts can automatically hide keywords and phrases. "Boosts" (retweets) from someone can be hidden. Accounts can be muted or blocked. Entire servers, including all of its posts, can be blocked by a user. Following, muting, blocking, and domain-blocking lists can be imported.

Upload options include: Users can choose where the thumbnail of a picture is focused when opening. Users can enter custom alt-text to image uploads. There is an OCR button to help extract text from an image for the visually impaired.
Audio files can be uploaded. Admins can upload custom emojis to their servers.

### Interoperability

Mastodon is compatible with all federated applications that use ActivityPub. These include Pleroma, another social application, PixelFed, a photo-sharing application, and PeerTube, a video-sharing application. Mastodon users can follow a Pleroma, PixelFed, or PeerTube user from Mastodon.

Mastodon users used to be able to find their Twitter friends using `bridge.joinmastodon.org`, but the service was shut down after the developer lost access to API keys and was not granted another set.

All Mastodon user data is available for export.

### Scalability

Mastodon.Social, the instance started by Mastodon's main developer, initially became the server of choice for new users. The server architecture was [scaled up](https://blog.joinmastodon.org/2017/04/scaling-mastodon/) several times, until new registrations were closed in 2017. He has now started mastodon.online to support more registrations. Anyone can run a Mastodon instance, but in practice, the majority of users have concentrated within a few.

When a surge of new users join an instance, server admins can run into scaling issues, as any web host who becomes unexpectedly popular does.

Mastodon hosting providers have emerged as a service to help individuals interested in being admins, but without sysadmin experience, to spin up servers.

A [2019 analysis](https://emilianodc.com/PAPERS/mastodonIMC19.pdf) of the Mastodon ecosystem found that the majority of posts are concentrated on a few instances, and outages in 10 instances would remove almost half of all posts from the network.

### Metrics

Mastodon has around 2.9 million accounts and 2785 nodes, as of June 2020, according to [fediverse.network](https://fediverse.network/mastodon). From Mastodon, 3.8 million accounts can be reached within the Fediverse.

### Links

- [Official website](https://joinmastodon.org/)
- [Documentation](https://docs.joinmastodon.org/)
