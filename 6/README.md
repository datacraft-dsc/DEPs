```
shortname: 6/INVOKEAPI
name: API to register & invoke compute services
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
      * [Exclusions](#exclusions)
      * [Change Process](#change-process)
      * [Language](#language)
      * [Motivation](#motivation)
      * [Roles](#roles)
      * [Technical Requirements](#technical-requirements)
   * [Specification](#specification)
      * [Phases](#phases)
      * [Service Delivery](#service-delivery)
      * [FAQ](#faq)
      * [Open Questions](#open-questions)
      * [License](#license)

      
<!--te-->

# Introduction

The Invoke API (**INVOKE**) is a specification to register and invoke compute operations. These compute operations could:

* Accept Input parameters (zero or more; which will typically be data assets to be used or algorithms to be run)
* Produce Outputs (zero or more; which will typically be references to generated data assets)
* Support the provision of proofs by service providers upon completion (e.g after which tokens in escrow may be released) 

## Exclusions

* This DEP does not prescribe the exact type of compute operations offered. It is open to service provider implementations to define them, providing that they conform with this API specification
* This DEP does not cover discovery.
* This DEP does not describe subscribable services, such as access to a dashboard for a fixed time period.
* This DEP does not describe details of installation of the operations and/or its dependencies. 

## Change Process
This document is governed by the [2/COSS](../2/README.md) (COSS).

## Language
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) \[[RFC2119](https://tools.ietf.org/html/rfc2119)\] \[[RFC8174](https://tools.ietf.org/html/rfc8174)\] when, and only when, they appear in all capitals, as shown here.

## Overview

The Ocean ecosystem's data publishers make data assets available for consumption. However, data assets are only the first part of a data pipeline. The finished products are usually models, predictions or dashboards and these are created by algorithms that transform the raw data, clean it, train models and generate predictions. 

The Invoke API specification provides a path that satisfied three sets of actors

- for data consumers to apply algorithms to data assets
- for data publishers to make data assets available while choosing appropriate trust levels to prevent data escapes
- for service providers to make useful algorithms available

Diagram to be added

## Motivation

The Ocean ecosystem provides decentralised marketplaces for relevant AI-related data services.
There is a need for a standardised **interface** for the invocation of compute operations so that different implementations can be provided and invoked by users of the Ocean Protocol.

Example of operations that could be offered by Ocean actors:

* A data cleaning operation that removes noise from data
* A model training operation that returns a trained model given training data
* A model verification operation that returns metrics of a model's performance, given a model and a test data set.
* A consent filtering operation which filters a dataset by checking each dataset instance (e.g. a single patient's data in a healthcare study) against an external consent registry.

It may be observed that these operations:

- Enable creation of dataset(s)
- Accept input dataset(s) and transform it in some fashion
- May require remote acquisition of input dataset(s) 

The Invoke API 

- Provides Ocean users tools to transform data assets registered on the Ocean network.
- Facilitates a workflow pipeline of data asset transformations.
- Enables provenance tracking by Ocean provenance aware algorithms.

### Trust

Data publishers in the Ocean ecosystem want to publish data assets, and data consumers want to run algorithms on data assets they purchase. However, some datasets may contain sensitive information (e.g. personally identifiable information, health records, financial information) and data publishers may be concerned about the possibility of a data escape. 

The Invoke API provides a mechanism for Ocean ecosystem actors to run algorithms on data, while minimizing the possibility of data escape. The details of trust levels and the systems required to prevent data escape are described in a different DEP.



## Roles

- Asset/algorithm owner: The owner of the algorithm, 
  - May be registered as an Ocean asset
  - May be available as a deployable package (e.g. a docker image or a jar on a maven repo)
- Service provider: The actor that runs the algorithm on their server(s).
- Service consumer: The actor that invokes the service.
- Agent: The software entity that enables Service Instance interactions  with the rest of the Ocean community.
  - Agent can be of many types, such as local or remote, and communicate via different interfaces.
  - The rest of this document assumes a remote Agent that communicates over REST.

The rest of this document is written for the perspective of a data consumer. Data Publishers are advised to read DEP XX for instructions to implement Operations. 

### Consumer flow

![Consumer flow ](https://user-images.githubusercontent.com/89076/56566746-fdb62680-65e5-11e9-9bd0-987d45d0f876.png)

## Technical requirements 

The Invoke service
* May be offered free or for a price
* May be offered in trusted mode or trustless mode (backed by Service Execution Agreements) 
* Must be identified with its asset ID on the Ocean Network
* Must register its metadata with the OCEAN agent
* May accept a list of ocean assets as inputs to the job  (along with access tokens to consume the asset)
* May register ocean assets generated as a result of the job. the ownership of the registered assets is an open item. 
* May accept a data payload as an input
* May return a payload
* The unit of measurement must be 
  - a one-shot execution of a job (e.g. a data cleaning job)
  
# Specification 

The **Operation Metadata** information should be managed using an API on the Invokable Rest Agent. 
The service providers hosting the API should expose the following capabilities in the Ocean Agent via HTTP REST. 


## Service Delivery

The Invoke implementation must host the following APIs.

| API                    | Description                                                   | Path |
| -                      | -                                                             |-     |
| Invoke Async Operation | Invoke an operation asynchronously                            |/invokeasync/operation-id|
| Invoke Operation       | Invoke an operation synchronously                             |/invoke/operation-id|
| Get Job status         | Get the status of an invoked job, and the result if completed |/jobs/jobid|

Unless mentioned otherwise, all requests, response and error payloads must be in JSON.

### Invoke async Operation

This is the primary interface by which a consumer can invoke an operation.

#### Request

The caller API must make

- A POST request to the `/invokeasync/operation-did` (e.g. https://service-endpoint/invokeasync/4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea) along with JSON formatted payload as described by the params section of the operation metadata.

- The path argument operation-id must be the ID of the operation asset.
- The keys in the (payload) map must be parameter names as specified in the operation metadata.
- All required parameters must be included.

- The values must be one of 
  - A map (if the type is **asset**)
  - A valid JSON data type (if the type is a **json**)

#### Design considerations

The values are categorized into two types (`asset` and `json`) to support libraries such as Starfish in:

- Validating if payloads adhere to the operation schema 
- Adding support for Ocean-aware metadata such as `ocean assets` which require `did`s and `access_token`s to be fully specified.


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

If the server accepts the request, the response must be a JSON encoded map with the **jobid** and the corresponding value.

```json 
{
    "jobid": "630363ce4a6a9ded23"
}
```

| Response code | Description                                                                    | JSON payload                              |
|---------------|--------------------------------------------------------------------------------|-------------------------------------------|
|           201 | job creation success                                                           | map with jobid key                        |
|           400 | bad request-not according to presribed format or invalid configuration options | map with error code and error description |
|           401 | not authenticated (no authentication tokens provided)                              | map with error code and error description |
|           403| not authorized (no authorization tokens provided)                              | map with error code and error description |
|           500 | error                                                                          | -                                         |
|           503 | service unavailable                                                            | -                                         |
|               |                                                                                |                                           |

### Get job result 

#### Request

The caller API must make

- An HTTP GET request to `/jobs/jobid` 
- The path parameter `jobid` must be the value returned by the Invoke Async Operation response

#### Response

The response must contain a JSON payload. 

- It must return a map with the `status` key, the value of which must be one of [scheduled|running|succeeded|failed|unknown] 
- Once the job has completed, it must also contain a map against the `result` key. The map with key(s) as defined in the `returns` section of the asset metadata.
 Each value in the map must be one of (as defined in the schema)

- A map (if type is **asset**)
- A JSON object (if type is **json**)


| Response code | Description                                       | Payload           |
|---------------|---------------------------------------------------|-------------------|
|           200 | job result                                        | Job result             |
|           401 | not authenticated  | error description |
|           403 | not authorised  | error description |

The Job result payload must have **errorcode** and **description** fields in case of error in executing the operation.

| Error code | Description                         |
|------------|-------------------------------------|
|       8001 | unknown job id                      |
|       8003 | service not paid for by consumer    |
|       8004 | asset id XX contents not accessible |
|            |                                     |

Example of a an operation that succeeded and returns one asset 


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

### Invoke operation synchronously

This is an convenience interface by which a consumer can invoke an operation. This is a synchronous request, and is expected to be used for jobs that finish quickly.

#### Request

The caller API must make

- A POST request to the https://service-endpoint/invoke/operation_id along with JSON formatted payload described by the params section of the operation metadata.
- The path argument operation_id must be the ID of the operation asset.
- The payload is the same as the asynchronous invocation

#### Response

The response format is the same as returned by the get job result operation.


## Open questions

- Authentication and Authorization headers are not yet defined in this DEP. Refer to [DEP 20](https://github.com/DEX-Company/DEPs/tree/master/20).
- Assets generated by the invoke service may need to be purchased by the consumer in order to view them. It is up to the service provider to decide if the created asset needs to be purchased, if required.

- The details of access_token are yet to be defined. 
  - For Squid assets, the access_token must be a service agreement id to confirm that the asset's content can be accessed by the service provider. 

## License

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/)
