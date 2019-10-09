```
shortname: 0/GLOSSARY
name: Data Ecosystem Glossary
type: Reference
status: Draft
editor: Mike Anderson <mike.anderson@dex.sg>
contributors: 
```

# Glossary

This Glossary provides definitions of common terms used in DEPs.

## Agent

A network-connected service operated by a participant of the data ecosystem that is able
to offer one or more DEP Standard APIs to other participants in the ecosystem.

Typically, an Agent will be server-side software hosted in a virtual environment managed
by the Agent's operator. Open source reference implementations of Agents are available
for operators to configure and deploy if they wish, or alternatively operators may wish
to develop their own Agents - the only requirement is conformance to the DEP Standard APIs.

## Algorithm

An algorithm is a sequence of instructions that can be execute to perform a computation.

Service providers in the data ecosystem may choose to offer an algorithm for use by the
broader ecosystem by adding appropriate metadata and bundling this as an Operation, which 
can be invoked by consumers using the Invoke API (DEP6).

The DEPs place no restrictions on what language or underlying execution environment is 
used to represent an algorithm, making it possible to plug-and-play with arbitrary 
compute technologies in the data ecosystem.


## Asset

An entity in the Data Ecosystem which has potential value, and may be utilised by participants
using systems supporting the DEPs standards.

## Asset Metadata

A JSON data object that describes an Asset in the Data Ecosystem. Every valid Asset in the 
Ecosystem must have valid Metadata associated with it.

Asset Metadata standards are defined in DEP8.

## Bundle

A special type of Asset that represents a collection of other Assets. A Bundle can be regarded
as analogous to a folder or directory in traditional computer file systems.

## Consumer

A participant in the data system who acquires and utilises Assets in the Data Ecosystem.

## Data Asset

An Asset that represents a single data object that can be utilised in the data ecosystem.

A Data Asset may have any content format, at the discretion of its Publisher. However it is recommended
that Publishers make use of existing standard data formats (e.g. CSV files) to maximise utility to 
Consumers.

## Data Ecosystem

An interconnected network of participants who are able to interoperate using standard protocols
such as those defined in the DEPs.

## Data Supply Line

An orchestrated set of data flows that is designed to create value in a Data Ecosystem.

A Data Supply Line may be executed by one or more Agents, can transfer Assets between different
participants, and can transform Assets through the use of Operations to create new, more valuable
Assets.

## DEP

Acronym for "Data Ecosystem Proposal", a document defining open standards for the data economy.

DEPs are technology neutral, and free for anyone to implement and use. Systems making use of
DEP standards will gain the benefits of interoperability with other systems in the Data 
Ecosystem, including the capability to participate in decentralised Data Supply Lines.

## DEP Standard APIs

An API provided by an Agent which conforms to the API Standards specified in the DEPs.

## DID

A *Decentralised Identifier* conforming to the W3C DID spec.

DIDs are used to refer to Agents and Assets in the Decentralised Data Ecosystem. DIDs may also
be assigned to individuals, enabling self-sovereign identity management

See: https://w3c-ccg.github.io/did-spec/

## Listing

An entity representing an Asset that is available for trade in the data economy.

Listings are able to represent data about the commercial terms on which an Asset may be acquired or accessed, including:
- Pricing
- Commercial terms and conditions
- Restrictions to specific geographies

A marketplace is responsible for maintaining a set of active listings for any assets that are intended for purchase
using DEP standardised purchase functionality. In general, the set of active Listings is assumed to be mutable, 
e.g. pricing may change over time. Furthermore, it
is possible for a single Asset to be listed multiple times (e.g. in different marketplaces, or under different terms).

## Marketplace

A Marketplace is a service or software product that enables buyers and sellers of data to discover Assets, validate and 
verify trust attributes, and perform economic transactions such as buying access to these Assets.

Marketplace operators may choose who is allowed to make use of the marketplace functionality: it could be open to all in
the ecosystem, or restricted to a trusted set of participants.

## Ocean Network

The Ocean Protocol Foundation provides the Ocean Network as a public service utility to the 
data economy. The Network provides important capabilities for decentralised data exchange,
including:
- Value exchange / transactions using the Ocean Token
- Establishing immutable provenance records
- Providing a Universal Resolver for DIDs / DDOs

## OEP

Acronym for "Ocean Enhancement Proposal", which defines enhancements to the Ocean Network and
related technologies. Current OEPs can be found at: https://github.com/oceanprotocol/OEPs.

## Operation

A special type of Asset representing a computational service that can be "Invoked" via the DEP
Standard APIs (see DEP6 for detailed specification).

An Operation may accept one or more inputs, and produce one or more outputs. Typically, inputs
will be references to source data assets and the outputs will be new assets produced by algorithms
executed upon the input assets.

Examples of Operations may include:
- Data cleaning
- Anonymisation / removal of Personally Identifiable Information
- Training a machine learning model
- Aggregating multiple source data sets

## Provenance

Provenance is a record of ownership or origin of an Asset, which may be valuable in enhancing the
trustworthiness of an Asset and therefore its economic value and suitability for use in regulated 
environments or in situations where higher levels of trust are required.

There are multiple forms of Provenance possible according to DEP standards, including:

- Publisher Declared Provenance, which forms part of Asset Metadata according to DEP12
- On-chain Provenance, which refers to immutable record of registration and/or purchase history recorded 
on a decentralised ledger.
- Logs maintained at the discretion of service providers or marketplaces

## Publisher

A participant in a Data Ecosystem who creates Assets and makes them available to others. A Publisher
is typically the Data Owner of the asset, or someone acting on the Data Owner's behalf.

## Resolver

A service available to the Data Ecosystem that maps DIDs to DDOs, effectively acting as a shared directory
of decentralised services in the data ecosystem.

The Ocean Network provides a resolver which handles DIDs registered on the Ocean network, which have the form:

`did:op:21f8a3a788fe465f81b146af04ed4c9bd7d9158bebbc458a8ca92d5dd57335d3`

## Service Provider

A Service Provider is a participant in the data ecosystem who makes Assets available to other participants
via one or more Agents supporting the DEP Standard APIs. Service providers may provider additional APIs
not covered by the DEP Standards, but should be aware that this will limit the ability for such functionality
to be used in an interoperable fashion, since such features will not be available to be used in general 
purpose data supply lines.

Service providers may choose to make their assets free to all, available for a free, or restricted to 
a controlled set of participants according to access control rules. This is at the discretion of the service
provider.

## Starfish

Starfish is an open source developer toolkit for building applications and use cases with
decentralised Data Supply Lines, available for multiple programming languages.

The core set of abstract operations in Starfish are defined in DEP4, however the specific library functions
and patterns may vary based on the implementation language. 


# License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
