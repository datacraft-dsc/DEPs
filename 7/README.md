```
shortname: 7/STORAGE
name: Asset Metadata definition
type: Standard
status: Raw
editor: Mike Anderson <mike.anderson@dex.sg>
contributors: 
```

Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Asset Metadata](#asset-metadata)
      * [Change Process](#change-process)
      * [Language](#language)
      * [Motivation](#motivation)
      * [References](#references)



# Universal Storage 

Ocean has a core mission of making data assets visible and discoverable, with a decentralised protocol for data exchange.

To ensure that clients are able to make use of assets from service providers throughout the ecosystem, it is necessary
to provide a universal storage API for accessing asset content that is independent of the underlying storage implementation,
i.e. we wish to avoid tight coupling between the implementation of asset consumers and asset providers.


## Change Process

This document is governed by the [2/COSS](../2/README.md) (COSS).


## Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) \[[RFC2119](https://tools.ietf.org/html/rfc2119)\] \[[RFC8174](https://tools.ietf.org/html/rfc8174)\] when, and only when, they appear in all capitals, as shown here.


## Motivation

In the absence of a defined standard for storage, asset consumers would need to separately determine and negotiate
methods for accessing asset content. This would undermine the vision of Ocean to provide a universal interoperable
substrate for data and AI services - it would simply become a network of custom point-to-point integrations with
tight coupling between producers and consumers.

The main motivations of this DEP are:

* Establish a standard asset content storage API to allow interoperability between participants in the Ocean ecosystem
* Ensure the storage API is easy to use and consistent with existing internet tools and standards
* Ensure the standard enables support for key features of the OCean Protocol


## References

TBC:

* A - https://foo.org

