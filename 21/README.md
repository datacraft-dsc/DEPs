```
shortname: 21/AUTHORIZATION
name: Authorization
type: Standard
status: Raw
editor: Tom Marble <tmarble@info9.net>
contributors: Paul Galwas
```

**Table of Contents**

<!--ts-->

   * [Authorization](#authorization)
      * [Change Process](#change-process)
      * [Language](#language)
      * [Motivation](#motivation)
      * [Specification](#specification)
      * [Changes Required](#changes-required)
      * [References](#references)

<!--te-->

# Authorization

This DEP defines how to handle authorization in the Data Ecosystem Protocol

## Change Process

This document is governed by the [2/COSS](../2/README.md) (COSS).


## Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) \[[RFC2119](https://tools.ietf.org/html/rfc2119)\] \[[RFC8174](https://tools.ietf.org/html/rfc8174)\] when, and only when, they appear in all capitals, as shown here.


## Motivation

The main motivation of this DEP is:

* Define a standard set of authorization guidelines for the Data Ecosystem Protocol.

## Specification

As discussed in [20/AUTHENTICATION](https://github.com/DEX-Company/DEPs/tree/master/20) there are three contexts for authentication:

1. The Marketplace context
2. The Asset Provider context
3. The Ocean Protocol Network context

Each of these contexts may have a specific identifier for principals
(recommended use of [DID](https://w3c-ccg.github.io/did-spec/)).

Each principal may be associated with a role and/or group.
The identification of such a role or group could also be via DID.

The actions permitted by a principal (role or group) may be authorized
with respect to specific API endpoints and/or assets.

An authorization may be expressed as a principal (role or group)
and action(s) possibly with assets. This authorization may be
expressed in a document (DDO) and referenced by DID. In this way
the authorization DID can be viewed as a grant token.

Evaluating the affirmative application of a grant token may be
made by evaluating the authorization via direct (or delegated)
authentication across each of the three contexts, as appropriate.

## Changes Required

The list of changes to apply in the proposed solution are:

* _TBD_

## References
