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

This DEP defines how Agents implmenting a DEP API should handle authentication.


## Change Process

This document is governed by the [2/COSS](../2/README.md) (COSS).


## Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) \[[RFC2119](https://tools.ietf.org/html/rfc2119)\] \[[RFC8174](https://tools.ietf.org/html/rfc8174)\] when, and only when, they appear in all capitals, as shown here.


## Motivation

The main motivations of this DEP are:

* Define a standard set of authentication guidlines for DEP APIs
* Provide a basis for a DEP on Authorization

## Specification

Requirements are:

* Agents wishing to restrict acccess should use the authentication method
  defined in this DEP.
* The decision of whether or not to authenticate a user given any set of
  credentials is left to the policy of the service provider operating the agent

## Proposed Solution

Managing identity and handling authentication are challenging topics,
generally, and especially so on a platform designed for decentralization.

The methods of authentication are certain to evolve (most likely as
supersets) yet will begin with mechanisms familiar to developers today.

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
