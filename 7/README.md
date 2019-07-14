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

This DEP supports provides a generaliased decentralised protocol for data exchange between actors in the deata economy.

To ensure that clients are able to make use of assets from service providers throughout the ecosystem, it is necessary
to provide a universal storage API for accessing asset content that is independent of the underlying storage implementation,
i.e. we wish to avoid tight coupling between the implementation of asset consumers and asset providers.

## Motivation

In the absence of a defined standard for storage, asset consumers would need to separately determine and negotiate
methods for accessing asset content. This would undermine the decentralised vision of providing universally interoperable
solutions for data and AI services - it would simply become a network of custom point-to-point integrations with
tight coupling between producers and consumers.

The main motivations of this DEP are:

* Establish a standard asset content storage API to allow interoperability between actors in the Data Economy
* Ensure the storage API is easy to use and consistent with existing internet tools and standards
* Ensure the standard enables support for key features of the Ocean Protocol


## Requirements

- The storage API must provide a simple interface to asset content via standard web protocols (HTTP / HTTPS)
- The storage API must support any type of asset content
- The storage API must allow content to be addressed by asset ID
- The storage API must integrate with relevant authentication and authorisation mechanisms

## Endpoints

| Name             | Method | URI                          |
|------------------|--------|------------------------------|
| uploadAsset      | PUT    | /api/v1/assets/{id}          |
| uploadAsset      | POST   | /api/v1/assets/{id}          |
| downloadAsset    | GET    | /api/v1/assets/{id}          |

## Upload

The upload endpoints are used by client to upload a file as raw content for storage for a given
asset ID. The asset must already be registered with the agent

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
- Storage agents should validate the content hash of uploaded data against the `contentHash` field of the asset metadata if this exists, and reject uplad with status 400 if the hash value is incorrect.
- Storage agents may store a single instance of data where multiple assets have the same content hash (e.g. by using content-addressed storage)


## Download

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
