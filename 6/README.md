```
shortname: 6/INVOKE
name: API to invoke compute services
type: Standard
status: Raw
editor: Kiran K <kiran.karkera@dex.sg>
contributor: Mike Anderson <mike.anderson@dex.sg>
```

<!--ts-->

Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Introduction](#introduction)
      * [Overview](#overview)
      * [Motivation](#motivation)
      * [Entities](#entities)
      * [Technical Requirements](#technical-requirements)
      * [Exclusions](#exclusions)
   * [API Definition](#api-definition)
      * [Methods](#methods)
      * [Authentication and Authorization](#authentication-and-authorization)
      * [Open Questions](#open-questions)
      * [License](#license)

      
<!--te-->

# Introduction

This section contains a non-normative introduction to the Invoke API.

The Invoke API is a specification to invoke compute operations. These compute operations could:

* Accept Input parameters (zero or more; which will typically be data assets to be used or algorithms to be run)
* Produce Outputs (zero or more; which will typically be references to generated data assets)

## Overview

Data publishers make data assets available for consumption. However, data assets are only the first part of a data pipeline. The finished products are usually models, predictions or dashboards and these are created by algorithms that transform the raw data, clean it, train models and generate predictions. 

This specification enables data ecosystem actors to build a data pipeline capable of transforming data assets.

## Motivation

The data ecosystem provides decentralised marketplaces for relevant AI-related data services.
There is a need for a standardised **interface** for the invocation of compute operations so that different implementations can be provided and invoked by users in the data ecosystem.

Actors in the data ecosystem could be categorized as:

- Data consumers/data scientists who transform data assets by using appropriate algorithms.
- Data publishers who make data assets available for consumption. These data assets may be available in raw form, or may be obfuscated using homomorphic encryption or other techniques to prevent data escapes.  
- Service providers providing data transformation algorithms and/or services. (e.g. a data cleaning service that removes outliers from data).

Example of operations that could be offered by data ecosystem actors:

* A data cleaning operation that removes noise from data
* A model training operation that returns a trained model given training data
* A model verification operation that returns metrics of a model's performance, given a model and a test data set.
* A consent filtering operation which filters a dataset by checking each dataset instance (e.g. a single patient's data in a healthcare study) against an external consent registry.

It may be observed that these operations:

- Enable creation of dataset(s)
- Accept input dataset(s) and transform it in some fashion
- May require remote acquisition of input dataset(s) 

The Invoke API 

- Provides tools to transform data assets.
- Facilitates a workflow pipeline of data asset transformations.
- Aid in establishing the provenance of data assets (by using DEP-12).

## Entities

- Account: An actor in the data system is identified using an Account.
- Asset: An Invoke service is registered as an Asset with a specific `type` (Operation) 
- Service provider: The actor that runs the algorithm on their server(s).     
  - InvokeEndpoint: The services are made available on one or more REST Endpoints on a Service Provider's server or cloud.
  
- Storage Provider: The entity providing a storage service compliant with [DEP-7](https://github.com/DEX-Company/DEPs/tree/master/7)
- Service consumer: The actor that invokes the service.

- Agent: The software entity that enables interactions with actors of the data ecosystem.
  
- Starfish : A library used by consumers and providers to interact with the data ecosystem.

The rest of this document is written for the perspective of a data consumer. 

### Consumer flow

![Consumer flow ](https://user-images.githubusercontent.com/89076/61201489-3f52fb00-a717-11e9-90c9-cb51fe1afbc2.png)

## Technical requirements 

The Invoke service
* May be offered for a price
* Must be identified with its DID
* Must register its metadata with an agent
* May accept asset(s) as input(s) to the job  (along with access tokens to consume the asset)
* May register assets generated as a result of the job. 
* May accept a JSON payload(s) as input(s)
* May return the result of the execution as the payload, or register the result of the execution as an asset.
* The unit of measurement must be 
  - a one-shot execution of a job (e.g. a data cleaning job)
  
## Exclusions

* This DEP does not prescribe the exact type of compute operations offered. It is open to service provider implementations to define them, providing that they conform with this API specification
* This DEP does not cover discovery.
* This DEP does not describe subscribable services, such as access to a dashboard for a fixed time period.
* This DEP does not describe details of installation of the operations and/or its dependencies. 

# API Definition 

This section is normative.

The **Operation Metadata** information should be managed using an API on the Invokable Rest Agent. 
The service providers hosting the API should expose the following capabilities in the Agent via HTTP REST. 


## Methods

The Invoke implementation must host the following APIs.

| API                    | Description                                                   | Path |
| -                      | -                                                             |-     |
| Invoke Async Operation | Invoke an operation asynchronously                            |/invokeasync/operation-id|
| Invoke Operation       | Invoke an operation synchronously                             |/invoke/operation-id|
| Get Job status         | Get the status of an invoked job, and the result if completed |/jobs/jobid|

- Unless mentioned otherwise, all requests, response and error payloads must be in JSON.
- The types of HTTP Requests (e.g. GET, POST) are defined in [RFC 2616](https://tools.ietf.org/html/rfc2616)

### Invoke async Operation

This is the primary interface by which a consumer can invoke an operation.

#### Request

The endpoint must accept 

- POST requests to the `/invokeasync/operation-did` (e.g. https://service-endpoint/invokeasync/4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea) along with JSON formatted payload as described by the params section of the operation metadata.

- The path argument operation-id must be the DID of the operation asset.
- The keys in the (payload) map must be parameter names as specified in the operation metadata.
- All required parameters must be included.

- The values must be one of 
  - A map (if the type is **asset**)
  - A valid JSON data type (if the type is a **json**)

#### Design considerations

This section is non-normative.

The values are categorized into two types (`asset` and `json`) to support libraries such as Starfish in:

- Validating if payloads adhere to the operation schema 
- Adding support for metadata such as `assets` which require `did`s and `access_token`s to be fully specified.


Here's an example of an request that defines a single input asset of type asset.

- The key `to_hash` is the parameter name
- Since the type is `asset` (as declared in the schema), the value must be a map with the `did` (and other optional keys)

```json 
{
    "to_hash": {
             "did" : "4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea",
             "access_token" : "cb0d65a716f613309693" 
    }
}
```

Here's an example of a parameterized request which specifies the algorithm to be used

```json 
{
    "to_sign": {
             "did" : "4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea",
             "access_token" : "cb0d65a716f613309693" 
    },
    "signing_algorithm": {
             "alg" : "ES256"
    },
}
```

#### Response

The response to a valid request must be a JSON encoded map with the **jobid** and the corresponding value. Invalid requests must return a response code as described below.

```json 
{
    "jobid": "630363ce4a6a9ded23"
}
```

| Response code | Description                                                                    | JSON payload                              |
|---------------|--------------------------------------------------------------------------------|-------------------------------------------|
|           201 | job creation success                                                           | map with jobid key                        |
| 404 | asset id not found | none|
|           400 | bad request-not according to presribed format or invalid configuration options | map with error code and error description |
|           401 | not authenticated (no authentication tokens provided)                              | map with error code and error description |
|           403| not authorized (no authorization tokens provided)                              | map with error code and error description |
|           500 | error                                                                          | -                                         |
|           503 | service unavailable                                                            | -                                         |
|               |                                                                                |                                           |

### Get job result 

#### Request

The endpoint must accept

- An HTTP GET request to `/jobs/jobid` 
- The path parameter `jobid` must be the value returned by the Invoke Async Operation response

#### Response

The response to a valid request must contain a JSON payload. 

- It must return a map with the `status` key, the value of which must be one of [scheduled|running|succeeded|failed|unknown] 
- Once the job has completed, and if it succeeded, it must also contain a map against the `result` key. The map with key(s) as defined in the `returns` section of the asset metadata.
 Each value in the map must be one of (as defined in the schema)

- A map (if type is **asset**)
- A JSON object (if type is **json**)


| Response code | Description                                       | Payload           |
|---------------|---------------------------------------------------|-------------------|
|           200 | job result                                        | Job result             |
| 404 | asset id not found | none|
|           401 | not authenticated  | error description |
|           403 | not authorised  | error description |

The Job result payload must have **errorcode** and **description** fields in case of error in executing the operation.

| Error code | Description                         |
|------------|-------------------------------------|
|       8001 | unknown job id                      |
|       8003 | service not paid for by consumer    |
|       8004 | asset id XX contents not accessible |
|            |                                     |

Example of an operation that succeeded and returns one asset 


```json
{ "status":"succeeded",
  "result": {"hashed_value": {
               "did" : "4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea",
               "access_token" : "630363ce4a6a9d"}
             }
}
```

Example of an operation that is in progress  

```json
{ "status":"running"}
```

Example of an operation that failed 

```json
{ "status":"failed",
  "errorcode":8004,
  "description":"Unable to access asset id 4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908fa"
}
```

Since the `json` type for params and results is quite loosely defined, implementations are free to define schemas for inputs (params) and outputs (results), either as part of the Operation metadata or elsewhere. It is recommended that implementations return an HTTP Bad Request response code, along with descriptive error message, in cases where the input payloads do not conform to the schema. 

### Invoke operation synchronously

This is an convenience interface by which a consumer can invoke an operation. This is a synchronous request, and is expected to be used for jobs that finish quickly.

#### Request

The endpoint must accept

- A POST request to the https://service-endpoint/invoke/operation_id along with JSON formatted payload described by the params section of the operation metadata.
- The path argument operation_id must be the ID of the operation asset.
- The payload is the same as the asynchronous invocation

#### Response

The response format is the same as returned by the get job result operation.

## Authentication and Authorization

Implementations may use various ways to authenticate and authorize that are transparent to the API.

## Open questions

- Authentication and Authorization headers are not yet defined in this DEP. Refer to [DEP 20](https://github.com/DEX-Company/DEPs/tree/master/20).
- Assets generated by the invoke service may need to be purchased by the consumer in order to view them. It is up to the service provider to decide if the created asset needs to be purchased, if required.


## License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
