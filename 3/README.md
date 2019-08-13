```
shortname: 3/ARCHITECTURE
name: Architecture & Principles
type: Standard
status: Draft
editor: Mike Anderson <mike.anderson@dex.sg>
contributors: 
```

Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Architecture](#architecture)
      * [Vision](#vision)
      * [Principles](#principles)
      * [Architecture Layers](#architecture-layers)
      * [Decisions](#decisions)
      * [References](#references)
      * [License](#license)

# Architecture

This DEP describes the conceptual reference architecture for Decentralised Data Ecosystems supported 
by the broader set of DEP standards.

## Vision

We envision a world where:

- Data and AI services can be harnessed by all for the good of humanity, and
not controlled by a handful of powerful gatekeepers. 
- The costs of developing AI solutions are minimised, so that the value of the "long tail" of Data and AI
assets can be realised.
- Interoperability is built into the ecosystem from the ground up, so that solutions can be built to
span across a broad range of participants without inefficient / expensive bespoke integration work.

It is the goal of this DEP to outline a reference architecture and architectural principles that can support
this vision. 

## Principles

The DEPs specify standards for the development of interoperable data ecosystems. The design of 
such standards and systems that implement them will inevitably require good judgement around 
specific decisions and technical trade-offs, and we will make such decisions with a preference
for the following principles:

- Open source and open standards
- No centralised point of control
- Decentralised ownership of assets and services
- Support for orchestration of decentralised Data Supply Lines
- Decentralisation of trust rules
- Abstraction from specific technology choices
- Support for arbitrary data formats
- Available over the public internet
- Peer-to-peer model

DEX is a founding company of the Ocean Protocol Foundation. While the DEPs represent 
open standards and do not mandate the use of the Ocean Network or any specific software 
produced by DEX (e.g. Starfish), a key goal of DEX development efforts is provision 
of convenient tools to build use cases and a vibrant data ecosystem on Ocean Protocol, 
and reference implementations are designed to support this.

## Architecture Layers

The components of a decentralised data ecosystem architecture can be conceptualised in four layers.

![Architecture Layers diagram](images/layers-1.png "Architecture Layers")

### Application Layer

This layer is responsible for interaction with the end user and delivering value-creating functionality
to the ecosystem.

In general, applications in this layer will be custom applications or products designed to support a particular use
case or set of users. Examples might include:

 - A user-friendly data marketplace application allowing the buying and selling of data assets
 - A consumer website making use of data assets and services to improve customer experience
 - A data science project performing machine learning on customer data sets
 - An enterprise system making use of data assets for real-time decision making
 
### Orchestration Layer

This layer is responsible for orchestrating the interactions between decentralised data assets and services in
order to meet the requirements of the application layer. 

> "Being abstract is something profoundly different from being vague... The purpose of abstraction is not to be vague, but to create a new semantic level in which one can be absolutely precise."
>
> (Edsger Dijkstra)

A central concept of the orchestration layer is to provide a toolkit required to assemble a flow of 
arbitrary data assets across arbitrary  participants, possibly including arbitrary computations, and thus forming a 
Data Supply Line. This is made possible by abstraction away from specific technologies to a common set of abstract
concepts (Assets, Operations, Agents etc.) that are defined in the DEP standards to have common interfaces.

Data Supply lines are typically orchestrated according to the custom requirements of the use case they are serving.
For example, the diagram below shows the application of predictive modelling to anonymised patient data sets from
multiple healthcare providers, with secure aggregation of data handled by a trusted broker. 

![Data Supply Line example diagram](images/data-supply-line.png "Data Supply Line example")

A significant set of open source software code providing the functionality for this layer is provided in
the form of the Starfish developer toolkit (described in [DEP4](../4/README.md), which includes a set of libraries suitable for orchestrating
data assets and services according to the DEP standards.

A typical pattern for implementing the orchestration layer is therefore for the user application to make use
of the Starfish libraries to provide orchestration capabilities. This is the recommended approach, however 
implementors may choose a different model (e.g. adopting an enterprise middleware solution to perform this 
orchestration function).

### Integration Layer

This layer is responsible for providing access to data assets and services in the ecosystem under the control
of a service provider. 

In order to enable ecosystem interoperbility, the Integration layer must expose these assets and services to 
the ecosystem according to a set of abstract DEP standard APIs. Key examples of such APIs include:
 - Ability to remotely invoke compute operations (see [DEP6](../6/README.md))
 - Access to raw asset data (see [DEP7](../7/README.md))
 - Access to asset metadata and provenance (see [DEP15](../15/README.md))

Typically the layer is implemented as a set of one or more Agents which are hosted in a secure environment 
controlled by the service provider. These agents would serve their capabilites via APIs either to the whole
ecosystem via the public internet or to a closed set of participants via a VPN / private network.

The integration layer is responsible for enforcing any trust rules and access controls required by the 
service provider, and may require access to the underlying storage and compute infrastructure that is being 
managed by a service provider. For this reasons, it is extremely important that appropriate measures
are taken to ensure the security of this layer, particularly if sensitive assets are at risk.

### Capability Layer

This layer provides the underlying data assets and services.

Because a layer of abstraction is provided via the integration layer, there is no restriction on the type of
underlying technology used. This is a significant advantage of this layered design, because it allows
any existing storage / compute infrastructure to be plugged in to the global decentralised data ecosystem
without the need for expensive replacements or upgrades (typically, a lightweight adapter to play the role
of the integration layer is all that is required).

Examples of capabilities that may be valuable to integrate into the data ecosystem include:
 - Big Data storage and compute technologies (e.g. Spark, Hadoop )
 - Traditional storage (e.g. file systems, SQL databases)
 - Secure Compute Hardware (e.g. Intel SGX)
 - Proprietary machine learning algorithms
 - Data cleaning / ETL processes
 - Decentralised networks (e.g. IPFS, Filecoin)

Note that these capabilities may be under the control of a single service provider (who may also be the data owner)
but could also be fully decentralised capabilities.


## Decisions

This section will be extended to list formal architectural decisions related to the reference architecture.

Note: Architecture decisions are currently under internal DEX review. Suggestions for new decisions
and refinements to existing decisions from contributors are welcome.

### General Decisions

<table style="width: 100%">
    <colgroup>
       <col span="1" style="width: 20%;">
       <col span="1" style="width: 40%;">
       <col span="1" style="width: 40%;">
    </colgroup>

  <tbody valign="top">
    <tr>
      <th>Topic</th>
      <th>Decision</th>
      <th>Rationale</th>
    </tr>
    <tr valign="top">
      <td>Ecosystem Architecture Model</td>
      <td>The Data Ecosystem will be regarded as consisting of four conceptual layers
		<ul>
		  <li>Application (dApps, CLI users, user environments etc.)</li>
          <li>Orchestration (Client side libraries enabling DEP capabilities, e.g. Starfish)</li>
          <li>Integration (Ecosystem agents offering standard APIs)</li>
          <li>Capabilities (Any underlying data systems or other technology integrated into the ecosystem)</li>
		</ul>
      </td>
      <td>
        <ul>
		  <li>Enable interfaces to be clearly defined between layers, especially between systems managed by different actors on a decentralised basis</li>
          <li>Support common architectural patterns and best practices</li>
          <li>Enable efficient integration of existing data ecosystems and technology</li>
          <li>Provide a flexible evolutionary path, allowing for further decentralisation of capabilities</li>
		</ul>
      </td>
    </tr>
    <tr style="vertical-align:top">
      <td>Ocean Integration</td>
      <td>The DEP Ecosystem shall support the Ocean Protocol / Ocean Network as a optional blockchain capability</td>
      <td>
        <ul>
		  <li>Enable economic value exchange via the Ocean Token and smart contracts</li>
          <li>Support immutable on-chain records of provenance</li>
          <li>Provide an instance of a Universal Resolver to resolve DDOs according to the W3C DID specification</li>
          <li>Provide a shared, public utility network as a service to the ecosystem</li>
		</ul>      
      </td>
    </tr>
    <tr>
      <td>Implementation independence</td>
      <td>All components and interfaces specified via DEPs may be freely implemented using different technology. All such implementations correctly implementing these open standards should be able to interoperate.</td>
      <td>
        <ul>
		  <li>Avoid lock in to specific technology choices or vendors</li>
		  <li>Allow evolution of ecosystem to tap into new technology innovations</li>
		</ul>      
      </td>
    </tr>
    <tr>
      <td>Open Source Reference Implementations</td>
      <td>All official reference implementations referred to in DEPs must be available under an Open Source License according to the [Open Source Definition](https://opensource.org/osd). Non open-source implementations are welcome in the ecosystem, but will not be regarded as reference implementations.</td>
      <td>
        <ul>
		  <li>Encourage benefits of community ownership of open source code</li>
		  <li>Open Source Definition is industry accepted standard</li>
		</ul>      
      </td>
    </tr>
    <tr>
      <td>Dependencies on underlying protocols</td>
      <td>DEP Standards may refer to underlying protocols (e.g. the Ocean Protocol or ODBC for database access) but must not
          absolutely require these as a hard dependency</td>
      <td>
        <ul>
		  <li>Choice of underlying protocols / substrates should be left as implementation choices for the ecosystem</li>
		  <li>Enables evolution of the DEP Standards independent of underlying technology choices</li>
		  <li>Prevent "lock in" to specific technology choices mandated by the DEP standards</li>
		</ul>    
	  </td>
    </tr>
  </tbody>
</table>

### Asset Model

<table>
    <colgroup>
       <col span="1" style="width: 20%;">
       <col span="1" style="width: 40%;">
       <col span="1" style="width: 40%;">
    </colgroup>

  <tbody valign="top">
    <tr>
      <th>Topic</th>
      <th>Decision</th>
      <th>Rationale</th>
    </tr>
    <tr>
      <td>Asset Definition</td>
      <td>An Asset shall be defined as an entity consisting of metadata and content, where:
        <ul>
          <li>Metadata is a file according to the format defined by DEP8</li>
          <li>Content may be any data, or other resource that represents the asset</li>
        </ul>
      </td>
      <td>
        <ul>
		  <li>Standardised metadata is necessary to allow interoperable services to work with arbitrary assets</li>
		  <li>A broad definition of content is required, because we intend to allow any form of
		  data, algorithm or other resource relevant to the data ecosystem to be represented as an asset</li>
		</ul>      
      </td>
    </tr>
    <tr>
      <td>Asset Identity</td>
      <td>An asset shall be identified by the cryptographic hash of its metadata using the SHA3-256 hashing algorithm. This value represented in lowercase hex shall be knows as the Asset ID</td>
      <td>
        <ul>
          <li>Enforces immutability of asset metadata</li>
		  <li>Enables cryptographic verification of asset metadata using the Asset ID</li>
          <li>Enables the recursive use of cryptographics hashes in asset metadata to establish a merkle tree, where appropriate</li>
          <li>SHA3-256 is a recognised standard representing current best practice suitable for the applications envisaged in the data ecosystem</li>
		</ul>      
      </td>
    </tr>
    <tr>
      <td>Data Assets</td>
      <td>A Data Asset shall be a subtype of Asset defined by having immutable content analogous to the content of a file. Any content type may be supported (e.g. MIME types as specified in RFC2045 etc.) </td>
      <td>
        <ul>
          <li>We need a standard, flexible way to provide a general purpose data asset than can be shared in the ecosystem in an interoperable manner</li>
          <li>Enable adoption of existing, established Internet standards (MIME) for content types</li>
          <li>Gain desirable property of data immutability for reasons of trusted provenance, caching, integrity verfification, performance etc.</li>
		</ul>      
      </td>
    </tr>
      <tr>
      <td>Operations</td>
      <td>An Operation shall be a subtype of Asset representing a computational operation, which may take multiple inputs and multiple outputs</td>
      <td>
        <ul>
          <li>Data assets alone are insufficient to deliver meaningful decentralised data use cases</li>
          <li>A standardised representation of a computational operation is necessary to enable arbitrary services to participate in Data Supply Lines orchestrated according to DEP standards</li>
		</ul>      
      </td>
    </tr>
  </tbody>
</table>

Note: initial reference implementations used the Keccak256 hash function for asset identity, for consistency with Ethereum. A decision has been made to move to SHA3-256 as the established standard - reference implementations are currently being updated.

### Agent Model



## References

* W3C Decentralized Identifiers (DIDs) Specification - https://w3c-ccg.github.io/did-spec/
* CSRC. (2019). "Hash Functions." Computer Security Resource Center, National Information Technology Laboratory. Updated 3 May 2019 - https://csrc.nist.gov/projects/hash-functions
* MIME Standard RFC2045 - https://tools.ietf.org/html/rfc2045

## License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
