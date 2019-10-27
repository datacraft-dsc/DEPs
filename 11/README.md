```
shortname: 15/AGENT-STATUS
name: Agent Status API
type: Standard
status: Raw
editor: Mike Anderson <mike.anderson@dex.sg>
contributors: Mike Anderson <mike.anderson@dex.sg>
```


Table of Contents
=================

   * [Table of Contents](#table-of-contents)


# Agent Status API

This DEP describes an API by which Clients may request key status information from an Agent.
- The version(s) of the DEP Standard API supported
- The DDO of the Agent, for obtaining a DID / DDO mapping directly 


## Motivation

In some circumstances, Clients may need to dynamically query an Agent to establish key information about agent status
and capabilities.

The following specific issues are addressed in this DEP:
- A Client wishes to obtain a DID/DDO from an Agent, but only knows the Agent's host address so cannot make use of a Resolver
- A client wishes to query which versions of the DEP Standard APIs are supported

### Agent Status Endpoints


| Name             | Method | URI                          |
|------------------|--------|------------------------------|
| getDDO           | GET    | api/ddo                      |
| getStatus        | GET    | api/status                   |

Unlike other DEP Standard APIs, Agent Status endpoints do not specify a version number. 


### getDDO

Performing a HTTP GET on the `api/ddo` endpoint must return a valid W3C DDO, or a 404 if no such DDO exists.

Example payload:

```json
{
  "@context": "https://www.w3.org/2019/did/v1",
  "id": "did:op:08e3b57916b153ecc3ba687b123e26eb15e9e8eb73311607788018559ec354c7",

  "service": [{
    "id:": "did:op:08e3b57916b153ecc3ba687b123e26eb15e9e8eb73311607788018559ec354c7#meta",
    "type": "Ocean.Meta.v1",
    "serviceEndpoint": "https://my.agent1234.com/ap1/v1/meta"
  }]
}

```


### getStatus

Performing a HTTP GET on the `api/status` endpoint should return a HTTP OK (200) response with a JSON map containing the
following values:

Attribute          |   Type          | Description
-------------------|-----------------|----------------------
**name**           | String          | Human-readable, descriptive name of the Agent e.g. "DEX Test Agent"
**description**    | String          | Extended description of the Agent. Free text at discretion of Agent operator
**api-versions**   | Array of String | List of API versions supported e.g. ["v1","v2"]. If missing, "v1" should be assumed by default
**custom**         | Any JSON Value  | Optional additional metadata provided by agent operator. Outside DEP standards.


## License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
