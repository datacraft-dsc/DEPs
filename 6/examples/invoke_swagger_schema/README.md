## Ocean invoke endpoint

This describes the process of creating an ocean invoke endpoint using OpenAPI

## Concepts:

- Endpoint : a URL where the invocable service has been made availble by the agent.

- Operation: the operation that be invoked at the endpoint. For the purpose of this discussion, lets assume that `ResNet model training` and `ResNet model prediction` are 2 operations. ResNet is a type of neural network used for image classification.

There are 2 ways of implementing this:

1. an endpoint provides a single operation. 

In this mode, each operation is available at a single endpoint. Therefore the 2 operations are available at

- http://resnetprovider.com/modeltraining
- http://resnetprovider.com/prediction

2. An endpoint provides multiple operations, and the invoke is routed using an 'operation' field in the payload. 
Example:

Endpoint : http://resnetprovider.com/invoke

HTTP POST payload (encoded as a JSON String) for training (note params are elided for clarity):
```json
{ "operation": "training",
  "params": {}}
```

 payload for prediction:
```json
{ "operation": "prediction",
  "params": {}}
```

The rest of this document uses the first approach, one endpoint per operation.

## TL;DR version

- Install [Swagger-codegen](https://github.com/swagger-api/swagger-codegen#development-in-docker). I've linked to the dockerized version, however, it can also be run locally.
- Create or modify a yaml file describing an Ocean invoke service. Here's [an example](https://github.com/DEX-Company/DEPs/blob/simplify_oep6/6/examples/invoke_swagger_schema/ocean.yaml) which takes 2 arguments
  - a string 
  - an ocean asset. 
- Choose the language you want to implement the service in. For example, if you choose to implement the service in python+flask, you could run

`./run-in-docker.sh generate -i path/to/ocean.yaml \
    -l python-flask -o /gen/out/ocean-invoke -DpackageName=oceaninvoke`

- The OpenAPI spec will generate a Flask App that conforms to the Ocean Invoke Spec.
- Head over to `oceaninvoke/controllers/invoke_controller.py` where you can implement your custom logic.

## What is the Ocean Invoke spec?

tl;dr version:

It is an API endpoint that supports at least one of the following the methods:

|Path | Payload| synchronous| HTTP method |
|-|-|-|-|
|/invokesync  | JSON encoded string with params for invoke | yes| POST|
|/invokeasync | JSON encoded string with params for invoke | no| POST |
|/getstatus | none |yes | GET |
|/getresult | none |yes | GET |


- Invoke sync returns synchronously and should be used for jobs that complete quickly (e.g. model prediction). 
- Its Async counterpart should be used for jobs that will take a longer period of time (e.g. model training), and should instead return a job id.


