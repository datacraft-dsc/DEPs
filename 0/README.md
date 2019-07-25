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

## Consumer

A participant in the data system who acquires and utilises Assets in the Data Ecosystem.

## Data Asset

A subtype of Asset that represents a single data object that can be utilised in the data ecosystem.

A Data Asset may have any content format, at the discretion of its Publisher. However it is recommended
that Publishers make use of existing standard data formats (e.g. CSV files) to maximise utility to 
Consumers

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

## Ocean Network

The Ocean Protocol Foundation provides the Ocean Network as a public service utility to the 
data economy. The Network provides important capabilities for decentralised data exchange,
including:
- Value exchange / transactions using the Ocean Token
- Establishing immutable provenance records
- Providing a Universal Resolver for DIDs / DDOs

## Publisher

A participant in the data system who creates Assets and makes them available to others.

## Starfish

Starfish is an open source developer toolkit for building applications and use cases with
decentralised Data Supply Lines


# License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
