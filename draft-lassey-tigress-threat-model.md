---

title: "TIGRESS Threat Model"
category: info

docname: draft-lassey-tigress-threat-model-latest
submissiontype: IETF
number:
date:
consensus: false
v: 3
area: "Applications and Real-Time"
workgroup: "Transfer dIGital cREdentialS Securely"
keyword:
 - tigress
 - threat model
venue:
  group: "Transfer dIGital cREdentialS Securely"
  type: "Working Group"
  mail: "tigress@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/tigress/"
  github: "bslassey/tigress-threat-model"
  latest: "https://bslassey.github.io/tigress-threat-model/draft-lassey-tigress-threat-model.html"
author:
 -
    fullname: Brad Lassey
    organization: Google
    email: lassey@google.com
 -
    fullname: Casey Astiz
    organization: Apple
    email: castiz@apple.com
 -
    fullname: Dmitry Vinokurov
    organization: Apple
    email: dvinokurov@apple.com
normative:

informative:
  Tigress-req-03:
    author:
    -
      ins: D. Vinokurov
      name: Dmitry Vinokurov
    -
      ins: A. Pelletier
      name: Alex Pelletier
    -
      ins: C. Astiz
      name: Casey Astiz
    -
      ins: B Lassey
      name: Brad Lassey
    -
      ins: Y. Karandikar
      name: Yogesh Karandikar
    -
      ins: B Lassey
      name: Brad Lassey

    title: "Tigress requirements"
    date: 2023-04
    target: https://github.com/dimmyvi/tigress-requirements/

--- abstract

This document describes a threat model by which the working group can evaluate potential solutions to the problems laid out in the [TIGRESS charter](https://datatracker.ietf.org/doc/charter-ietf-tigress/).


--- middle

# Introduction
The TIGRESS Working Group is [chartered](https://datatracker.ietf.org/doc/charter-ietf-tigress/) to deliver a protocol for transferring copies of digital credentials. The charter specifies certain goals:

## Privacy goals:

* The intermediate server should not see sensitive details of the Provisioning Information {{Tigress-req-03}}

* The intermediate server should not be able to provision the credential itself,
acting as an intermediary for the recipient (person-in-the-middle,
impersonation attack)

* Aside from network-level metadata, the intermediate server should not learn
information about the sender or receiver

## Security goals:

* Allow for ensuring that only the intended recipient is able to provision the credential

* Allow for ensuring that the credential can only be provisioned once (anti-replay)

* Allow for ensuring that the sender has the intent to transfer (proof of the fact that the
initiation of the credential transfer is attributed to a valid device and a user)

## Functional goals:
* Allow a sender to initiate a credential transfer and select an intermediary server

* Allow a recipient to view the transfer request with Provisioning Information {{Tigress-req-03}}, and provision the credential information associated with it upon receipt

* Allow a sender and a recipient to perform multiple round trip communications
within a limited time frame

* Not require that both the sender and recipient have connectivity to the intermediary
server at the same time

* Support opaque message content based on the credential type

* Support a variety of types of credentials, to include those adhering to
public standards (e.g., Car Connectivity Consortium) and proprietary (i.e.,
non-public or closed community) formats

From these goals we can derive a threat model for the general problem space.

# Threat Model
## Assets and Data
### Credential
The credential or key that is being transferred via this protocol.
### Intermediary data
Data that is transferred over the course of the transaction.
### Credential transfer invitation
The initial data containing Provisioning Information {{Tigress-req-03}} transmetted to the receiver which represents an invitation to accept the transferred credential.
# Users
## Sender
The user who initiates the credential transfer.
## Receiver
The user who is the intended recipient and accepts the invitation with the transferred credential.
# Attackers and Motivations
# Threats and mitigations

|Threat Description|Likelihood|Impact|Mitigations|
|:-----------------|----------|------|-----------|
|An Attacker with physical access to the victim's phone initiates the transfer of a Credential to the the Attacker's device|MED|HIGH|{user-auth}|
|Attacker intercepts or eavesdrops on sharing message|HIGH|HIGH|{secret-transport}|
|Sender mistakenly sends to the wrong Receiver|HIGH|HIGH|{transfer-control}|
|Sender device compromised|MED|HIGH|{transfer-control}|


## If an intermediary server is used
Some designs may rely on an intermediary server to facilitate the transfer of material. Below are threats and mitigations assuming that there is an intermediary server hosting encrypted content at an "unguessable" location.

|Threat Description|Likelihood|Impact|Mitigations|
|:-----------------|----------|------|-----------|
|Attacker brute forces "unguessable" location|LOW|LOW|{limited-ttl}|
|Attacker intercepts encryption key|MED|MED|{secret-separation}|
|Attacker intercepts encryption key and unguessable location|MED|HIGH|{group-transfer-warning}|
|Attacker compromises intermediary server|LOW|LOW|{mailbox-content-encryption}|
|Attacker uses intermediary server to store unrelated items (i.e. cat pictures)|HIGH|LOW|{mailbox-limits}|


## Mitigations.
### User authentication at the time of transfer initiation {#user-auth}
Implementers SHOULD take sufficient precautions to ensure that the device owner is in possession of the device when initiating a transfer such as requiring authentication at the time of initition.

### Secret to be sent securily {#secret-transport}
Solution should require an end-to-end encrypted messaging channel or otherwise specify a way to send a secret out of band.

### Transfer control {#transfer-control}
Implementers should ensure any initiated attempts of credential transfer can be withdrawn or revoked at any time.

### Limited time-to-live for mailbox storage {#limited-ttl}
Limited TTL of storage, rate limiting of requests.

### Separation of shareURL and secret {#secret-separation}
Separate transmission of encryption key and unguessable location.

### Group transfer warning {#group-transfer-warning}
Implementor should warn users about transferring credentials to groups.

### Encrypted mailbox content {#mailbox-content-encryption}
Content on the server is encrypted.

### Mailbox size limit and TTL {#mailbox-limits}
Intermediary server should have tight size limits and TTLS to discourage misuse

# Conventions and Definitions

{::boilerplate bcp14-tagged}



# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

This document took as inspiration the [threat model](https://github.com/dimmyvi/tigress-sample-implementation/blob/main/draft-tigress-sample-implementation.md#threat-model) that was part of Dmitry Vinokurov's sample implementation document.
