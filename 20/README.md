```
shortname: 20/AUTHENTICATION
name: Authentication
type: Standard
status: Raw
editor: Tom Marble <tmarble@info9.net>
contributors: Mike Anderson <mike.anderson@dex.sg>, Paul Galwas
```

**Table of Contents**

<!--ts-->

   * [Authentication](#authentication)
      * [Change Process](#change-process)
      * [Language](#language)
      * [Motivation](#motivation)
      * [Specification](#specification)
      * [Asset Provider Context Example Solution](#asset-provider-context-example-solution)
         * [HTTP Basic Authentication](#http-basic-authentication)
         * [OAuth2 Token Authentication](#oauth2-token-authentication)
      * [Changes Required](#changes-required)
      * [References](#references)

<!--te-->

# Authentication

This DEP defines how to handle authentication in the Data Ecosystem Protocol


## Change Process

This document is governed by the [2/COSS](../2/README.md) (COSS).


## Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) \[[RFC2119](https://tools.ietf.org/html/rfc2119)\] \[[RFC8174](https://tools.ietf.org/html/rfc8174)\] when, and only when, they appear in all capitals, as shown here.


## Motivation

The main motivations of this DEP are:

* Define a standard set of authentication guidelines for the Data Ecosystem Protocol
* Provide a basis for a DEP on Authorization

## Specification

Authentication in the Data Ecosystem Protocol can be divided into three
importantly distinct contexts:

1. The Marketplace context
2. The Asset Provider context
3. The Ocean Protocol Network context

### The Marketplace context

First we expect that there will be many marketplaces that will participate
in Ocean Protocol. A distinguishing feature of a Marketplace is
that it may have responsibility for maintaining the legal
identity of participants and thus must control (or delegate)
authentication and authorization at least for activities directly
associated with the marketplace. This legal obligation may come from
restrictions of the data itself (which may, for example, have
personally identifiable information) and/or commercial terms
(non-gratis pricing, field of use restrictions, etc.).

A legal binding between Marketplace principals and third parties is
part of the value add of the Marketplace and not something which can
be provided by the Ocean Protocol Network directly.  Note this binding
is distinct from any additional legal agreements between members of a
marketplace.

### The Asset Provider context

Second an Asset Provider must also control (or delegate) identification,
authentication and authorization or, for example, be vulnerable to
fraudulent identity assertions (e.g. man in the middle attacks) for
the assets they must control. Here it is especially important to
make the requisite authentication "pluggable" into APIs and reference
marketplace services such as to not impose any specific authentication
technology on Asset Providers. Mandating one specific authentication
mechanism for Asset Providers would be a significant impediment to
adoption of the Data Ecosystem Protocol and significantly limit
the commercial applicability.

### The Ocean Protocol Network context

Third the network which implements Ocean Protocol provides a basis for
community consensus sets (e.g. Ethereum smart contracts).
Marketplaces cannot control this consensus nor prevent bad actors
participating in the Ocean Protocol network or its operation or
governance. Marketplaces, however, can discourage bad behavior by
imposing and/or enforcing rules for membership and participation.
Thus authorization and authentication in Ocean Protocol Network must be seen
as distinct from that by Marketplaces and Asset Providers.

### Decentralized Identifiers

In implementing authentication it is recommended to make use of
Decentralized Identifiers (DID's)
[w3c DID spec](https://w3c-ccg.github.io/did-spec/)
[7/DID](https://github.com/oceanprotocol/OEPs/tree/master/7)
and DID Documents (DDO's)
[DDO](https://blog.oceanprotocol.com/ocean-protocol-squid-library-java-c92a6035ce39)
[8/ASSET-DDO](https://github.com/oceanprotocol/OEPs/tree/master/8).

DDO's are a direct and/or indirect representation of asset’s
information, including potentially some metadata. If a DID is the
index key in a key-value pair, then the DDO is the value to which the
index key points. The combination of a DID, a resolver and its
associated DDO forms the root record for a decentralized identifier.

### DID Resolution

As the DDO unambiguously defines (explicitly or by reference) an asset
the resolution process from DID to DDO can be at least as sensitive as
the process of Domain Name Resolution in resolving host names to data
records (including IP addresses).

It is important to note that there are two interpretations of DID resolution:
one in an "on chain" context and the other in an "off chain" context.

### Marketplace Authentication

In the case of marketplace service actions the marketplace must
provide the necessary identification, authentication and authorization
(including issuing credentials). Using DID/DDO for this is
recommended (e.g a template DDO).

For example a marketplace may refer to a given Asset Provider's identity
based HTTP Basic Authentication using a [DID Scheme](https://w3c-ccg.github.io/did-spec/#the-generic-did-scheme) such as:

    did:dep:commercial.assets.io/username#janedoe

If an asset provider supports OAuth2 style tokens then a DID reference to
such a token could be:

    did:dep:commercial.assets.io/token#123456789abcdefghi

## Asset Provider Context Example Solution

Managing identity and handling authentication are challenging topics,
generally, and especially so on a platform designed for decentralization.

The methods of authentication are certain to evolve (most likely as
supersets) yet will begin with mechanisms familiar to developers today.
The Asset Provider example solution for the reference implementation
is based upon HTTP Basic and Token authentication.

The endpoint for the Authentication API is defined in [19/ENDPOINTS](../19/README.md) as **Ocean.Authentication.v1**

### Transport Layer Security

It is REQUIRED that production implementations of services including the
this DEP Authentication API are provided over Transport Layer Security (TLS).
This ensure that both any credentials provided via query parameters
(the URI) or HTTP headers are not transmitted as cleartext.

### HTTP Basic Authentication

An agent implementing a DEP API may have specified users (including usernames
and passwords) as part of an initial configuration.

These credentials may be used with HTTP Basic Authentication.

### OAuth2 Token Authentication

Each DEP 20 implementing service should have a mechanism (API) for generating
access tokens.

These tokens act as a means to associate an API request with
a specific user and then evaluate authorizations for that that user.

In the future such tokens may narrowly define specific access
grants (to be covered in a DEP on Authorization).

## Changes Required

The list of changes to apply in the proposed solution are:

* Modify Agent API reference implementations to offer authentication methods
  as defined in this DEP.

## References

* https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication
* https://oauth.net/2/
* https://developer.github.com/v3/#authentication
