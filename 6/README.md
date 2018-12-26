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
   * [Service Invocation](#service-invocation)
      * [Change Process](#change-process)
      * [Language](#language)
      * [Motivation](#motivation)
      * [Application Abstraction](#application-abstraction)
      * [Constraints](#constraints)
      * [Options for a host platformOpti](#options-for-a-host-platform)
      * [Suggested Choice](#suggested-choice)
      * [Specification](#specification)

      
<!--te-->

<a name="service-invocation"></a>
# Service Invocation

The Service Invocation API (**INVOKE**) is a specification for the Ocean Protocol to register and invoke compute jobs.

INVOKE offers a general purpose computational interface

Compute services are defined as services available on the Ocean Network that

* May accept one or more Input parameters (which will typically be data assets to be used or algorithms to be run)
* Typically produce one or more Outputs (which will typically be references to generated data assets)
* Support the provision of proofs by service providers upon service completion (after which tokens in escrow may be released) 

# Exclusions

* This OEP does not prescribe the exact type of compute services offered. It is open to service provider implementations to define them, providing that they conform with this API specification
* This OEP does not cover service discovery.
* The OEP is not intended to apply to services where invocation / access is off-chain (e.g. high volume APIs or queue services)
* This OEP does not describe subscribable services, such as access to a dashboard for a fixed time period.
* This OEP does not describe installation of the service and/or its dependencies.

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

Asset/algorithm owner: The owner of the algorithm, which is made available as a package (e.g. a docker image or a jar on a maven repo)
Service provider: The entity that runs the algorithm on their server(s).
Service consumer: The entity that invokes the service.


![User flow ](./imgs/InvokeService.png)

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
  
## Specification 

The **Service Metadata** information should be managed using an API on the Ocean Agent. 
This API should expose the following capabilities in the Ocean Agent via HTTP REST.

The service provider makes the service available in two temporal phases:

### Service registration

The service provider registers the service and provides the spec for the Service Definition. This information describes 

- the endpoints at which the service is available
- the configuration options accepted by the invoke method(s)
- list of mandatory and optional arguments and their types, accepted by the invoke methods
- sync/async nature of invoke methods
- data returned by methods.

### Service delivery

The Service Delivery phase consists of 

- the consumer calling the invoke method(s) 
- the service executing the invoke methods. These could be jobs that take a significant amount of time to complete.
- the consumer retrieving the result(s) of the invoke method(such as data payloads, created assets and/or associated proofs), or getting a notification (e.g. via a webhook).

The service spec consists of 2 parts, Service definition and Service Invocation. The service provider publishes the service defintion 

## Service definition

```json 
{
  "inputs" : [ 
              {
             "argname" :"inputasset1",
             "type": "oceanasset",
             "mandatory" : "true",
             }
             ]},
  "serviceinfo" : {
      "consumerid" : "consumerid", 
      "invokeserviceagreementid" : "in_said"  },
  "configuration" : {
      "job" : "options"
  }
}
```

### Registering a new Service

Registering a service 

* is the same process as registering a new asset with an Ocean Agent.
* uses the same metadata as described in OEP8. In addition to the [regular metadata](https://github.com/oceanprotocol/OEPs/tree/master/8#base-attributes) it must specify the following:

```json
{ "other" : "metadata",
  "type"  : "algorithm",
  "links" : [ {"name" : "My algorithm",
               "url" : "https://github.com/my-algorithm/metadata"}]
}
```

The metadata in the url specifies configuration required to fire an invoke job.
Example configuration

```json
{ "oceaninputs" : ["inputasset1"]}
```

- Each input asset to be consumed must be named. On invoke, each named inputasset needs a map containing the asset id, asset url and other arguments (described in the invoke section)
- Non-Ocean inputs such as URL-accessed payloads and configuration options can also be specified

### Retire a Service

Retiring a service uses the same method as retiring an asset

### Invoke a job

#### Request

- a POST request to the https://service-endpoint/jobs , along with the following JSON formatted payload

```json 
{
  "inputs" : [ 
              {
             "argname" :"inputasset1",
             "type": "oceanasset",
             "mandatory" : "true",
             "assetid" : "ocnassetid",
             "asseturl" : "url to consume the asset ",
             "serviceagreementid" : "sa_id",
             }
             ],
             "consumerid" : "consumerid",
             "invokeserviceagreementid" : "in_said" 
  },
  "configuration" : {
      "job" : "options"
  }
}
```

#### Arguments

The value against the *inputs* key is a  list of maps. Each map defines a single input. each key is the input argument name, and the value is a map.

| param              | description                                 | Mandatory? |
|--------------------|---------------------------------------------|------------|
| assetid            | is the id of the asset on the Ocean network | yes        |
| asseturl           | the URL where the asset is consumed from    | yes        |
| serviceagreementid | ID of the service agreement                 | no         |
| assetmetadataurl   | URL where the asset metadata is hosted      | no         |

- Each Ocean input asset must have the mandatory arguments. It can also have optional arguments.
- Each invocation can have any number of input assets. The payload needs to contain a map where the keys are parameter names (as defined in the service metadata).

Non-data asset inputs:

| param                  | description                                         | Mandatory? |
|------------------------|-----------------------------------------------------|------------|
| consumerid             | user id of the consumer                             | no         |
| invokeserviceagreementid | service agreement of the (purchased) invoke service | no         |


#### Response

| response code | description          | payload |
|---------------|----------------------|---------|
|           201 | job creation success | jobid   |
|           500 | error                |         |

### Describe the status of the job

#### Request

- an HTTP GET request to https://service-endpoint/jobs/status/jobid

#### Arguments

The arguments are to be passed as HTTP request parameters

| argument   | description                       |
| --      |  --                               |
| consumerid | The consumer who invoked this job |
|            |                                   |

#### Response

| response code | description                                                | payload                   |
|---------------|------------------------------------------------------------|---------------------------|
|            200 | job status, one of: started, in progress, completed, error | {"status" : "inprogress"} |
|           500 | error                                                      |                           |

### Get the result of a job

#### Request

- an HTTP GET request to https://service-endpoint/jobs/result/jobid

#### Arguments

The arguments are to be passed as HTTP request parameters

| argument   | description                       |
| --|--                                         |
| consumerid | The consumer who invoked this job |
|            |                                   |

#### Response

| response code | description                                                | 
|---------------|------------------------------------------------------------|
|            200 | job result, a json formatted string| 
|           500 | error                                                      |

The json response is of the form

```json
{ "oceanoutputs" : [ "generatedassetid1", "generatedassetid2"]}

```

It can contain other keys such as non-Ocean payloads.
Note: this response section is underspecified. It needs to handle

- registering the generated asset on behalf of the service consumer
- specifying the service agreement, purchase price, additional metadata. 

### FAQ

- Can the API accept configuration options:
  - Yes the payload can contain any other inputs in the json object, other than ocean inputs
  
  
## License

This OEP is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 3 of the License, or (at your option) any later version.

This OEP is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program; if not, see http://www.gnu.org/licenses.

































