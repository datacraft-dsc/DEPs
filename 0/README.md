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

## Asset Instance

A copy of an Asset held by an actor in the Data Ecosystem. Asset Instances may be created by copying
asset data and metadata between agents (assuming appropriate access rights).

## Asset Metadata

A JSON data object that describes an Asset in the Data Ecosystem. Every valid Asset in the 
Ecosystem must have valid Metadata associated with it.

Asset Metadata standards are defined in DEP8.

## Bundle

A special type of Asset that represents an aggregation of other Assets. 

A Bundle can be regarded as analogous to a folder or directory in traditional computer file systems, except that
a Bundle is immutable: the contents cannot be changed once it has been created. This is important for ensuring
that data usage can be audited and integrity checked in the future.

## Collection

A collection is a set of assets managed by an agent on behalf of a set of users. A user may use a Collection to organise a set of assets for a workflow.

## Consumer

A participant in the data system who acquires and/or utilises Assets in the Data Ecosystem.

## Content

The data contained within an Asset, as distinct from the Metadata of the Asset.

Actors may include arbitrary content within an Asset that they create. To help specify the type / format of data
included it is possible to tag the Content with an Optional MIME Type as per RFC2045 and related internet standards.

Operations may be considered to have "Content" which is equivalent to the executable code of the Operation. This
may or may not be possible to share between different execution environments.

## Convex Network

The Convex Network is a public service utility which can be used for digital value exchange withing the data economy.

Use of the DEP standards does not require using Convex, however Datacraft provides several useful facilities on the Convex network which may be of value to the data economy, including:

- Value exchange / transactions using Tokens
- Establishing immutable provenance records
- Providing a Universal Resolver for DIDs / DDOs

## Data Asset

An Asset that represents a single data object that can be utilised in the data ecosystem.

A Data Asset may have any content format, at the discretion of its Publisher. However it is recommended
that Publishers make use of existing standard data formats (e.g. CSV files) to maximise utility to 
Consumers.

## Data Ecosystem

An interconnected network of participants who are able to interoperate using standard protocols
such as those defined in the DEPs.

## Digital Supply Line

A set of data flows that produces data outputs, designed to create value in a Data Ecosystem,
and specified according to the DEP Standards.

A Digital Supply Line may be executed by one or more Agents, can transfer Assets between different
participants, and can transform Assets through the use of Operations to create new, more valuable
Assets.

Digital supply lines can be automated using the Orchestration capabilities in the DEP Standards.
Alternatively, digital supply lines can be manually executed by invoking the individual operations
that make up the digital supply line.

## DEP

Acronym for "Data Ecosystem Proposal", a document defining open standards for the data economy.

DEPs are technology neutral, and free for anyone to implement and use. Systems making use of
DEP standards will gain the benefits of interoperability with other systems in the Data 
Ecosystem, including the capability to participate in decentralised Digital Supply Lines.

## DEP Standard APIs

An API provided by an Agent which conforms to the API Standards specified in the DEPs.

## DID

A *Decentralised Identifier* conforming to the W3C DID spec.

DIDs are used to refer to Agents and Assets in the Decentralised Data Ecosystem. DIDs may also
be assigned to individuals, enabling self-sovereign identity management

See: https://w3c-ccg.github.io/did-spec/

## Directed Acyclic Graph (DAG)

A directed acyclic graph is a graph where edges indicate a direction and there are 
no cycles (loops).

DAGs are important for defining Orchestrations, where the direction of graph edges define the flow
 of data between operations and the acyclic property guarantees that the orchestration can be
 executed without infinite loops, and with all operations being executed at most once.

See: https://en.wikipedia.org/wiki/Directed_acyclic_graph 

## Job

A Job allows a user to view an Orchestration. The result status of the Job can be used to inspect the status of execution for each step of the Orchestration (including potential failures in child Operations).
For further details, see [DEP6](https://github.com/DEX-Company/DEPs/tree/master/6).

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

## Orchestration

An Orchestration is a special type of Operation that represents the combination of multiple child 
operations, which are arranged as a directed acyclic graph.

When "Invoked" in the context of an Agent, the agent is responsible for executing the child operations
in an order consistent with the dependencies specified in the directed acyclic graph.

Like any other Operation, an Orchestration may accept one or more inputs, and produce one or more outputs,
with the added consideration that these inputs may become inputs of child operations, and the outputs of
child operations may become outputs of the overall Orchestration. Outputs of child Operations may also 
become inputs of other child Operations, with the proviso that this must be consistent with the execution
ordering specified in the directed acyclic graph.

As such, an Orchestration can be used to represent a fully automated Digital Supply Line that can
be executed and managed by agents in the data ecosystem.

Orchestrations are defined in more depth in DEP13

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

Datacraft provides a DEP-compliant Resolver using the public Convex Network, which handles DIDs of the form:

`did:dep:21f8a3a788fe465f81b146af04ed4c9bd7d9158bebbc458a8ca92d5dd57335d3`

## Service Provider

A Service Provider is a participant in the data ecosystem who makes Assets available to other participants
via one or more Agents supporting the DEP Standard APIs. Service providers may provide additional APIs
not covered by the DEP Standards, but should be aware that this will limit the ability for such functionality
to be used in an interoperable fashion, since such features will not be available to be used in general 
purpose digital supply lines.

Service providers may choose to make their assets free to all, available for a free, or restricted to 
a controlled set of participants according to access control rules. This is at the discretion of the service
provider.

## Starfish

Starfish is an open source developer toolkit for building applications and use cases with
decentralised Digital Supply Lines, available for multiple programming languages.

The core set of abstract operations in Starfish are defined in DEP4, however the specific library functions
and patterns may vary based on the implementation language. 


# License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
