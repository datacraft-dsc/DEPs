```
shortname: 13/ORCHESTRATION
name: Orchestration Operations
type: Standard
status: Raw
editor: Mike Anderson <mike.anderson@dex.sg>
contributors: 
```


Table of Contents
=================

   * [Table of Contents](#table-of-contents)


# Overview

This DEP describes a standard for creating Orchestrations, which are type of Operation that 
represents the combination of multiple child operations.

Orchestrations enable Digital Supply Lines to be defined and automated using a common, flexible 
standard. 

# Orchestration format

## Metadata

An orchestration is an asset of type `operation` and as such follows the general requirements for
operations as defined in DEP6 and DEP8.

The input and output parameters defined in the operation metadata automatically become input and 
output parameters for the orchestration.

The metadata must contain the additional attribute `"class":"orchestration"` as defined in DEP8 to
indicate that the operation is a valid orchestration.

## Content

The content of an orchestration asset must satisfy the following rules:
- The content of an orchestration must be a valid JSON string encoded in UTF-8 (MIME content type
`application/json`).
- The JSON content must represent a valid orchestration DAG, as defined below
- The JSON content may contain additional information in "custom" attributes, which may be useful
to client applications but must be ignored during the execution of the orchestration

Example:

```json
{
    "id":"parent",
	"children":[
		{
		  "id": "child1"
		  "did":"did:dex:9a4d904f195889aee3b12b867f764dbc06fa4a54030c44b6f805c70f85cb05e7/bd41ffa50baf8fddbfead91c426de45fcaa474773a9278d63bcaa10200591362"
		}
	],
	"edges":[
	   {
	     "id":"optional-edge-id",
	     "source":"parent",
	     "sourcePort":"operation-input-param", 
	     "target":"child1",
	     "targetPort":"child-input-param"
	   },
	   {
	     "source":"child1",
	     "sourcePort":"child-output-param",
	     "target":"parent",
	     "targetPort":"operation-output-param", 
	     "custom": {"custom-edge-attribute":"baz"}
	   }
	],
	"custom": {"custom-op-attribute":"foo"}
}
```

Notes:
- If the `"source"` or `"target"` nodes are omitted from edges, they are assumed to refer to the
overall orchestration (i.e. inputs or outputs of the overall parent operation) 

# Execution

An orchestration may be executed in the context of a given agent by invoking the orchestration in 
accordance with DEP6.

## Authorisation

The execution of the orchestration must be authorised by the orchestrating agent, as with any 
invokable operation.



## Failure conditions

An orchestration may fail for any of the reasons below:
- The orchestration DAG is not valid (e.g. contains a cycle)
- A child operation fails
- A required parameter is not provided
- The client is not authorised to execute the orchestration

## Orchestration results

Executing an orchestration will create and report operation results in accordance with DEP6. 

An orchestration shall be considered to have succeeded if:
- All immediate child operations have succeeded
- All required output parameters of the orchestration have been produced

In addition, the Job results map should contain nested results for child operations, so that
the execution status of the complete operation can be inspected.

Example of a successful Job result:

```json
{ 
  "status":"succeeded",
  "results": {"operation-output-param": "4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea"}
  "children": {
    "child1": {
       "status":"succeeded",
       "results": {"child-output-param": "foo"}
    }
  }
}
```

# License

Copyright (c) 2019-2020 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
