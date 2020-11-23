```
shortname: 8/ASSET-METADATA
name: Asset Metadata definition
type: Standard
status: Raw
editor: Mike Anderson <mike.anderson@dex.sg>
contributors: Aitor Argomaniz <aitor@oceanprotocol.com>, Kiran Karkera <kiran.karkera@dex.sg>,  Matthias Kretschmann <matthias@oceanprotocol.com>
```


Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Asset Metadata](#asset-metadata)
      * [Change Process](#change-process)
      * [Language](#language)
      * [Motivation](#motivation)
      * [Base attributes](#base-attributes)
        * [Additional Information](#additional-information)
        * [Links](#links)
      * [Data Asset attributes](#data-asset-attributes)
      * [Operation attributes](#invokable-service-attributes)
      * [Bundle attributes](#bundle-attributes)
      * [Example](#example)
      * [References](#references)



# Asset Metadata

Developing decentralised data ecosystems that are interoperable requires a standard for describing assets with metadata
in a format that is machine readable and provides necessary information of the operation of relevant protocols.

As such, each Asset in the data ecosystem (dataset, operation, etc.) has Asset Metadata associated with it. Asset Metadata is created by the asset publisher, and hashed to ensure integrity and ensure that the asset can be subsequently referenced as an immutable part of the provenance of other assets.

Assets without proper descriptive metadata can have poor visibility and discoverability, so it is generally in the publisher's interest to ensure good metadata is made available.


# Motivation

The main motivations of this DEP are:

* Establish a standard Asset Metadata format to allow interoperability between participants in the data ecosystem
* Specify the common attributes that must be included in Asset Metadata under certain circumstances
* Identify the recommended additional attributes that SHOULD be included in Asset Metadata to facilitate the ASSETS search
* Provide a examples structured Asset Metadata and additional links for reference

## Asset Types

Assets must be one of the following types:

Type            | Description
----------------|---------------
**dataset**     | A Data Asset which represents some form of content, e.g. a CSV file
**operation**   | An asset that represents a computational operation that may be invoked
**bundle**      | A composite Asset that may contain other Assets.


## Base attributes

The following attributes may be included as part of Asset Metadata for any Asset, and have
specific meanings which participants should interpret accordingly.


Attribute          |   Type          |   Required    | Description
-------------------|-----------------|---------------|----------------------
**name**           | String          | No            | Human-readable, descriptive name of the Asset. Should be relatively short, e.g. "UK Rainfall Averages 2018"
**type**           | String          | No            | Type of the Asset. Allowed values are: "dataset", "operation", "bundle" according to the Asset Types defined above. If no type is specified, it is assumed to be "dataset"
**description**    | String          | No            | Details of what the resource is. For a data set this might explain what the data represents and what it can be used for
**dateCreated**    | String (Date)   | No            | The timestamp at which the asset was created with ISO 8601 String format e.g. "2019-08-13T06:05:27+00:00"
**creator**        | String          | No            | Free text name of the entity generating this data (e.g. Tfl, Disney Corp, etc.)
**license**        | String          | No            | Short name referencing to the license of the asset (e.g. Public Domain, CC-0, CC-BY, No License Specified, etc. ).
**copyrightHolder**| String          | No            | The party holding the legal copyright.
**links**          | Array of Link   | No            | Mapping of links for data samples, or links to find out more information.
**inLanguage**     | String          | No            | The language used in this instance of Asset Metadata. Please use one of the language codes from the [IETF BCP 47 standard](https://tools.ietf.org/html/bcp47). Assumed to be "en" if not specified.
**tags**           | Array of String | No            | Keywords or tags used to describe this content. Multiple entries in a keywords list are typically delimited by commas. Empty by default
**additionalInformation** | JSON Map | No            | Additional aritrary JSON content at discretion of publisher

### Additional Information

Additional attributes are totally free to add and can be defined by the publisher.

These are examples of attributes that might help describe or enhance the discoverability of a resource:

| Attribute         | Description                                                                                                                  |
| -                 | -                                                                                                                            |
| sla               | Service Level Agreement                                                                                                      |
| industry          |                                                                                                                              |
| category          | can be assigned to a category in addition to having tags                                                                     |
| updateFrequency   | how often are updates expected (seldome, annual, quarterly, etc.), or is the resource static (never expected to get updated) |
| termsOfService    |                                                                                                                              |
| privacy           |                                                                                                                              |
| keyword           | A list of keywords/tags describing a dataset                                                                                 |
| structured-markup | A link to machine readable structured markup (such as ttl/json-ld/rdf) describing the dataset                                |                                                                                                                  |

Additional information is opaque to the DEP Standards, and has no specific meaning from a protocol perspective. Solutions
intended to work with arbitrary Assets should make no assumptions about the content or existence of additional
information in the Asset Metadata.

Some implementations may make use of this information, e.g. service providers might define their own standards
for data formats here, which could affect processing in some circumstances. Service providers making this choice
should be aware that their implementation may not be compatible with all valid Assets as a result.

### Links

An array of Links can be provided to give supplementary information about an Asset, e.g.

```json
[
	{
		"name" : "Sample of Asset Data",
		"type" : "sample",
		"url": "https://foo.com/sample.csv"
	}
	{
		"name" : "Data Format Definition",
		"type" : "format",
		"assetID: "4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea"
	}
]
```

Links may be to either a URL or another Asset. We expect tribes and/or marketplaces to converge
on agreements of typical formats for linked data: The Ocean protocol itself does not mandate any
specific formats as requirements are likely to be domain-specific.

## Data Asset attributes

In addition to the base attributes, the following Attributes are defined for Data Assets (with type: "dataset")

Attribute       |   Type        |   Required    | Description
----------------|---------------|---------------|----------------------
**size**        | String        | No            | Exact size of the asset in bytes, encoded as a decimal String
**contentType** | String        | No            | File format if applicable, as a MIME type
**encoding**    | String        | No            | File encoding if relevant to content type (e.g. UTF-8)
**compression** | String        | No            | File compression (e.g. gzip, bzip2, etc). Assumed to be uncompressed if not included
**contentHash** | Text          | No            | SHA3-256 hash of asset data. Required if publisher wishes to offer integrity checks

## Operation attributes

Attribute       |   Type        |   Required    | Description
----------------|---------------|---------------|----------------------
**operation**   | JSON Map      | Yes           | Operation descriptor map as defined below
**contentHash** | Text          | No            | SHA3-256 hash of operation data. Required if publisher wishes to offer integrity checks


### Operation descriptor map

This section must be included whenever the Asset Type is **operation** and the value is a map
containing a definition of the **operation**, which contains the following attributes:

| Attribute   | Value Type | Required | Description                                |
|-------------|------------|----------|--------------------------------------------|
| **class**   | string     | Optional | Specifies if the operation has a special execution class. Allowable values are `orchestration` or `unknown` |
| **modes**   | list       | Optional | A list with values "sync" and/or "async". If absent, it assumed that "async" is supported.   |
| **params**  | map        | Yes      | Inputs to this operation                   |
| **results** | map        | Yes      | Outputs generated by this operation        |

The values for **params** and **results** must be a map with the following attributes:

- Keys are the parameter names
- Value against the parameter name must be a map. This map must contain

| Value        | Value Type | Required | Description                                                                       | Used in         |
|--------------|------------|----------|-----------------------------------------------------------------------------------|-----------------|
| **type**     | list       | Yes      | Must be one of **asset** or **json**                                            | params, results |
| **position** | int        | Optional | The value is an integer indicating the argument index. Used by libraries that support positional parameters. If absent, parameter names are required for each argument     | params          |
| **required** | boolean    | Optional | Indicates that this parameter is mandatory. If absent, default value is True | params          |


Example of `params` for an operation that hashes the input data asset, it accepts one input where:
- parameter name for the value to be hashed is `toHash`
- param value is of type asset, which required the DID of the asset to be passed.

```json
{
    "params" : {"toHash":{"type": "asset",
                           "position":0,
                           "required":true}}
}
```

Example of `params` for an operation that hashes the input payload. It accepts two inputs:
- parameter name for the value to be hashed is `toHash`
  - param value is of type `json`.
- parameter name for the hashing algorithm is `algorithm`
  - param value is type `json`

```json
{
    "params" : {"toHash": {"type": "json"},
                "algorithm": {"type": "json"}}
}
```

Example of `results` for an operation that generates predictions. It returns two outputs:
- parameter name for the predicted class is `prediction`
  - param value is of type `json`.
- parameter name for the confidence in the prediction is `confidenceLevel`
  - param value is type `json`

```json
{
    "results" : {"prediction": {"type": "json"},
                "confidenceLevel": {"type": "json"}}
}
```

Example of a complete section of operation metadata:
```json
{
  "name": "hashing",
  "description":"hashes the input",
  "type":"operation",
  "operation":{
    "class":"unknown",
    "modes":["sync","async"],
    "params" : {"toHash":{"type": "asset",
                           "position":0,
                           "required":true}},
    "results":{"hashValue": {"type": "asset"}}
  },
}
```

## Bundle attributes

In addition to the base attributes, the following Attributes are defined for bundles of assets only (with type: "bundle")

Attribute       |   Type        |   Required    | Description
----------------|---------------|---------------|----------------------
**contents**    | Map of String -> Content          | Yes           | A list of contents of this bundle

Content records are specified as a JSON map with the following attributes

Attribute      |   Type        |   Required    | Description
---------------|---------------|---------------|----------------------
**assetID**    | String (64)   | Yes           | A hex string containing the Asset ID of the content


Example:

```json
{
  "name": "Pyrotech firework safety data",
  "type": "bundle",
  "contents": {
  	"test":   {"assetID": "26cb1a92e8a6b52e47e6e13d04221e9b005f70019e21c4586dad3810d46220135"}
  	"train":  {"assetID": "503a7b959f91ac691a0881ee724635427ea5f3862aa105040e30a0fee50cc1a00"}
  	"verify": {"assetID": "e2f910c3f44126323ace27daf9a6e18ab0cbcc3ab9fa74a7c9462d7e8247f8811"}
  }
}
```



## Example

Here is a an example Asset using the schema described:

```json
{
    "name": "UK Weather information 2011",
    "type": "dataset",
    "description": "Weather information of UK including temperature and humidity",
    "size": "3.1gb",
    "dateCreated": "2012-02-01T10:55:11+00:00",
    "author": "Met Office",
    "license": "CC-BY",
    "copyrightHolder": "Met Office",
    "encoding": "UTF-8",
    "compression": "zip",
    "contentType": "text/csv",
    "workExample": "stationId,latitude,longitude,datetime,temperature,humidity\n
                        423432fsd,51.509865,-0.118092,2011-01-01T10:55:11+00:00,7.2,68",
    "contentUrls": ["https://testocnfiles.blob.core.windows.net/testfiles/testzkp.zip"],
    "links": [
	    {
		    "name" : "Sample of Asset Data",
		    "type" : "sample",
		    "url": "https://foo.com/sample.csv"
	    }
	    {
		    "name" : "Data Format Definition",
		    "type" : "format",
		    "assetID: "4d517500da0acb0d65a716f61330969334630363ce4a6a9d39691026ac7908ea"
	    }
    ],
    "inLanguage": "en",
    "tags": ["weather", "uk", "2011", "temperature", "humidity"],

    "additionalInformation" : {
        "updateFrecuency": "yearly",
        "structuredMarkup": [
            {
                "uri": "http://skos.um.es/unescothes/C01194/jsonld",
                "mediaType": "application/ld+json"
            },
            {
                "uri": "http://skos.um.es/unescothes/C01194/turtle",
                "mediaType": "text/turtle"
            }
        ]
    }
}
```


## References

[Schema.org](https://schema.org/) is a collaborative, community activity with a mission to create, maintain, and promote schemas for structured data on the Internet,
Data types use the [Schema.org primitive data types](https://schema.org/DataType).

The Asset Metadata is based in part on the Dataset scheme from schema.org [DataSet schema](https://schema.org/Dataset).

Schemas:

* DataSet - https://schema.org/Dataset
* FileSize - https://schema.org/fileSize
* Common license types for datasets - https://help.data.world/hc/en-us/articles/115006114287-Common-license-types-for-datasets

## License

Copyright (c) 2018 OceanProtocol Foundation.
Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
