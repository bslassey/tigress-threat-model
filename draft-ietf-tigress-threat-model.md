---

title: "TIGRESS Threat Model"
category: info

docname: draft-lassey-tigress-threat-model-latest
submissiontype: IETF
number:
date:
consensus: false
v: 3
area: ART
workgroup: TIGRESS WG
keyword:
 - tigress
 - threat model
venue:
  group: TIGRESS
  type: Working Group
  mail: tigress@ietf.org
  arch: https://datatracker.ietf.org/wg/tigress/about/
  github: lassey/tigress-threat-model
  latest: https://github.com/bslassey/tigress-threat-model/edit/main/draft-lassey-tigress-threat-model.md
author:
 -
    fullname: Brad Lassey
    organization: Google
    email: lassey@google.com

normative:

informative:


--- abstract

TODO Abstract


--- middle

# Introduction
The TIGRESS Working Group is chartered to deliver a protocol for transfering copies of digital credentials. The charter specifies certain goals:

## Privacy goals:

* The relay server should not see sensitive details of the share

* The relay server should not be able to provision the credential itself,
acting as an intermediary for the recipient (person-in-the-middle,
impersonation attack)

* Aside from network-level metadata, the relay server should not learn
information about the sender or receiver

## Security goals:

* Ensure only the intended recipient is able to provision the credential

* Ensure the credential can only be provisioned once (anti-replay)

* Ensure the sender has the intent to transfer (proof of the fact that the
share initiation is attributed to a valid device and a user)

## Functional goals:
* Allow a sender to initiate a share and select a relay server

* Allow a recipient to view the share request, and provision the credential
associated with the share upon receipt

* Allow a sender and a recipient to perform multiple round trip communications
within a limited time frame

* Not require that both the sender and recipient have connectivity to the relay
server at the same time

* Support opaque message content based on the credential type

* Support a variety of types of credentials, to include those adhering to
public standards (e.g., Car Connectivity Consortium) and proprietary (i.e.,
non-public or closed community) formats

From these goals we can derive a threat model for the general problem space.

# Threat Model
## Assets and Data
### Credential
The credential or key that is being shared via this protocol.
### Intermediary data
Data that is shared over the course of the transaction.
### Share invitation
The initial data shared with the reciever which represents an invitation to share a credential.
# Users
## Sender
The user who initiates the share.
## Receiver
The user who is the intended recipient and accepts the invitation to share a credential.
# Attackers and Motivations
# Threats and mitigations

|Threat Description|Likelihood|Impact|Mitigations|
|:-----------------|----------|------|-----------|
|An Attacker with physical access to the victim's phone initiates a share of a Credential to the the Attacker's device|MED|HIGH|Implementors SHOULD take sufficient precautions to ensure that the device owner is in possession of the device when initiating a share such as requiring authentication at share time|
|Attacker intercepts or eavesdrops on sharing message|HIGH|HIGH||
|Sender mistakenly sends to the wrong Receiver|HIGH|HIGH||
|Sender device compromised|MED|HIGH||


## If an itemediary server is used
Some designs may rely on an intermediary server to facilitat the transfer of material. Below are threats and mitigations assuming that there is an intermidary server hosting encrypted content at an "unguessible" location.

|Threat Description|Likelihood|Impact|Mitigations|
|:-----------------|----------|------|-----------|
|Attacker brute forces "unguessible" location|LOW|LOW|Limited TTL of storage, rate limiting of requests|
|Attacker intercepts encryption key|MED|MED|Seperate transimission of encryption key and unguessible location|
|Attacker intercepts encryption key and unguessible location|MED|HIGH|Implementor should warn users about sharing credentials to groups|
|Attacker compromises itermediary server|LOW|LOW|Content on the server is encrypted|
|Attacker uses itermediary server to store unrelated items (i.e. cat pictures)|HIGH|LOW|Itermediary server should have tight size limits and TTLS to discourage misuse|



# Conventions and Definitions

{::boilerplate bcp14-tagged}



# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

This document took as inspiration the [threat model](https://github.com/dimmyvi/tigress-sample-implementation/blob/main/draft-tigress-sample-implementation.md#threat-model) that was part of Dmitry Vinokurov's sample implementation document.
