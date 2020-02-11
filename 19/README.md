```
shortname: 19/ENDPOINTS
name: DEP Standard Endpoints
type: Standard
status: Draft
editor: Mike Anderson <mike.anderson@dex.sg>
contributors:
```

**Table of Contents**

<!--ts-->

   * [DEP Standard Endpoints](#dep-standard-endpoints)
      * [Motivation](#motivation)
      * [Specification](#specification)
         * [Endpoint Definition](#endpoint-definition)
         * [Endpoint Types](#endpoint-types)
      * [Implementations](#implementations)
      * [References](#references)
      * [License](#license)

<!--te-->

# DEP Standard Endpoints

This DEP defines how API endpoints should exposed by Agents in the ecosystem, and discovered
by other actors in the ecosystem that wish to utilise these endpoints.

Endpoints exposed are be declared in the relevant DDO of an Agent that offers these endpoints,
according to the W3C DID specification.


## Motivation

In order to enable clients to locate specific APIs offered by service providers, we must provide a 
method for a client to resolve network-accessible endpoints.

These endpoints are optional, but required if an service provider wishes their services 
to be consumed by standard DEP-compatible client tools, such as code written with the Starfish
developer toolkit.


## Specification

### Endpoint Definition

Agents wishing their endpoints to be utilised by clients should register their endpoints in the DDO
representing the agent via the Universal Resolver

Endpoints are specified in the following form in the DDO:

```json
{
  "service": [{
    "type": "DEP.Meta.v1",
    "serviceEndpoint": "https://mobi.com/api/v1/meta"
  }]
}
```

### Endpoint Resolution

Clients should resolve DEP Standard API endpoints for a given Agent using the following procedure:
1. Obtain the DID for the target agent
2. Obtain the DDO for the agent's DID using the Universal Resolver
3. Look up the desired endpoint type in the DDO's "services" section
4. Make use of the "serviceEndpoint" thus identified for subsequent API calls

Clients may adopt alternative mechanisms for resolving endpoints, however this is not 
recommended as this approach will not be support interoperability across the ecosystem.

### Endpoint types

Each DEP Standard API must be allocated a unique endpoint type.

The endpoint type must include a version number, to allow for protocol evolution.

Each Agent's DDO may specify one or more services as per the W3C DID spec

If the service represents an DEP Standard API, the "type" of the service must be the endpoint type

Current allocated endpoint types are listed below:

Endpoint type           |   Description
------------------------|----------------------
DEP.Meta.v1           | Endpoint for the Meta Agent API v1 ([DEP15](https://github.com/DEX-Company/DEPs/tree/master/15))
DEP.Invoke.v1         | Endpoint for an invokable service API ([DEP6](https://github.com/DEX-Company/DEPs/tree/master/6))
DEP.Storage.v1        | Endpoint for a generalised storage API ([DEP7](https://github.com/DEX-Company/DEPs/tree/master/7))
DEP.Auth.v1           | Endpoint the Authentication API ([DEP20](https://github.com/DEX-Company/DEPs/tree/master/20))
DEP.Market.v1         | Endpoint for the Market Agent API (TBC)

## Implementations

DEP Standard API endpoint resolution is supported in the Starfish Developer Toolkit.

## References

* https://w3c-ccg.github.io/did-spec/

## License

Copyright (c) 2019-2020 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
