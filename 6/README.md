```
shortname: 6/CSAPI
name: API to register & invoke computer services   
type: Standard
status: Raw
editor: Mike Anderson <mike.anderson@dex.sg>
contributors: Kiran K <kiran.karkera@dex.sg> 
```

<!--ts-->

Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Invoke Job capability](#invoke-job-capability)
      * [Exclusions](#exclusions)
      * [Change Process](#change-process)
      * [Language](#language)
      * [Motivation](#motivation)
      * [Roles](#roles)
      * [Technical Requirements](#technical-requirements)
   * [Specification](#specification)
      * [Service Definition](#service-definition)
      * [Service Registration](#service-registration)
      * [Service Delivery](#service-delivery)
      * [FAQ](#faq)
      * [Open Questions](#open-questions)
      * [License](#license)

      
<!--te-->

<a name="service-invocation"></a>
# Invoke Job capability

The Service Invocation API (**INVOKE**) is a specification for the Ocean Protocol to register and invoke compute jobs.

INVOKE offers a general purpose computational interface that can run compute Jobs on demand.

Compute services are defined as services available on the Ocean Network that

* May accept one or more Input parameters (which will typically be data assets to be used or algorithms to be run)
* Typically produce one or more Outputs (which will typically be references to generated data assets)
* Support the provision of proofs by service providers upon service completion (after which tokens in escrow may be released) 

## Exclusions

* This OEP does not prescribe the exact type of compute services offered. It is open to service provider implementations to define them, providing that they conform with this API specification
* This OEP does not cover service discovery.
* The OEP is not intended to apply to services where invocation / access is off-chain (e.g. high volume APIs or queue services)
* This OEP does not describe subscribable services, such as access to a dashboard for a fixed time period.
* This OEP does not describe details of installation of the service and/or its dependencies. 

This specification is based on [Ocean Protocol technical whitepaper](https://github.com/oceanprotocol/whitepaper), [3/ARCH](../3/README.md), [4/KEEPER](../4/README.md) and [5/AGENT](../5/README.md).


<a name="change-process"></a>
## Change Process
This document is governed by the [2/COSS](../2/README.md) (COSS).

<a name="language"></a>
## Language
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) \[[RFC2119](https://tools.ietf.org/html/rfc2119)\] \[[RFC8174](https://tools.ietf.org/html/rfc8174)\] when, and only when, they appear in all capitals, as shown here.

<a name="motivation"></a>
## Motivation

Ocean network aims to power marketplaces for relevant AI-related data services.
There is a need for a standardised **interface** for the invocation of compute services so that different implementations can be provided and invoked by users of the Ocean Protocol.

Example of data related services that could be offered by Ocean actors:

* A data cleaning service that removes noise from data
* A model training service that returns a trained model given training data
* A model verification service that returns metrics of a model's performance, given a model and a test data set.
* A consent service which filters a dataset by checking each dataset instance (e.g. a single patient's data in a healthcare study) against an external consent registry.

It may be observed that these services

- Enable creation of dataset(s)
- Accept input dataset(s) and transform it in some fashion

The Invoke API enables

- provides Ocean users tools to transform data assets registered on the Ocean network.
- facilitates a workflow pipeline of data asset transformations.
- enables provenance tracking by Ocean provenance aware algorithms.

<a name="specification"></a>

## Roles

- Asset/algorithm owner: The owner of the algorithm, 
  - may be registered as an Ocean asset
  - may be available as a deployable package (e.g. a docker image or a jar on a maven repo)
- Service provider: The entity that runs the algorithm on their server(s).
- Service consumer: The entity that invokes the service.
- Service instance: The software entity that is running the invokable service. 
- Agent: The software entity that enables Service Instance interactions  with the rest of the Ocean community.
  - Agent can be of many types, such as local or remote, and communicate via different interfaces.
  - The rest of this document assumes a remote Agent that communicates over REST.

### Provider flow

![Provider flow ](https://user-images.githubusercontent.com/89076/55699303-d1eb4c00-59fc-11e9-9c93-59939e15283d.png)

### Consumer flow

![Consumer flow ](https://user-images.githubusercontent.com/89076/55699307-d6176980-59fc-11e9-9227-03a0d661bc57.png)

## Technical requirements 

* The service may be offered free or for a price
* The service may be offered in trusted mode or trustless mode (backed by Service Execution Agreements) 
* the service must be identified with its asset ID on the Ocean Network
* the service must register its metadata with the OCEAN agent
* may accept configuration options to tune the algorithm/job to be run.
* may register ocean assets generated as a result of the job. the registered assets must be in the name of the service coinsumer
* may return a payload
* may accept a list of ocean assets as inputs to the job  (along with access tokens to consume the asset)
* may accept a data payload as an input
* The unit of measurement must be 
  - a one-shot execution of a job (e.g. a data cleaning job)
  
# Specification 

The **Service Metadata** information should be managed using an API on the Ocean Agent. 
This API should expose the following capabilities in the Ocean Agent via HTTP REST.

The service provider makes the service available in two temporal phases:

### Service registration

The service provider registers the service and provides the spec for the Service Definition. This information describes 

- the endpoints at which the service(s) are available
- the configuration options accepted by the invoke method(s)
- list of mandatory and optional arguments and their types, accepted by the invoke methods
- sync/async nature of invoke methods
- data returned by methods.

### Service delivery

The Service Delivery phase consists of 

- the consumer calling the invoke method(s) 
- the service executing the invoke methods. These could be jobs that take a significant amount of time to complete.
- the consumer retrieving the result(s) of the invoke method(such as data payloads, created assets and/or associated proofs), or getting a notification (e.g. via a webhook).

The service spec consists of 2 parts, Service definition and Service Invocation. 

## Service definition

### Inputs/outputs

The invoke service can accept 
- one or more inputs
  - each of which must have a name and type*, 
- return one or more outputs 
  - each of which must have a name and type.

Currently, type *types* of inputs have been defined

- asset: these are registered Ocean assets
- string : string formatted data passed to the invocation. 

## Service Delivery

The rest of this document assumes that a REST Agent is used to delivering the service.

The Invoke API implementer needs to host the following APIs.

- Get Operation (returns a list of operations)
- Get Operation Details (returns the payload required by an operation)
- invoke Operation (invoke the operation)
- Get job status (get the status of an invoked job)
- Get job result (get the restult of an invoked job)


### Get Operations

Return a list of operations offered by this endpoint 

#### Request

- a GET request to the http://endpoint/operations

#### Response

returns a list, where each element is map containing:

- the DID of the operation
- the name of the operation

Example response with 2 operations:
```json
[
  {
    "did": "hashing_did",
    "name": "hashing"
  },
  {
    "did": "echo_did",
    "name": "echo"
  }
]
```

| response code | description          | payload |
|---------------|----------------------|---------|
|           200 | service available    | empty|
|           500 | server error  | data describing the error |
|           503 | service unavailable | data describing the error |

### Get Operation

Return the schema required by this operation

#### Request

- a GET request to the http://endpoint/services/operation/operation_did 

#### Response

Returns a list of arguments, where each element is a map. Each map can have 2 arguments:

- name of the argument.
- type of the asset. These can be of 2 type: asset, and string

Example response with a single asset argument:
```json
[
  {
    "name": "to_hash",
    "type": "asset",
  }
]
```

| response code | description          | payload |
|---------------|----------------------|---------|
|           200 | service available    | empty|
|           500 | server error  | data describing the error |
|           503 | service unavailable | data describing the error |

### Invoke a job 

This is the primary interface by which a consumer can invoke a service/run a job.

#### Request

- a POST request to the https://service-endpoint/api/v1/brizo/services/invoke/operation_did along with JSON formatted payload as described in the Service Definition.

Here's an example of an invocation that defines a single input asset of type asset.
```json 
{
    "to_hash": {
             "assetid" : "assetid",
             "purchase_token" : "value_of_purchase_token" 
             }
}
```


#### Response

| response code | description          | payload |
|---------------|----------------------|---------|
|           201 | job creation success | jobid   |
|           400 | bad request-not according to presribed format or invalid configuration options | error description|
|           401 | not authorized (no authorization tokens provided) | error description|
|           500 | error                | error description |
|           503 | service unavailable | error description|
|           8003 | service not paid for by consumer | error description|

### Describe the status of the job

#### Request

- an HTTP GET request to https://service-endpoint/jobs/status/jobid

The default return (for a valid jobid ) is a string enum with started/in progress/completed/errot.

#### Response

| response code | description                                                | payload                   |
|---------------|------------------------------------------------------------|---------------------------|
|           200 | job status, one of: started, in progress, completed, error | {"status" : "inprogress"} |
|           400 | invalid job id|  |
|           500 | error                                                      |                         error description  |
|           8001 | input assets cannot be retrieved |  error description|
|           8002 | output assets cannot be registered | error description |

### Get the result of a job

#### Request

- an HTTP GET request to https://service-endpoint/jobs/result/jobid

#### Response

| response code | description                                                | 
|---------------|------------------------------------------------------------|
|            200 | job result, a json formatted string| 
|           400 | invalid job id|  |
|           401 | not authorized (no authorization tokens provided) | error description|
|           500 | error                                                      | error description |

The json response is of the form

```json
{ "outputs" : [ "generatedassetid1", "generatedassetid2"]}

```

It can contain other keys such as non-Ocean payloads.
Note: this response section is underspecified. It needs to handle

- registering the generated asset on behalf of the service consumer
- specifying the service agreement, purchase price, additional metadata. 


## Open questions

- Authentication and Authorization headers are not yet defined in this DEP
- Assets generated by the invoke service needs to be purchased by the consumer in order to view them. The mechanics of this are yet to be defined.
- The details of purchase_token are yet to be defined. In the squid world, the purchase token is the service agreement id.

## License

This DEP is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 3 of the License, or (at your option) any later version.

This DEP is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program; if not, see http://www.gnu.org/licenses.

































