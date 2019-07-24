```
shortname: 5/IDENTITY
name: Agent and Asset Identity
type: Standard
status: Raw
editor: Mike Anderson <mike.anderson@dex.sg>
contributors: 
```

Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Overview](#overview)
   * [Motivation](#motivation)
   * [Specification](#specification)
   * [Rationale](#rationale)
   * [Implementations](#implementations)
   * [License](#license)

## Overview

The W3C DID draft specification provides a basis for decentralised identifiers for 
assets and agents in the decentralised data ecosystem.

*Agents* are identified by a DID, which can be resolved via a Universal Resolver to obtain a DDO 
(DID Document) that describes the Agent.

*Assets* are identified by the cryptographic hash of their metadata. This Asset ID can be combined 
with the DID of an Agent hosting the Asset to create an Asset DID, which fully identifies an
asset in the context of an Agent in the ecosystem.

This DEP does *not* consider the identity of individual data subjects (e.g. individuals whose
personal data appears in one or more data assets). In general, data assets may contain
data related to multiple individuals and the method of encoding / managing / processing such 
data at the individual level will be handled by service providers according to their use cases
and in accordance with legal obligations and relevant regulations.

## Motivation

In a decentralised data ecosystem, it is necessary to have a notion of identity that can be used to 
refer to entities in the ecosystem. 

In particular, these identities should have the following desirable properties:

- They are *stable* - the identifier has meaning for a sufficiently long duration that it is useful to
identify the same entity for the entire duration of an operation or sequence of operations
- They are *unambiguous* - for a given identifier, there is only one entity that is referred to
- They are *practical* in that they help us in the task of orchestrating decentralised data use cases.
- They are *decentralised* in the sense that they are controlled by the owner of the entity in question
and do not depend on a centralised authority.

## Specification

### Agent Identity

An Agent should obtain a W3C DID to use as its identity in the data ecosystem.

An example of a DID on the Ocean Network could be:

`did:op:6064a7d73efef9ff22dbc0a03572763fae0f76e935736b34ed2e1f56aa07bbf5`

The Agent should have a DDO (DID Document) to describe itslf to other parties. This DDO should include:

- Any service endpoints offered by the Agent according to the DEP Standard APIs (see DEP15)
- Any public cryptographic keys that the Agent wishes to share (e.g. for verification purposes)

An Agent's DID / DDO must be registered via a Universal Resolver if the Agent wishes its service 
endpoints to be generally discoverable by other parties in the data ecosystem.

The precise method of DID allocation is open to alternative implementations, however the use
of sufficiently long random hex strings is a pragmatic choice. Univeral Resolver implementations 
must ensure uniqueness of DID in their context, and that ability to update the DID for a given DDO
is restricted to the owner of the DID.

Note: Facilities for obtaining a DID/DDO on the Ocean Network are offered as part of the Starfish 
Developer Toolkit.

### Asset Identity

An Asset must have metadata, and its Asset ID is defined to be the SHA3-256 cryptographic hash of 
its metadata (encoded as a UTF-8 byte sequence), represented in 4 lowercase hex charcaters. 

An example of an asset ID would be:

`840eb7aa2a9935de63366bacbe9d97e978a859e93dc792a0334de60ed52f8e99`

Any actor in the ecosystem may validate the metadata associated with a given Asset ID by computing 
the same cryptographic hash function.

If an Asset is hosted by an Agent, its Asset DID is defined to be the DID of the Agent, to 
which is appended a single slash `/` plus the Asset ID. This is a "DID Path" as defined by the
W3C DID specification.

An example of a Asset DID for the asset above located on the agent described above would be:

`did:op:6064a7d73efef9ff22dbc0a03572763fae0f76e935736b34ed2e1f56aa07bbf5/840eb7aa2a9935de63366bacbe9d97e978a859e93dc792a0334de60ed52f8e99`

## Rationale

We adopt the W3C DID standard because it is a good fit for the use case of decentralised identity, and
shows high potential as an emerging standard.

We consider it valuable to make use of the DID Path to specify an Asset DID for multiple reasons:
- Conceptually, the same Data Asset may be hosted by multiple Agents, and it is frequently 
necessary to identify which instance is being referred to. 
- Without specifying the Agent's network location, it is difficult work out where to obtain an asset from 
(some indirect method would need to be used, e.g. a search engine)
- This is consistent with the W3C DID spec section 4.5: "A DID path SHOULD be used to address resources 
available via a DID service endpoint"

## Implementations

Reference implementations provided by DEX Pte Ltd., including the Starfish developer toolkit libraries,
implement asset and agent identity as described in this DEP.

The Ocean Protocol provides a Universal Resolver capable of registering DID/DDOs and resolving 
DID to DDOs.

## References

* W3C Decentralized Identifiers (DIDs) Specification - https://w3c-ccg.github.io/did-spec/

## License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
