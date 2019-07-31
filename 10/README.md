```
shortname: 10/TOKEN-PAY
name: Direct Payments with ERC20 Tokens
type: Standard
status: Raw
editor: Ilya Bukalov <ilya.bukalov@dex.sg>
contributors: Mike Anderson <mike.anderson@dex.sg>
```

# Table of Contents

   * [Table of Contents](#table-of-contents)
   * [Overview](#overview
   * [Requirements](#requirements)
   * [Specification](#specification)
       * TBD
   * [Motivation](#motivation)
   * [References](#references)
   * [License](#license)


# Overview 

This DEP describes a payment system using arbitrary ERC20 Tokens, which allows data assets and services to be made available
for purchase using a direct token payment.

The direct payment mechanism, by definition does *not* support escrow or on-chain dispute resolution. Actors wishing to make
use of such features should use a different payment mechanism, for example the Service Execution Agreements available on the
Ocean Network.

Direct Payment is functionally equivalent to transferring tokens, so should only be used is situations where the consumer 
is willing to accept the risk that the publisher / service provider does not subsequently provide the asset or service. 


# Requirements

The Token payments system:

- Must allow purchases to be made using any ERC20 token
- Must allow purchases to be made on any Ethereum compatible network, including but not limited to the Ocean Network and the Ethereum Mainnet
- Must allow service providers to verify that a purchase has been successfully made
- Must allow purchases to be made that credit the publisher's account, without active participation by the publisher

# Specification

## Direct Payment Contract

The Direct Payment contract is a smart contract deployed on an Ethereum-compatible network that enables direct ERC20 token
payments for data assets and services

## Allowance API

The Allowance API enables a consumer to allow the direct payment contract to transfer tokens from the consumer's account.

## Purchase API

The Purchase API provides functionality for a client to make a direct payment for a data asset or service.

## Purchase Confirmation API

The Purchase Confirmation API enables a service provider to verify that a purchase has been made by a consumer for
a specific data asset or service.

Service providers should use this API to verify successful purchase before authorising consumer access to the protected
data asset or service.


# References

* ERC20 Standard as describe in EIP-20 - https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md

# License

Copyright (c) 2019 DEX Pte. Ltd.

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
