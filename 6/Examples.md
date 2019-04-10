# Examples

| Attributes      | Asset | Service |
| -               | -     | -       |
| Openrefine free | free  | free    |
| Openrefine paid | paid  | paid    |
|                 |       |         |




## OpenRefine free data exploration 

### Service Definition

Example of service definition for a service that creates an openrefine session for data exploration 
  
- the service takes a single argument ('csv') of type ocean asset
- the service does not return any data

```json 

```


### Service Invocation
Example of the invocation payload for the service described above

The consumer invokes the job by calling Starfish with the following payload 
```json
{
  "operation": "openrefine",
  "params" : {"csv" {"asset-did":"1234567890123456789012345678901234567890123456789012345678901234"}}}
```
## OpenRefine data exploration 

### Service Definition

Example of service definition for a service that creates an openrefine session for data exploration 
  
- the service takes a single argument ('csv') of type ocean asset
- the service and the asset have to be purchased prior to invocation
- the service does not return any data

```json 

```


### Service Invocation
Example of the invocation payload for the service described above

The consumer invokes the job by calling Starfish with the following payload 
```json
{
  "operation": "openrefine",
  "params" : {"csv" {"asset-did":"1234567890123456789012345678901234567890123456789012345678901234"
  "service-agreement-id":"1234567890123456789012345678901234567890123456789012345678901234",
  "consumerAddress" : "0x00a329c0648769A73afAc7F9381E08FB43dBEA72", 
  "serviceAgreementid" : "bb23s87856d59867503f80a690357406857698570b964ac8dcc9d86da4ada010",
  "serviceDefinitionId" : "df7escc856d59867503f80a690357406857698570b964ac8dcc9d86da4a832de",
  "signature" : "elided for clarity"},
  "openrefine", {"asset-did":"1234567890123456789012345678901234567890123456789012345678901234"
  "service-agreement-id":"1234567890123456789012345678901234567890123456789012345678901234",
  "consumerAddress" : "0x00a329c0648769A73afAc7F9381E08FB43dBEA72", 
  "serviceAgreementid" : "bb23s87856d59867503f80a690357406857698570b964ac8dcc9d86da4ada010",
  "serviceDefinitionId" : "df7escc856d59867503f80a690357406857698570b964ac8dcc9d86da4a832de",
  "signature" : "elided for clarity"}
  }}
```

## Model training

### Service Definition
1. Example of service definition for a service that trains a model.
  - the service provider provisions and installs the service
  - the service takes 2 datasets (training + test dataset) of type ocean asset
  - the service returns a single output (a trained model) of type ocean asset

```json 

```


### Service Invocation
Example of the invocation payload for the service described above

The consumer invokes the job by calling Starfish with the following payload 
```json
{
  "operation": "model_training",
  "params" : {"csv" {"asset-did":"1234567890123456789012345678901234567890123456789012345678901234", 
  "service-agreement-id":"1234567890123456789012345678901234567890123456789012345678901234",
  "consumerAddress" : "0x00a329c0648769A73afAc7F9381E08FB43dBEA72", 
  "serviceAgreementid" : "bb23s87856d59867503f80a690357406857698570b964ac8dcc9d86da4ada010",
  "serviceDefinitionId" : "df7escc856d59867503f80a690357406857698570b964ac8dcc9d86da4a832de",
  "signature" : "elided for clarity"}},
}
```

2. Example of service definition that runs [Blender](https://www.blender.org/) to render scenes, given a .blend file
  - the consumer installs the service. The example uses Docker, however the consumer can choose to install a different package (say, a Kubernetes helm chart), based on service provider's supported  containers.
  - The service takes a blend file as input
  - the service generates an Ocean asset (the generated video)as output.

```json 
{
 "servicedescription": {"name": "name of service",
                        "description" : " free form description of service  ",
                        "agenttype": "rest",
                        "endpoint": "https://blenderserviceprovider/blend",
                        "version" : "0.2"},
 "service_installation" : {"container_supported" : ["docker"]},
  "inputs" : [{
             "argname" :"blendfile",
             "type": "runcommand",
             "mandatory" : "true"}],
  "outputs": [{"argname" : "generated_video",
              "type": "oceanasset"}] 
}
```

Example of the invocation payload for the service described above

The consumer invokes the job by HTTP POSTing to "https://blenderserviceprovider/blend"
```json
{
 "service_installation" : {"package_type": "docker",
                           "docker_image": "https://hub.docker.com/r/ikester/blender-autobuild/tags/2.79"},
  "inputs" : [{
             "argname" :"blendfile",
             "exec" : "docker run --rm -v /source/path/:/media/ ikester/blender /media/blendfile.blend -o /media/frame_### -f 1"}],
  "slainfo" : {
      "consumerAddress" : "0x00a329c0648769A73afAc7F9381E08FB43dBEA72", 
      "serviceAgreementid" : "bb23s87856d59867503f80a690357406857698570b964ac8dcc9d86da4ada010",
      "serviceDefinitionId" : "df7escc856d59867503f80a690357406857698570b964ac8dcc9d86da4a832de",
      "signature" : "elided for clarity"
      },
}
```

