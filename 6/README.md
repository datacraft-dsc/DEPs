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

* This DEP does not prescribe the exact type of compute services offered. It is open to service provider implementations to define them, providing that they conform with this API specification
* This DEP does not cover service discovery.
* This DEP does not describe subscribable services, such as access to a dashboard for a fixed time period.
* This DEP does not describe details of installation of the service and/or its dependencies. 

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

It may be observed that these services:

- Enable creation of dataset(s)
- Accept input dataset(s) and transform it in some fashion
- May require remote acquisition of input dataset 

The Invoke API 

- Provides Ocean users tools to transform data assets registered on the Ocean network.
- Facilitates a workflow pipeline of data asset transformations.
- Enables provenance tracking by Ocean provenance aware algorithms.

<a name="specification"></a>

## Roles

- Asset/algorithm owner: The owner of the algorithm, 
  - May be registered as an Ocean asset
  - May be available as a deployable package (e.g. a docker image or a jar on a maven repo)
- Service provider: The entity that runs the algorithm on their server(s).
- Service consumer: The entity that invokes the service.
- Agent: The software entity that enables Service Instance interactions  with the rest of the Ocean community.
  - Agent can be of many types, such as local or remote, and communicate via different interfaces.
  - The rest of this document assumes a remote Agent that communicates over REST.

### Provider flow


![Provider flow ](https://user-images.githubusercontent.com/89076/55699303-d1eb4c00-59fc-11e9-9c93-59939e15283d.png)

### Consumer flow
change name to invokable rest agent
![Consumer flow ](https://user-images.githubusercontent.com/89076/55699307-d6176980-59fc-11e9-9227-03a0d661bc57.png)

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

The **Service Metadata** information should be managed using an API on the Ocean Agent. 
This API should expose the following capabilities in the Ocean Agent via HTTP REST.

The service provider makes the service available in two temporal phases:

### Service registration

The service provider registers the service and provides the spec for the Service Definition. This information describes 

- The endpoints at which the service(s) are available
- List of mandatory and optional arguments and their types, accepted by the operations 
- Sync/async nature of invoke methods
- Data returned by methods.

### Service delivery

The Service Delivery phase consists of 

- The consumer calling the invoke operation
- The service executing the operation. These could be jobs that take a significant amount of time to complete.
- The consumer retrieving the result(s) of the operations(such as data payloads, created assets and/or associated proofs), or getting a notification (e.g. via a webhook).

The service spec consists of 2 parts, Service definition and Service Invocation. 

## Service definition

### Inputs/outputs

The service definition includes:
- The endpoint where the service is available
- The list of supported operations
- The inputs and output params for each operation

Each operation must accept 
- One or more inputs
  - Each of which must have a name and type*, 
- Return one or more outputs 
  - Each of which must have a name and type.

The **types** of inputs/outputs accepted are:

- **asset**: These are registered Ocean assets identified by the asset DID
- **string**: string formatted data passed to the invocation. 

The value supported by each input are the following:

- **asset** type is represented by a map with 2 keys:
  - did: the asset DID 
  - purchasetoken: a token indicating proof of purchase. This field is optional.
  
Example of **asset** input type
```json
{"myasset": { "did": "26cb1a92e8a6b52e47e6e13d04221e9b005f70019e21c4586dad3810d46220135",
               "purchasetoken": "98e41a92e8a6b52e47e6e13d04221e9b005f70019e21c4586dad3810d46220136"}}
```

- **string** : the value is a string

Example of **string** asset type
```json
{"myasset": "mystringasset"}
```

## Service Delivery

The Invoke service implementation must host the following APIs.

- Invoke Operation (invoke the operation)
- Invoke Async Operation (invoke the operation asynchronously)
- Get job status (get the status of an invoked job, and the result if completed)

Unless mentioned otherwise, all requests, response and error payloads must be in JSON.

### Invoke a job asynchronously

This is the primary interface by which a consumer can invoke a service/run a job.

#### Request

- A POST request to the https://service-endpoint/invokeasync/operation_did along with JSON formatted payload as described by the asset schema.

- The keys in the map are parameter names as specified in the get operation schema
- The values are one of 
  - A map (if the type is asset)
  - A string (if the type is a string)
  
Here's an example of an request that defines a single input asset of type asset.

- The single argument **to_hash** is the parameter name
- Since the type is **asset** (as declared in the schema), the value must be a map with the **did** (and other optional keys)

```json 
{
    "to_hash": {
             "did" : "did",
             "purchase_token" : "value_of_purchase_token" 
    }
}
```

#### Response

If the server accepts the request, the response must be a JSON encoded map with the **jobid** and the corresponding value.

```json 
{
    "jobid": "valueofjobid"
}
```

| response code | description                                                                    | JSON payload                              |
|---------------|--------------------------------------------------------------------------------|-------------------------------------------|
|           201 | job creation success                                                           | map with jobid key                        |
|           400 | bad request-not according to presribed format or invalid configuration options | map with error code and error description |
|           401 | not authorized (no authorization tokens provided)                              | map with error code and error description |
|           500 | error                                                                          | -                                         |
|           503 | service unavailable                                                            | -                                         |
|               |                                                                                |                                           |


### Invoke a job synchronously
This is an convenience interface by which a consumer can invoke a service/run a job. This is a synchronous request, and is expected to be used for job that finish quickly.

#### Request

- A POST request to the https://service-endpoint/invoke/operation_did along with JSON formatted payload as described by the asset schema.
- The payload is the same as the asynchronous invocation

#### Response

The response format is the same as returned by the get job result operation.


### Get the result of a job

#### Request

- an HTTP GET request to https://service-endpoint/jobs/result/jobid

#### Response

The response must contain a JSON payload.  It must return a map with the **status** key, the value of which can be one of "scheduled", "inprogress", "error".
If the job has completed, it must also contain a **result** map with key(s) as defined in the "output" schema.
 Each value in the map must be one of (as defined in the schema)

- A map (if type is asset )
- A string (if type is string )


| response code | description                                       | payload           |
|---------------|---------------------------------------------------|-------------------|
|           200 | job result                                        |                   |
|           400 | Bad request                                       | described below   |
|           401 | not authorized (no authorization tokens provided) | error description |

HTTP methods that return error can encode further information in the payload. The payload must have **errorcode** and **description** fields.

| error code | description                         |
|------------|-------------------------------------|
|       8001 | unknown job id                      |
|       8003 | service not paid for by consumer    |
|       8004 | asset id XX contents not accessible |
|            |                                     |

Example of a an operation that returns 1 asset 

```json
{ "status":"completed",
  "result": {"hashed_value": {
               "did" : "did",
               "purchase_token" : "value_of_purchase_token"}
             }
}

```

Note: this response section is underspecified. It needs to address

- registering the generated asset on behalf of the service consumer
- specifying the service agreement, purchase price, additional metadata. 


## Open questions

- Authentication and Authorization headers are not yet defined in this DEP. Refer to the that DEP.
- Assets generated by the invoke service needs to be purchased by the consumer in order to view them. The mechanics of this are yet to be defined.
- The details of purchase_token are yet to be defined. In the squid world, the purchase token is the service agreement id.
- Add a trust question

## License

This DEP is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 3 of the License, or (at your option) any later version.

This DEP is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program; if not, see http://www.gnu.org/licenses.
