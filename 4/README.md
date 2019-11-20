```
shortname: 4/STARFISH
name: Starfish Abstract API
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

Starfish is an open source developer toolkit for building decentralised data ecosystems and use cases.

Starfish provides the tools and capabilities required for an application developer to create,
orchestrate, execute and manage decentralised data supply lines spanning multiple parties in the 
data ecosystem. The ability for such parties to interoperate is made possible by conformance to
DEP Standard APIs.

This DEP specifies Starfish as a set of abstract capabilities and APIs supported by the toolkit: It does 
not specify the precise developer-level API, since these decisions will depend upon the idioms, 
conventions and syntax of specific language implementations. 

## Motivation

Building decentralised data ecosystems is a monumental task, and interoperability will depend
on the adoption of common standards.

We draw an analogy to the growth in world trade enabled by container shipping: the explosive growth
rates of world trade since 1970 were strongly enabled by the adoption of ISO Standards for 
shipping containers. Such standards enable the construction and operation of much larger container ships,
dockside infrastructure such as container cranes and container terminals, all of which are able to
interoperate in global supply chains thanks to the common standards.

In the same way, Starfish provides a common abstraction to enable decentralised data infrastructure to 
interoperate effectively.

- Any existing data resource can be "packaged" into a Data Asset usable with Starfish 
- Any existing compute resource or algorithm can be expressed as a Starfish Operation, and made
available for remote invocation across the ecosystem (see DEP6)
- "Data Supply Lines" can be constructed representing a flow of data assets and execution of
algorithms / compute operations across many participants

There is no practical limit to the types of operations that can be created, and potentially 
recombined in interesting ways to create novel data solutions. Orchestration of such 
operations with Starfish is a perfect way to facilitate rapid innovation in data and AI 
solutions, especially where these solutions must orchestrate data and 
services across multiple parties in a decentralised or federated model.



## Specification

### Assets

Assets are represented in Starfish as an abstraction enabling users to work with such Assets wherever they
may be located in the Data Ecosystem.

#### Asset Creation

Starfish must provide the ability to create data assets, where the creator of the asset may specify 
both metadata and data content.

Starfish should provide automatic creation of default metadata, for developer convenience, subject to the 
expectation that the user may override such defaults.

#### Asset registration

Starfish must allow the ability to register an asset with a remote agent, where registration includes 
the upload of asset metadata to the remote agent.

Starfish should enable the developer to validate the registered Asset ID (since it is the cryptographic
of the asset metadata). This capability should also be utilised by service providers to validate
assets upon registration.

#### Asset upload

Starfish must allow the ability to upload asset data content for a registered asset to a remote agent.

Starfish may offer a combined "register plus upload" function as a developer convenience, provided that 
these are also available as separate functions.

#### Asset Content Access

Starfish must allow the user to request the content for a given asset.

Starfish must accomodate the possibility that access to asset content may be impossible, and report 
the failure reason accordingly.

Starfish must allow the content, if available, to be used to create another asset.

Starfish should allow the user to validate the content according to its cryptographic hash.

Starfish should make the content available in forms convenient for the developer to 
utilise in their given language or environment (e.g. byte arrays, streams, writing to filesystem etc.).

Starfish may provide additional utilities to handle specific common content types (e.g. CSV files),
where this is particularly appropriate for the given implementation. It is recommended that 
implementations are conservative about incorporating such functionality, as it is infeasible to 
support all possible data types and the interpretation of content types should be considered 
outside the scope of Starfish in general.

### Agents

Agents are represented in Starfish as an abstraction enabling users to work with Agents wherever
they are operated in the Data Ecosystem. 

#### Remote Agents

Note: In this context we consider Remote Agents to be network-accessible servers hosting one or 
more of the DEP Standard APIs. 

Starfish must allow Remote Agents to be resolved from a DID via the Universal Resolver capability.
Resolving a Remote Agent should return a value that is conceptually equivalent to a "handle" for the
remote agent.

Starfish must allow a DID to be obtained for a Remote Agent, which can be passed to other parties as
a stable identified for the Remote Agent.

Starfish API functions performed using a Remote Agent (e.g. requesting asset content) must 
be relayed to the actual Remote Agent via the appropriate DEP Standard APIs.

#### Capability Agents

Starfish may offer specialised Capability Agent implementations that allow different underlying technology capabilities 
to be accessed via the standard Agent APIs.

Capability Agents are the recommended approach for a developer to integrate services or systems into 
a Data Supply Line in situations where a remote or decentralised service does not support the DEP
Standard APIs. If the DEP Standard APIs are available, a standard Remote Agent should be used.

Starfish should make it simple, as far as possible, for developers to create their own Capability 
Agents for custom purposes, e.g. by extending a common base implementation.

Note: providing a Capability Agent of this nature is a technique used in reference implementations 
to integrate the Ocean Network, by wrapping the Squid library provided by the Ocean Protocol Foundation.
This technique is valuable because it allows a consistent Agent API to be maintained for user 
while allowing for evolution and innovation in the underlying technology capabilities.

Note: It is beyond the scope of Starfish to allow Capability Agents to be exposed to the ecosystem
(to the extent that they could be invoked as Remote Agents by other parties), however
service provider implementations may wish to offer this functionality.

#### Local Agents

Starfish may implement Local Agents, which offer functionality and interfaces equivalent to
any other Agent but intended for use only in the local environment.

Local Agents may be used where Agent functionality is desired but sharing across the ecosystem is
unnecessary. Example of such usage may include:
- Testing and simulation of Data Supply Lines in a local environment
- Development of agent functionality prior to packaging for remote usage

Local agents must conform to the same interfaces as remote agents, with certain limitations
e.g. it will typically not be possible to pass meaningful references to Local Agents to remote parties 
in the ecosystem.

Note: It is beyond the scope of Starfish to allow Local Agents to be exposed to the ecosystem
(to the extent that they could be invoked as Remote Agents by other parties), however
service provider implementations may wish to offer this functionality.

### Operations

Note: In this context we consider Operations to be a subtype of asset which represents a computation
according to the specification in DEP6.

#### Resolving Operations

Starfish must allow an Operation provided by a Remote Agent to be resolved into an operation
that may be used in the developer's context. This resolved Operation can be considered as a "handle"
to the Remote Operation.

#### Invocation

Starfish must allow the developer to "invoke" an Operation, in the sense defined in [DEP6](https://github.com/DEX-Company/DEPs/tree/master/6).

Starfish must allow input parameters to be passed to the invoked Operation, where such input
parameters may be JSON values or Assets as defined in DEP6. Parameters should be provided in 
a manner that is idiomatic for the given language implementation (e.g. passing a named map).

Starfish must allow invocation to be made either synchronously or asynchronously.

Starfish must allow the user to check the status of asynchronous invocation jobs.

Starfish must allow the user to examine the results of a successfully completed operation (which
may include either directly returned JSON values or Assets provided as results).

Starfish must handle the case of invocation failure, and report errors back to the user including
the reason for failure (including, but not limited to: authorisation failure, network failure, 
invalid parameters, server failure, timeout etc.).

#### Local Operations

Starfish must allow the user to create an Operation in their local context.

Starfish must allow Local Operations to be used in the same way as Remote Operations in the context
of the local environment.

Starfish should allow the user to provide arbitrary executable code for their local operation (bearing in mind
that it is the responsibility of the user to ensure security properties of this executable code)

Note: It is beyond the scope of Starfish to allow Local Operations to be exposed to the ecosystem
(to the extent that they could be invoked as Remote Operations by other parties), however
service provider agent implementations may wish to offer this functionality using the Invoke API.

### Marketplace

TBC: Starfish standards for marketplace operations are are currently under
development as part of the reference implementations.

Overall goals for marketplace capabilities include:

- The ability for asset publishers to create / manage listings of assets for sale on a data marketplace
- The ability for asset consumers to execute transactions using the Ocean Network


#### Create listing

Allows a publisher to create a listing on a Marketplace for an asset that they control.

#### Update listing

Allows a publisher to update a listing that they control on a Marketplace.

Possible changes include:
 - Changing the status of the listing (e.g. disabling it)
 - Changing the price
 - Changing terms and conditions of sale

#### Get Listing

Retrieves a listing from a marketplace, allowing the Consumer to inspect the terms and conditions of the 
listing before purchase.

#### Purchase

Provides the capability for a Consumer to purchase an Asset via a Listing, making a payment using an
appropriate mechanism (e.g. Sending Ocean Tokens to the publisher)

#### Purchase confirmation

Provide the ability for the Consumer to check that a purchase is complete, and for a service provider to
verify that payment has been made before granting access to the purchased Asset.

### Resolver

The Resolver in starfish provides utilities to access Agents and Assets via DIDs / DDOs.
There are two supported implementations in starfish: first one for compatibility with Ocean Squid and second one from DEX. First one requires Aquarius service as storage of DDO while second one uses Ethereum network itself as storage. In both cases DID is registered on-chain. 

#### Resolve Agent via DID

Gets an existing Agent via the Universal Resolver.

#### Create and Register Agent DDO 

Creates an Agent with a registered DDO on the Resolver

#### Update Agent DDO 

DDO can be updated but only by owner. Owner is the one who signed the registration transaction

#### Install Local DDO

Starfish provides the capability for the user to install a locally specified DDO.

This is useful in the following scenarios:
- Local / test usage
- Accessing an Agent that is not listed on a public resolver service

### General functionality

#### Authorisation

Starfish must handle the case that any remote operation may fail because of authorisation failure. 
It should be considered a user error to request an operation for which the user does not have 
authorisation and reported as such.

#### Error handling

Starfish must report errors whenever a requested operation cannot be successfully performed (i.e. it 
should never fail silently).

Starfish must report errors back to the user using the conventional mechanism for error reporting
used by the implementation language (e.g. raising an Exception in Java).

Starfish should provide meaningful human-readable error messages back to the caller, in whatever form
is idiomatic for the implementation language.

## Rationale

Starfish is specified as a set of abstract capabilities because it is intended to be implemented in
multiple programming language and paradigms. As such, different implementations
will need to take different according to their respective language ecosystems in areas such as:

- Error handling mechanisms
- Naming conventions
- Use of language syntax and idioms
- Documentation

While it would be possible to build data ecosystems purely on the basis of the API Standards defined
in DEPs, this would place considerable implementation burden on the application developer, who would
have to recreate much of the functionality already offered by Starfish. In this sense, the main benefit
of Starfish is to provide a shared open source toolkit so that application developers only need to
focus on the high level orchestration required by their use case, and not worry about low-level 
plumbing.

## Implementations

| Language | Repository | Status |
|-|-|-|
| Python | https://github.com/DEX-Company/starfish-py | Alpha |
| Java | https://github.com/DEX-Company/starfish-java | Alpha |
| Clojure | https://github.com/DEX-Company/starfish-clj | Alpha |
| JavaScript  | https://github.com/DEX-Company/starfish-js | Early Dev |

## License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
