```
shortname: 0/GLOSSARY
name: Data Ecosystem Glossary
type: Reference
status: Draft
editor: Mike Anderson <mike.anderson@dex.sg>
contributors: 
```

#Glossary

This Glossary provides definitions of common terms used in DEPs.

## Agent

A network-connected service operated by a participant of the data ecosystem that is able
to offer one or more DEP Standard APIs to other participants in the ecosystem.

Typically, an Agent will be server-side software hosted in a virtual environment managed
by the Agent's operator. Open source reference implementations of Agents are available
for operators to configure and deploy if they wish, or alternatively operators may wish
to develop their own Agents - the only requirement is conformance to the DEP Standard APIs.

## Data Ecosystem

An interconnected network of participants who are able to interoperate using standard protocols
such as those defined in the DEPs

## Data Supply Line

An orchestrated set of data flows that creates value in a Data Ecosystem. A Data Supply Line
is designed to be executed by one or more Agents, can transfer Data Assets between different
participants, and can transform Data Assets through the use of Operations to create new Data
Assets.

## DID

A *Decentralised Identifier* conforming to the W3C DID spec.

DIDs are used to refer to Agents and Assets in the Decentralised Data Ecosystem. DIDs may also
be assigned to individuals, enabling self-sovereign identity management

See: https://w3c-ccg.github.io/did-spec/

## DEP

Acronym for "Data Ecosystem Proposal", a document defining open standards for the data economy.

## Starfish

Starfish is an open source developer toolkit for building applications and use cases with
decentralised Data Supply Lines


# License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
