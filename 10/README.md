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
   * [Overview](#overview)
   * [Requirements](#requirements)
   * [Specification](#specification)
       * TBD
   * [Motivation](#motivation)
   * [References](#references)
   * [License](#license)


# Overview 

This DEP describes a payment system using ERC20 Tokens, which allows data assets and services 
to be made available for purchase using a direct token payment.

Direct Payment is functionally equivalent to transferring tokens, so should be used in situations where the consumer is willing to accept the risk that the publisher / service provider does not subsequently provide the asset or service. 


# Requirements

The Token payments system:

- Must allow purchases to be made using any ERC20 token
- Must allow purchases to be made on any Ethereum compatible network, including but not limited to the Ethereum Mainnet
- Must allow service providers to verify that a purchase has been successfully made
- Must allow purchases to be made that credit the publisher's account, without active participation by the publisher

# Specification

## Direct Payment Contract

The Direct Payment contract is a smart contract deployed on an Ethereum-compatible network that enables direct ERC20 token
payments for data assets and services. Direct Payment Contract can be created by anyone and deployed in any network but to be compatible with this specification it must generate the event with the following signature:
```
event TokenSent(
	address indexed _from,
	address indexed _to,
	uint256 _amount,
        bytes32 _reference1,
	bytes32 indexed _reference2
  );
```
Direct Payment contract is supposed to perform 3 functions:
1. To assign any (but only one) ERC20 token to perform transaction
2. To implement Allowance API by calling transferFrom function of ERC20 token. Purchaser must set Direct Payment Contract address as spender in approve() function beforehand to  start using this contract. 
3. To log edditional information of transaction

## Allowance API

The Allowance API enables a consumer to allow the direct payment contract to transfer tokens from the consumer's account.
There is no special requirements for implementation of this API since it is the part of existing ERC20 token interface:
```
transferFrom(address _from, address _to, uint256 _value) public returns (bool success)
approve(address _spender, uint256 _value) public returns (bool success)
allowance(address _owner, address _spender) public view returns (uint256 remaining)
```
Using these methods any ERC20 token can be allowed to be transferred by third party address (or Direct Payment Contract as a special case) to recipient with prior agreement of purchaser

## Purchase API

The Purchase API provides functionality for a client to make a direct payment for a data asset or service.
It has the following interface:
```
void sendTokenAndLog(address to, uint256 amount, bytes32 reference1, bytes32 reference2)
```
It is called by purchaser directly and this transaction is signed by purchaser as well and is sent to blockchain. This way additional information as _reference1_ and _reference2_ apart from token transaction (_amount_, _address of account_from_, _address of account_to_) is recorded to blockchain. _reference2_ must be unique for life because only this way it is possible clearly to distinguish transactions.

## Purchase Confirmation API

The Purchase Confirmation API enables a service provider to verify that a purchase has been made by a consumer for
a specific data asset or service.

Service providers should use this API to verify successful purchase before authorising consumer access to the protected
data asset or service.
It has the following interface:
```
bool check_is_paid(address_from, address_to, amount_paid, reference)
```
Having these 4 parameters it is possible to clearly conclude whether the payment has been done or not by searching in blockchain ledger.

# References

* ERC20 Standard as describe in EIP-20 - https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md

# License

Copyright (c) 2019-2020 DEX Pte. Ltd., Datacraft

This DEP is free software; you can redistribute it and/or modify it under the terms of the Apache 2.0 License
