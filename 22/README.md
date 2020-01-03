```
shortname: 22/Resolver
name: Resolver functionality
type: Reference
status: Draft
contributors: Ilia Bukalov
```
# Resolver

The Resolver provides functionality to register DDO of given DID, to resolve DDO by DID and access rights control.

## Implementations

There are two supported implementations: first one for compatibility with Ocean Squid and second one from DEX. 
First one requires Aquarius service as storage of DDO while second one uses Ethereum network itself as storage. 
In both cases DID is registered on-chain.

## Interface

It implements the following interface:
```
String getDDOString(DID did) throws ResolverException;
void registerDID(DID did, String ddoString) throws ResolverException;
```

## Permission control

The records are allowed to read (resolve) for everyone while to register (to write) is allowed only by the owner, which is actually the blockchain account which registered DDO the first time.
This way it is possible to update DDO record by onwer/creator only. The permission is controlled by the blockchain itself.

## Config

Each resolver must have the following config:
```
# credentials configuration
account.main.address=<defines account of owner>
account.main.password=<defines account of owner>
account.main.credentialsFile=<defines account of owner>
# network configuration
contract.DIDRegistry.address=<defines address of resolver smart contract>
keeper.url=<defines the network within which to register/resolve>
```
Each Resolver **must** have account in blockchain (the first 3 parameters in the config) to attach its access rights to.

## Config variations
There are possible use cases when:
1. There are different resolvers which imply different blockchain account within one network. 
2. There are different resolver in the different networks.

All these cases are possible to reach using custom config described above.
