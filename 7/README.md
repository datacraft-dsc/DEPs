```
shortname: 7/STORAGE
name: Universal Storage API
type: Standard
status: Draft
editor: Mike Anderson <mike.anderson@dex.sg>
contributors: 
```

Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Universal Storage](#universal-storage)
      * [Requirements](#requirements)
      * [Change Process](#change-process)
      * [Language](#language)
      * [Motivation](#motivation)
      * [References](#references)


# Universal Storage 

This DEP supports a generalised decentralised protocol for data exchange between actors in the data economy by providing an abstract API for asset content upload and download.

To ensure that clients are able to make use of assets from a diverse set service providers throughout the ecosystem, it is necessary
to provide a universal storage API that is independent of the underlying storage implementation,
i.e. we wish to avoid tight coupling between the implementation of asset consumers and asset providers. 

> “Everything should be made as simple as possible, but no simpler”
>
> (attributed to Albert Einstein)

The Storage API defined here is intended to be "The Simplest Thing That Could Possibly Work". It has the following 
properties:
- Asset content can be uploaded and retrieved via a REST API using standard HTTP methods
- Request URLs can be simply calculated using the Asset ID
- All typical success and failure conditions are communicated via regular HTTP status codes.

## Motivation

We want any client in the data ecosystem to be able to access data content stored by any service provider, 
regardless of the underlying technology choices made by the service providers (which are expected to evolve
 over time). 
 
This concept is analogous to HTTP: Web browsers should not need to know anything about the underlying 
implementation of a web server in order to execute a GET request for an HTML page. 

In the absence of such a defined standard for storage, asset consumers would need to separately determine and negotiate
methods for accessing asset content. This would undermine the decentralised vision of providing universally interoperable
solutions for data and AI services - it would simply become a network of custom point-to-point integrations with
tight coupling between producers and consumers.


## Requirements

- The storage API must provide a simple interface to asset content via standard web protocols (HTTP / HTTPS)
- The storage API must support any type of asset content (assuming it can be expressed as an ordered sequence of byte values)
- The storage API must allow content to be addressed by the Asset's DID
- The storage API must integrate with relevant authentication and authorisation mechanisms
- The storage API should be easy to use and consistent with existing Internet tools and standards as far as possible

## API Specification

### Endpoints

| Name             | Method | URI                          |
|------------------|--------|------------------------------|
| uploadAsset      | PUT    | /api/v1/assets/{id}          |
| uploadAsset      | POST   | /api/v1/assets/{id}          |
| downloadAsset    | GET    | /api/v1/assets/{id}          |

The Storage endpoints for any given asset DID can be constructed using the following procedure:

1. Use a Universal Resolver to obtain the `DDO` for the `DID`
2. Extract the `"Ocean.Storage.v1"` `Service Endpoint` from the DDO e.g. `"https://big.data-service.com/api/v1/assets"`
3. Take the `Asset ID` from the `DID Path` e.g. `"23e33783fa7e81a78ed05310bdd4568e6cf23bf2a8d1f2498435f33e9b1848d1"`
4. Compute `[Service Endpoint] + "/" + [Asset ID]` to get the correct Storage API URI

### Upload

The upload endpoints are used by client to upload a file as raw content for storage for a given
asset ID. The Storage API is agnostic to the file format used for content.

The asset metadata must already be registered with the agent. The result of
attempts to upload content for an unregistered asset is undefined.

The PUT and POST endpoints perform the same function, with the following difference:
- For PUT, the asset content must be the body of the HTTP PUT request
- For POST, the asset content must be provided as a file (HTTP multipart form data)

Success or failure of the upload operation is indicated as follows:

| Response code | Description                                       | Payload           |
|---------------|---------------------------------------------------|-------------------|
|           200 | Upload successful                                 | None              |
|           400 | Invalid content (hash failure)                    | error description |
|           401 | not authenticated                                 | error description |
|           403 | not authorised                                    | error description |
|           404 | Asset not registered / found                      | error description |


Upload follows the following rules:
- Storage agents must ensure the data is persisted in storage in the case of a 2xx response
- Storage agents should reject requests (status 401) in the case of authentication failure
- Storage agents should reject requests (status 403) in the case of authorisation failure
- Storage agents must reject (status 404) requests to upload a non-existent asset
- Storage agents should validate the content hash of uploaded data against the `contentHash` field of the asset metadata if this exists, and reject upload with status 400 if the hash value is incorrect.
- Storage agents may store a single instance of data where multiple assets have the same content hash (e.g. by using content-addressed storage)


### Download

The download endpoint enables clients to download the content of a specific asset.

Success or failure of the upload operation is indicated as follows:

| Response code | Description                                       | Payload           |
|---------------|---------------------------------------------------|-------------------|
|           200 | Success                                           | Asset content     |
|           401 | not authenticated                                 | error description |
|           403 | not authorised                                    | error description |
|           404 | Asset not registered / visible                    | error description |


Download behaviour follows the following rules:
- Storage agents should deliver downloaded content via TLS (HTTPS protocol)
- The HTTP response should have a HTTP `Content-Type` set to the content type specified in the asset metadata if this exists, otherwise `application/octet-stream` for content of unknown type.
- The HTTP response may include a `Content-Disposition` header appropriate to the asset (see relevant IETF RFCs)
- The storage agent must apply any relevant authorisation / authentication methods and return HTTP status 401 or 403 in event of failure
- The storage agent should return HTTP status 404 rather than 403 if the requestor does not have access to asset metadata (this is to prevent information leaks regarding the existence of the asset)



## References

TBC:

* A - https://foo.org

## License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
