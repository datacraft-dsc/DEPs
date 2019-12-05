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

The Invoke API is a standardised solution to the problem of transforming data (Assets) using [Operations](https://github.com/DEX-Company/DEPs/tree/master/0#operation)  in 
the context of a decentralised data ecosystem.

In order to build a useful [data supply line](https://github.com/DEX-Company/DEPs/tree/master/0#data-supply-line), actors in the data ecosystem will generally need the ability 
to transform and process data assets. 

Operations can segmented into three broad use cases:

### Data transformation on public assets

Examples:

* A data cleaning operation that removes noise from data
* A model training operation that returns a trained model given training data
* A model verification operation that returns metrics of a model's performance, given a model and a test data set.

### Operations on private assets

Examples:

* Run model training on private data.
* Run model accuracy tests on a private test dataset.
* Request to run a proprietary, private algorithm on a public data asset.

### Retrieval queries that require provenance

Example:

* A consent filtering operation which filters a dataset by checking each dataset instance (e.g. a single patient's data in a healthcare study) against an external consent registry. The use case requires that the [Provenance](https://github.com/DEX-Company/DEPs/tree/master/0#provenance) of this query be associated with the retrieved result.

It may be observed that these operations:

- Enable creation of dataset(s)
- Accept input dataset(s) and transform it in some fashion
- May require remote acquisition of input dataset(s) 

The Invoke API 

- Provides a standardised interface to transform data assets.
- Facilitates creation of data pipeline(s) of data asset transformations.
- Aid in establishing the provenance of data assets (by using DEP-12).

![User flow](https://user-images.githubusercontent.com/89076/61606548-78dab780-ac7d-11e9-9e41-6364988a6652.png)

## Trustworthiness of executed Operations

DEP6 deals with the execution of computations within the security context of a service provider. As such,
the service provider must consider the trustworthiness of any Operations that it allows to be invoked. It should 
be considered a major security risk to allow arbitrary consumers to execute arbitrary code, therefore sevice providers
should almost always enforce some restrictions.

Several approaches exist for providing more trustworthy execution, which may be employed at the 
discretion of the Service Provider, balancing the needs of their consumers with the potential risks.

| Approach | Description | Advantages | Risks and limitations |
| - | - | - | - |
| Restricted Operations | The service provider offers only a limited set of Operations that it explicitly approves and/or maintains | <ul><li>High degree of control over code executed</li><li>Service provider may offer proprietary code without revealing algorithms</li></ul> | </ul><li>Consumers have limited choice of Operations</li><li>Service providers must fully anticipate usage and create appropriate API designs</li></ul> |
| Domain Specific Language | The service provider allows consumers to specify code with a constrained Domain Specific Language (DSL) that is designed to only allow certain language functions which the service provider considers safe | <ul><li>Gives relevant flexibility to Consumers within a given domain</li><li>May be secure given correct DSL design and implementation</li><li>May be possible to prove properties of DSL (e.g. non-Turing completeness)</li></ul> | </ul><li>Consumers are limited to capabilities provided by DSL</li><li>Risk of security flaws in DSL design and implementation</li></ul> |
| Secure Sandbox | The service provider allows consumers to specify arbitrary code to execute, but runs this within a secure sandboxed execution environment (e.g. a docker container) | <ul><li>Significant flexibility to consumers, may use existing preferred languages</li></ul>| <ul><li>Security dependent on the implementation of the sandoxed environment</li><li>May require additional controls on resource usage</li></ul> |
| Trusted Consumers | The service provider permitted only trusted Consumers to execute code (e.g. with a whitelist) | <ul><li>May offer significant capabilities to trusted consumers</li><li>Reduced requirement to secure the execution environment</li></ul> | <ul><li>Risk of stolen trusted credentials allowing an attacker to execute arbitrary code</li></ul> |

Service providers may also employ a combinations of the above techniques.



## Entities

- [Operation](https://github.com/DEX-Company/DEPs/tree/master/0#operation): A computation that may be executed via the Invoke API is as represented an Asset with a specific `type` ("operation") 
- Account: An actor in the data system is identified using an Account.
- [Service provider](https://github.com/DEX-Company/DEPs/tree/master/0#service-provider): The actor that hosts the algorithm implemention on their server(s).
  - InvokeEndpoint: The operation is made available on one or more REST Endpoints on a Service Provider's server or cloud.
  
- Storage Provider: The entity providing a storage service compliant with [DEP-7](https://github.com/DEX-Company/DEPs/tree/master/7). Note that the Invoke service provider may also run a Storage service, or use an external Storage service.

- Service consumer: The actor that invokes the service.

- [Agent](https://github.com/DEX-Company/DEPs/tree/master/0#agent): The software entity that enables interactions with actors of the data ecosystem.
  
- [Starfish](https://github.com/DEX-Company/DEPs/tree/master/0#starfish) : A library used by consumers and providers to interact with the data ecosystem.

The rest of this document is written for the perspective of a data consumer. 

### Consumer flow

The following diagram is an example of consumer using Starfish to invoke an operation. Note that in this case, the Invoke Service provider uses an external Storage Provider. 

![Consumer flow ](https://user-images.githubusercontent.com/89076/61201489-3f52fb00-a717-11e9-90c9-cb51fe1afbc2.png)

## Technical requirements 

The Invoke service

* May be offered for a price
* Must be identified with its DID
* Must register its metadata with an agent
* May accept asset(s) as input(s) to the job  (along with access tokens to consume the asset)
* May accept a JSON payload(s) as input(s)
* May return the result of the execution as the payload or as an asset
  - If it returns **asset**, it must register the generated asset(s) 
* Recommends that the operation must terminate within a specified time. The Invoke service is not designed to support subscription services (e.g. a service that is active for 30 days).
  
## Exclusions

* This DEP does not prescribe the exact type of compute operations offered. It is open to service provider implementations to define them, providing that they conform with this API specification
* This DEP does not cover discovery.
* This DEP does not describe subscribable services, such as access to a dashboard for a fixed time period.
* This DEP does not describe details of installation of the operations and/or its dependencies. 

# API Definition 

This section is normative.

The **Operation Metadata** information should be managed using [Metadata Agent API](https://github.com/DEX-Company/DEPs/tree/master/15). 
The service providers hosting the API should expose the following capabilities in the Agent via HTTP REST. 

Here's an example of the Operation metadata for an operation that removes empty rows from a data asset.
```
{
    "license": "CC-BY",
    "dateCreated": "2019-05-07T08:17:31.521445Z",
    "author": "Filtering Inc",
    "name": "Filter rows in a CSV dataset",
    "inLanguage": "en",
    "description": "filter rows in a csv dataset, if more than 10 columns are empty",
    "type": "operation",
    "contentType": "application\/octet-stream",
    "operation": {
        "modes": ["sync", "async"],
        "params": {
            "dataset": {
                "type": "string",
                "x-format": "did-asset",
                "position": 0,
                "required": true
            },
            "max-empty-columns": {
                "type": "integer",
                "position": 1,
                "required": false
            },
            "dataset-filter": {
                "type": "object",
                "propereties": {
                    "id": {
                    "type": "integer"
                },
                    "name": {
                        "type": "string"
                    }
                }
                "position": 2,
                "required": false
            },
            "other-agents": {
                "type": "array",
                "items": {
                    "type": "string",
                    "x-format": "did"
                }
                "position": 3,
                "required": false
            }
        },
        "results": {
            "filtered-dataset": {
                "type": "string",
                "x-format": "did-asset"
            }
        }
    },
  "contentHash": "4e03657aea45a94fc7d47ba826c8d667c0d1e6e33a64a036ec44f58fa12d6c48",
  "tags": ["row filtering"]
```

## Methods

The Invoke implementation must host the following APIs.

It is the recommended that the server host the API under the path `/api/<version>/invoke`.

| API                    | Description                                                   | Path |
| -                      | -                                                             |-     |
| Invoke Async Operation | Invoke an operation asynchronously                            |/async/operation-id|
| Invoke Operation       | Invoke an operation synchronously                             |/sync/operation-id|
| Get Job result         | Get the result if completed |/jobs/jobid|

- Unless mentioned otherwise, all requests, response and error payloads must be in JSON.
- The types of HTTP Requests (e.g. GET, POST) are defined in [RFC 2616](https://tools.ietf.org/html/rfc2616)

### Invoke async Operation

This is the primary interface by which a consumer can invoke an operation.

#### Request

The endpoint must accept 

- POST requests to the `/async/operation-id` (e.g. https://service-endpoint/async/4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea) along with JSON formatted payload as described by the params section of the operation metadata.

- The path argument operation-id must be the ID of the operation asset.
- The keys in the (payload) map may be parameter names as specified in the operation metadata.
- All required parameters may be included.

- The values can be any of following:
   OpenAPI SchemaObject [OpenAPI Schema Object standards] (https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md#schema-object).
   This may include the following OpenAPI schema data types:
        object
        array
        integer
        number
        string
        boolean
        
- The parameters may be used to display swagger interface documentation.

#### Design considerations

This section is non-normative.

* The `string` data type mave have the extended fields:
    - x-resolve
    - x-format


* The x-resolve and x-format must have one of the following values:
    - did-asset
    - did

- **did-asset** describes the type asset as a did/asset. For example


```
    # full asset id
    
    did:op:4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea/0xf8c12e287be8199a8c12eb9bbb63bc302c318b2290ef1e40af5a934e3315022d
```

- **did** describes a single did agent. For example

```
    # did resolve to an agent
    
    did:op:4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea
    
```

- **x-resolve** Trys to resolve the asset id or did depending on the reference value.

- **x-format** Validates the string value to make sure the string value is the correct format ( did or did-asset)


Here's an example of an request that defines a single input asset of type asset. The operation accepts an asset as input and returns the hash of the asset.

- The key `to_hash` is the parameter name required by the operation (as declared in the operation's metadata)
- Since the type is `string` with the `x-format` set to `did-asset` (as declared in the schema), the value must be a full asset address with did and asset id.

```
{
    "to_hash": {
         "asset_id" : {
            "type": "string",
            "x-format": "did-asset"
        }
    }
}

# calling

{
    "to_hash": {
         "asset_id" : "did:op:4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea/0xf8c12e287be8199a8c12eb9bbb63bc302c318b2290ef1e40af5a934e3315022d"
    }

}
```

Here's an example of a request anoher operation that requires just the agent which also includes an optional parameter, the algorithm to be used for computing the hash.

```
{
    "to_sign": {
         "did" : {
            "type": "string",
            "x-format": "did"
        }
        "asset": {
            "type": "string",
        }
    }
    "signing_algorithm": {
        "algorithm": {
            "type": "string"
        }
    }
}

# calling 
{
    "to_sign": {
        "did" : "did:op:4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea"
        "asset": "0xf8c12e287be8199a8c12eb9bbb63bc302c318b2290ef1e40af5a934e3315022d"
    },
    "signing_algorithm": {
        "algorithm" : "ES256"
    },
}
```

#### Response

The response to a valid request must be a JSON encoded map with the **jobid** and the corresponding value. Invalid requests must return a response code as described below.

The Service Provider should reject operations that the requestor is not authorised to perform with a 403 status.
Service providers should use this 403 response in situations where the operation requested is not trusted for
security reasons.

The choice of schema for the jobid's value is left to the implementor of the operation.

```
{
    "jobid": "630363ce4a6a9ded23"
}
```

| Response code | Description                                                                    | JSON payload                              |
|---------------|--------------------------------------------------------------------------------|-------------------------------------------|
|           201 | job creation success                                                           | map with jobid key                        |
| 404 | asset id not found | none|
|           400 | bad request-not according to prescribed format or invalid configuration options | map with error code and error description |
|           401 | not authenticated (no authentication tokens provided)                              | map with error code and error description |
|           403| not authorised (no authorisation tokens provided)                              | map with error code and error description |
|           5XX | error                                                                          | -                                         |
|               |                                                                                |                                           |

### Get job result

#### Request

The endpoint must accept

- An HTTP GET request to `/jobs/jobid` 
- The path parameter `jobid` must be the value returned by the Invoke Async Operation response

#### Response

The response to a valid request must contain a JSON payload. 

- It must return a map with the `status` key, the value of which must be one of [scheduled|running|succeeded|failed|cancelled] 

#### State transitions

|Status |Description|
|-|-|
|scheduled| The job has been scheduled, but has not yet started|
|running|The job is currently running|
|succeeded|The job completed successfully|
|failed|The job failed to complete successfully|
|cancelled|The job was cancelled|

Note that:
- Implementations must adhere to the state transitions described in this diagram. 
- A job can go from state `scheduled` to `failed` because it was unable to access data assets required for execution 

![Job status state transitions](https://user-images.githubusercontent.com/89076/65857597-be5ee380-e396-11e9-997c-a10bac51f0ed.png)


- Once the job has completed, it must contain a map against the `results` key. The map with key(s) as defined in the `returns` section of the asset metadata.
 Each value in the map must be one of (as defined in the schema)

   OpenAPI SchemaObject [OpenAPI Schema Object standards] (https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md#schema-object).
   This may include the following OpenAPI schema data types:
        object
        array
        integer
        number
        string
        boolean
    With the `string` data type extensions `x-format`.



| Response code | Description                                       | Payload           |
|---------------|---------------------------------------------------|-------------------|
|           200 | job result                                        | Job result             |
| 404 | job id not found | none|
|           401 | not authenticated  | error description |
|           403 | not authorised  | error description |

The Job result payload must have **errorcode** and **description** fields in case of error in executing the operation.
The following table lists error codes that are specific to operations. This list is indicative and may be expanded in future.

| Error code | Description                         |
|------------|-------------------------------------|
|       8003 | service not paid for by consumer    |
|       8004 | asset id XX contents not accessible |
|            |                                     |

Example of an operation which hashes the value of an asset. 

```
{ 
  "status":"succeeded",
  "results": {"hashed_value": "4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea"}
}
```

Example of an operation that failed 

```
{ 
  "status":"failed",
  "results":{
  "errorcode":8004,
  "description":"Unable to access asset did:op:4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908fa"
  }
}
```

Since the `OpenAPI` type for params and results allows arbitrary results, implementations are free to define schemas for inputs (params) and outputs (results). Implementations should return an HTTP Bad Request response code, along with descriptive error message, in cases where the input payloads do not conform to the schema. 

### Invoke operation synchronously

This is an convenience interface by which a consumer can invoke an operation. This is a synchronous request, and is expected to be used for jobs that finish quickly. Since the implementation is on top of low level protocols (as TCP/IP) the user should be aware of that waiting time may be limited by these protocols.

#### Request

The endpoint must accept

- A POST request to the https://service-endpoint/invoke/operation_id along with JSON formatted payload described by the params section of the operation metadata.
- The path argument operation_id must be the ID of the operation asset.
- The payload is the same as the asynchronous invocation

#### Response

The response format is the same as returned by the get job result operation. 

## Authentication and Authorisation

- Implementations should ensure that requests which may result in execution of untrusted code are tightly controlled
- Implementations may use various ways to authenticate and authorise that are transparent to the API. Refer to [DEP 20](https://github.com/DEX-Company/DEPs/tree/master/20) for further details.
- Implementations may use specific access rules for each API (e.g. getting job status) 

## Open questions

- Assets generated by the invoke service may need to be purchased by the consumer in order to view them. 

## Reference implementations

- [Koi-clj](https://github.com/DEX-Company/koi-clj) is a reference implementation of an invokable service. 

## Related Standards

- [Provenance DEP](https://github.com/DEX-Company/DEPs/blob/master/12)
- [Metadata DEP](https://github.com/DEX-Company/DEPs/blob/master/8)

# License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
