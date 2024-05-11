---
author: Paul Berg (@PaulRBerg)
created: 2024-05-14
description: Standard API for a single Native Token
title: Single Native Token
---

## Abstract

The following standard allows for the implementation of a smart contract that manages a single Native Asset. The basic
functionality is provided, as well as on-chain metadata for other applications to use.

## Motivation

A standard interface for Native Tokens on Sablier allows external application to interact with the native token, whether
that be decentralized exchanges, trading facilities, wallets, or other such applications.

## Prior Art

The SRF-20 Single Native Token standard naming pays homage to the
[ERC-20 Token Standard](https://eips.ethereum.org/EIPS/eip-20) seen on Ethereum. While there is functionality we may use
as a reference, it is noted that Sablier's Native Tokens are fundamentally different from ERC-20 tokens. On Sablier, the
token balances are managed by the VM, whereas on Ethereum, they are managed by the token smart contract.

This standard has been inspired by Fuel's
[SRC-20: Native Asset](https://github.com/FuelLabs/sway-standards/blob/a001d3c/SRCs/src-20.md) standard.

## Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT
RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 and RFC 8174.

### Required Functions

The following functions MUST be implemented to follow the SRC-20 standard:

#### `decimals`

Returns the number of decimals used to get the user representation.

This function MUST always return 18.

```solidity
function decimals() external pure returns (uint8);
```

#### `ID`

This function MUST return the ID of the Native Token minted by the contract.

The ID MUST NOT change and SHOULD be generated using the default sub ID of 0.

```solidity
function ID() external pure returns (uint256);
```

#### `name`

This function MUST return the name of the asset, e.g., "Ether". The name MUST NOT change.

```solidity
function name() external view returns (string memory);
```

#### `symbol`

This function MUST return the symbol of the asset, e.g., "ETH". The symbol MUST NOT change.

```solidity
function symbol() external view returns (string memory);
```

#### `totalSupply`

This function MUST return the total supply of tokens in circulation.

```solidity
function totalSupply() external view returns (uint256);
```

### Recommended Errors

The following custom errors SHOULD be implemented:

#### `SRF20_InvalidHolder`

Indicates a failure with a holder address when tokens are burned.

```solidity
error SRF20_InvalidHolder(address holder);
```

#### `SRF20_InvalidRecipient`

Indicates a failure with a recipient address when tokens are minted.

```solidity
error SRF20_InvalidRecipient(address recipient);
```

## Rationale

As this standard leverages Native Tokens, we do not require transfer, balance, or approval functions. These functions
are performed within the SabVM, so we do not require the token contract to track and update balances. Accordingly, we
did not include any transfer or approval events.

Unlike Ethereum's ERC-20, this standard:

- Enforces 18 decimals. We consider that allowing different numbers of decimals provides little value while imposing
  significant technical debt.
- Enforces metadata. We remind implementation authors that the empty string is a valid response to `name` and `symbol`
  if you protest to this requirement. We also remind everyone that any smart contract can use the same name and symbol
  as _your_ contract. How users determine which SRF-20 smart contracts are canonical is outside the scope of this
  standard.

Unlike Fuel's SRC-20, this standard requires the smart contract to store the token's ID. We think that this will lead to
a better user experience since users will not have know the ID when interacting with the token.

The provided specification outlines only the functions and events necessary to implement fully functional Native Tokens
on Sablier. Additional functionality and properties may be added as needed.

## Backwards Compatibility

This standard is compatible with Sablier's Native Tokens. There are no other standards that require compatibility.

## Security Considerations

This standard does not introduce any security concerns, as it does not call external contracts, nor does it define any
mutations of the contract state.

## Example Implementation

TODO

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).
