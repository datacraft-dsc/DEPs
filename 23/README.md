```
shortname: 23/Provenance verification
name: Provenance verification on chain. 
type: Reference
status: Draft
contributors: Ilia Bukalov
```
# Provenance verification

While full provenance information is stored in asset metadata as described in [DEP12](https://github.com/DEX-Company/DEPs/tree/master/12) some information is stored in distributed public ledger.
Since this ledger is immutable by its nature, this fact allows to verify provenance providing more trust for the end user.

## Records

There are 3 bound fields stored on ledger. Each record of provenance has all of them:
- __User__. Blockchain account.
- Timestamp (UTC time). Unix time since epoch
- Asset id.

## Registering interface

```
void registerAsset(String assetId) throws DexChainException
```

Each time when __User__ accesing, writing, creating or doing any other actions on an asset he should call this interface to update provenance records of this asset on the ledger.

## Searching interface

```
public ArrayList<DexProvenanceResult> getAssetProvenance(String assetId) throws DexChainException
public ArrayList<DexProvenanceResult> getAssetProvenance(String assetId, String user) throws DexChainException
public ArrayList<DexProvenanceResult> getAssetProvenance(String user)
```
So it is possible to get all provenances records related to __User__, related to asset or search/filter by both of them.

## Config

Similarly to [DEP22](https://github.com/DEX-Company/DEPs/tree/master/22) Provenance interface must be initialized by the following config:
```
# credentials configuration
account.main.address=<defines account of User>
account.main.password=<defines account of User>
account.main.credentialsFile=<defines account of User>
# network configuration
contract.Provenance.address=<defines address of provenance smart contract>
keeper.url=<defines the network within which to store provenance informaion>
```
Each Provenance interface **must** have an account in blockchain (the first 3 parameters in the config) to identify __User__ field in record for each transaction.

## Usage
```
DexProvenance provenance = DexProvenance.create();
String AssetId = "0xassetid";
provenance.registerAsset(assetId);
ArrayList<DexProvenance.DexProvenanceResult> results = provenance.getAssetProvenance(assetId);
```
