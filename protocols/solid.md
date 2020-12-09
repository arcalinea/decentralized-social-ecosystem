# Solid

Solid (derived from "social linked data") is a proposed set of conventions and tools for building decentralized social applications based on Linked Data principles. It relies as much as possible on existing W3C standards and protocols, tying them together into a common framework.

The main concept behind Solid is that users store their personal data in a "pod" (personal online data store), and applications request access to it, rather than storing user data and accounts on an application server.

### Identity

Solid uses WebID URIs as universal usernames. The WebID URI's primary function is to point to the location of a public WebID Profile document.

Example WebIDs: `https://alice.databox.com/profile/card#me` or `http://somepersonalsite.com/#webid`

A WebID is a globally unique, decentralized identifier. It enables cross-service federated sign-in, and does not tie a user's identity to a server. A WebID Profile Document is in Linked Data format, and contains identity information such as a username, profile image, preferences, and public key certificates.

Solid requires cross-domain, de-centralized authentication mechanisms not tied to any particular identity provider or certificate authority, so it uses the WebID-TLS protocol instead of passwords. Instead of creating an account with a username and password for each service, a user selects a security certificate for the site from a popup. The server matches the private key stored by the user's browser with the public key stored in that user's WebID Profile Document to authenticate them.

Username and password authentication mechanisms are an active area of research. Solid recommends that servers implement secondary account recovery mechanisms, such as email recovery, in case browser certificates are lost.

There is some discussion of [using DIDs in addition to WebIDs](https://github.com/solid/identity-panel/issues/1).

### Network

Solid allows inboxes for any resources, such as actors or articles. An example of an inbox for [annotations related to a particular article](https://linkedresearch.org/annotation/csarven.ca/%23i/87bc9a28-9f94-4b1b-a4b9-503899795f6e).

Solid provides a [HTTPS REST API](https://github.com/solid/solid-spec/blob/master/api-rest.md) and a [Websockets API](https://github.com/solid/solid-spec/blob/master/api-websockets.md) for a PubSub mechanism. Smart clients currently retrieve messages with pull/get, rather than through server push.

Nofitications use the [Linked Data Notifications](https://www.w3.org/TR/ldn/) standard.

### Data

A data storage space is called a "pod" (personal online data store). All a user's data is stored in their pod. Users may self-host their pods (on their own server) or use a "pod Provider", a federated pod server. Applications read and write data into the pod depending on the authorizations granted by the users associated with that pod. A person may have multiple pods, for example for work and for home. Data may be replicated across pods. A key concept of the pod is that users may switch Pods easily without losing their data (such as their contacts and chat history).

Solid [strongly encourages](https://github.com/solid/solid-spec#content-representation) the use of RDF-based Linked Data (RDF in the form of JSON-LD, Turtle, HTML+RDFa, etc) for interoperability with the ecosystem. The Solid [content representation spec](https://github.com/solid/solid-spec/blob/master/content-representation.md) encourages consistent naming conventions.

An example payload:

```
{
  "@context": {
    "@language": "en",
    "sioc": "http://rdfs.org/sioc/ns#",
    "foaf": "http://xmlns.com/foaf/0.1/"
  },
  "@id": "",
  "@type": "sioc:Comment",
  "sioc:reply_of": { "@id": "http://example.org/article" },
  "sioc:created_at": {
    "@type": "http://www.w3.org/2001/XMLSchema#dateTime",
    "@value": "2015-12-23T16:44:21Z"
  },
  "sioc:content": "This is a great article!",
  "sioc:has_creator": {
    "@id": "http://example.org/profile",
    "@type": "sioc:UserAccount",
    "sioc:account_of": { "@id": "http://example.org/profile#alice" },
    "sioc:avatar": { "@id": "http://example.org/profile/avatar.png" },
    "foaf:name": "Alice"
  }
}
```

### Social & Discovery

Solid aims to create tools that enable the building of interoperable decentralized social applications. WebIDs allows user addressing across all Solid-conforming apps. Resources (including WebIDs) are self-describing, hosted on pods, and have HTTP endpoints with standard naming conventions to make discovery easier. Any app that can parse the format can read the inbox and send responses.

### Privacy & Access Control

A user's data is stored on their pod, which applications request access to. A user grants access to their data by selecting a security certificate to use with an application. If the user revokes access of an app to their data, the app will no longer be able to read or update the user's data, but would still have access to the data that the user sent out prior to the revocation.

Data is always encrypted when in transition between an app and a pod. Pod providers can encrypt data at rest on a pod, but are not required to.

Solid uses the [Web Access Control Spec](http://solid.github.io/web-access-control-spec/) for management of access control lists.

### Interoperability

Solid provides [specs and recommendations](https://github.com/solid/solid-spec#social-web-app-protocols) for interoperability between Solid ecosystem social applications. There are weekly meetings for the [Solid Data Interoperability Panel](https://github.com/solid/data-interoperability-panel) to discuss data interoperability across applications.

Solid is potentially compatible with federated social applications. For example, a Solid pod could be used in the implementation of an ActivityPub server. The minimum requirement to be conformant with the Solid protocol is the use of JSON-LD, which ActivityPub uses. Dokieli (a solid app) is able to send notifications to ActivityPub. A [discussion of the links between ActivityPub and Solid](https://socialhub.activitypub.rocks/t/which-links-between-activitypub-and-solid-project/529/8) highlights similarities, although work is required to close the gap between the specs.

### Governance & Business Models

Pod providers can choose whether to charge for hosting a pod. Possible business models include charging users for storage or advertising through applications.

[Inrupt](https://inrupt.com/faq) is a company committed to the development of Solid. It plans to monetize through consulting and pod hosting fees.

### Implementations & Applications

A list of [Solid Apps](https://solidproject.org/use-solid/apps). Apps built to use Solid data pods are considered part of the Solid ecosystem.

Solid does not have many widely used social applications yet, although there has been research such as [A Demonstration of the Solid Platform for Social Web Applications](http://crosscloud.org/2016/www-mansour-pdf.pdf).

An example application is:

- [dokieli](https://dokie.li/) for publishing articles and posts

### Links

- [Solid](https://solid.mit.edu/)
- [Solid spec](https://github.com/solid/solid-spec)
- [Solid repo](https://github.com/solid/solid)
- [Solid paper](http://emansour.com/research/lusail/solid_protocols.pdf)
- [Solid Community Group Meetings](https://www.w3.org/community/solid/wiki/Meetings)
