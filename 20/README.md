```
shortname: 20/AUTHENTICATION
name: Authentication
type: Standard
status: Raw
editor: Tom Marble <tmarble@info9.net>
contributors:
```

**Table of Contents**

<!--ts-->

   * [Authentication](#authentication)
      * [Change Process](#change-process)
      * [Language](#language)
      * [Motivation](#motivation)
      * [Specification](#specification)
         * [HTTP Basic Authentication](#http-basic-authentication)
         * [OAuth2 Token Authentication](#oauth2-token-authentication)
      * [Changes Required](#changes-required)
      * [References](#references)

<!--te-->

# Authentication

This DEP defines how Starfish applications should handle authentication.


## Change Process

This document is governed by the [2/COSS](../2/README.md) (COSS).


## Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) \[[RFC2119](https://tools.ietf.org/html/rfc2119)\] \[[RFC8174](https://tools.ietf.org/html/rfc8174)\] when, and only when, they appear in all capitals, as shown here.


## Motivation

The main motivations of this DEP are:

* Define a standard set of authentication guidlines for Starfish APIs
* Provide a basis for a DEP on Authorization

## Specification

Requirements are:

* Each Starfish API may restrict access to authenticated users

## Proposed Solution

Managing identity and handling authentication are challenging topics,
generally, and especially so on a platform designed for decentralization.

The methods of authentication are certain to evolve (most likely as
supersets) yet will begin with mechanisms familiar to developers today.

It is recommended that implementations serve API's over TLS (SSL) as
to ensure HTTP headers are not sent in cleartext.

### HTTP Basic Authentication

A Starfish application may have specified users (including usernames
and passwords) as part of an initial configuration.

These credentials may be used with HTTP Basic Authentication.

### OAuth2 Token Authentication

Each Starfish service should have a mechanism (API) for generating
access tokens.

These tokens act as a means to associate an API request with
a specific user and then evaluate authorizations for that that user.

In the future such tokens may narrowly define specific access
grants (to be covered in a DEP on Authorization).

## Changes Required

The list of changes to apply in the proposed solution are:

* Modify Starfish libraries to support one (or more) of the proposed Authentication schemes

## References

* https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication
* https://oauth.net/2/
* https://developer.github.com/v3/#authentication
