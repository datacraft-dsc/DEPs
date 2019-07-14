```
shortname: 15/META-API
name: Metadata Agent API
type: Standard
status: Raw
editor: Bill Barman <bill@oceanprotocol.com>
contributors: Mike Anderson <mike.anderson@dex.sg>
```


Table of Contents
=================

   * [Table of Contents](#table-of-contents)



# Metadata Agent API

This OEP describes the API by which Agents in the Ocean ecosystem can store and provide verifiable metadata to Ocean clients.

Metadata is identified by the hash of its content, i.e. a Meta Agent acts as content-addressable storage of metadata records.

This service is primary used for storing Asset meta data, but should be open enough to provide meta data storage for other forms of metadata within the Ocean ecosystem.

## Change Process

This document is governed by the [2/COSS](../2/README.md) (COSS).

## Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) \[[RFC2119](https://tools.ietf.org/html/rfc2119)\] \[[RFC8174](https://tools.ietf.org/html/rfc8174)\] when, and only when, they appear in all capitals, as shown here.


## Motivation

The main motivations of this OEP are:

* Ensure that clients can access metadata for any asset or off-chain data structure in the Ocean ecosystem
* Ensure that the data can be verifiable and persistent within the storage system.


#### Base URL

/api/v1/meta

| Name             | Method | URI                          |
|------------------|--------|------------------------------|
| addMetadata      | POST   | /data/{asset_id}             |
| addMetadata      | PUT    | /data/{asset_id}             |
| getMetadata      | GET    | /data/{asset_id}             |


-------------------------------------------------------------------------------
### addMetaData

Add meta data to the server storage. The message body will be hashed using keccak256 to
generate the asset id.

If using the PUT method with {asset_id}, the asset ID will be compared to the given asset ID to ensure integrity. An error will occur if the hash is invalid.

If the above executes correctly, the Meta Storage Server must ensure the metadata is persisted in its own storage, associating the {asset_id} with this metadata.

Meta agents are recommended to limit usage of this service to authorised clients to avoid issues with spam.



#### Inputs

The metadata JSON must be provided in the message body. A content type of `application/json` is assumed.

Metadata should be in JSON format consistent with Asset Metadata format as defined in DEP8. Metadata agents should
check for valid metadata format and return status 400 in case of error.

The Asset Id is provided in the URL in the PUT case, in which case is it used for validation. This is not required in the POST case. The two methods are otherwise identical.


#### Response

If the server accepts the request, the response must be a JSON string value containing the Asset ID.

Example:


```json 
"2baa473780e786e9e513fc285f443b6bf015a67939ecc82d4fa30bc9284e7436"
```

| Response code | Description                                                                    | JSON response payload                     |
|---------------|--------------------------------------------------------------------------------|-------------------------------------------|
|           200 | The asset was successfully stored as new metadata (POST)                       | Asset ID as string                        |
|           201 | The asset was successfully stored as new metadata (PUT)                        | Asset ID as string                        |
|           400 | The asset was not in a permitted format, or validation failed                  | map with error code and error description |
|           401 | Authentication was required but valid authentication not provided              | map with error code and error description |
|           403 | The caller is not authorised to register the asset metadata                    | map with error code and error description |
           

-------------------------------------------------------------------------------
### getMetaData

Gets metadata stored by the server for a given Asset ID

It is the responsibility of the agent's API implementation to enforce access control on metadata access if any is required.


#### Inputs

The Asset ID (i.e. the keccak256 hash of the metadata) is provided in the URL.

#### Outputs

The API should return the metadata of the requested Asset ID if available. 

This returned metadata should be identical to the metadata provided to the addMetadata endpoint, i.e. it should be possible to validate the hash code of the returned metadata as the Asset ID.

```json

  < Metadata as per DEP8 >

```

Clients may choose to verify the integrity of the metadata by computing the keccak256 hash of the returned metadata and comparing to
the requested metadata ID.


| Response code | Description                                                                    | JSON response payload                     |
|---------------|--------------------------------------------------------------------------------|-------------------------------------------|
|           200 | Metadata successfully retreived                                                | Asset Metadata as JSON                    |
|           400 | The request was incorrectly formed (i.e. invalid asset ID)                     | map with error code and error description |
|           401 | Authentication was required but valid authentication not provided              | map with error code and error description |
|           403 | The caller is not authorised to register the asset metadata                    | map with error code and error description |
|           404 | The asset metadata was not found                                               | map with error code and error description |

## License

Copyright (c) 2018 OceanProtocol Foundation.
Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License