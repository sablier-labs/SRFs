---
author: Paul Berg (@PaulRBerg)
created: 2024-05-11
description: Enables mint and burn functionality for fungible tokens
title: Mint and Burn
---

## Abstract

The following standard enables the minting and burning of Native Tokens for any fungible tokens. It seeks to define mint
and burn functions defined separately from other asset standards such as [SRF-20](./srf-20.md).

## Motivation

The intent of this standard is to provide a generic interface for minting and burning fungible tokens represented as
Native Tokens.

## Prior Art

This standard has been inspired by Fuel's
[SRC-3: Mint and Burn](https://github.com/FuelLabs/sway-standards/blob/a001d3c/SRCs/src-3.md) standard.

## Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT
RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 and RFC 8174.

### Required Functions

The following functions MUST be implemented to follow the SRF-2 standard:

#### `burn`

This function MUST burn `amount` tokens with the sub-identifier `subID` and MUST ensure the ID of the token is the
SHA-256 hash of `(address(this), subID)` for the implementing contract.

The `holder` MUST have at least `amount` tokens.

If the [SRF-20](./srf-20.md) standard is used, this function MUST update the total supply. This function MAY contain
arbitrary conditions for burning, and revert if those conditions are not met.

```solidity
function burn(uint256 subID, address holder, uint256 amount) external;
```

##### Arguments

- `subID` - The sub-identifier of the token to burn.
- `holder` - The address of the token holder to burn tokens from.
- `amount` - The quantity of tokens to burn.

#### `mint`

This function MUST mint `amount` tokens with sub-identifier `subID` and transfer them to the `recipient`. This function
MAY contain arbitrary conditions for minting, and revert if those conditions are not met.

```solidity
function mint(uint256 subID, address recipient, uint256 amount) external;
```

##### Arguments

- `subID` - The sub-identifier of the token to mint.
- `recipient` - The address to which the newly minted token is transferred to.
- `amount` - The quantity of tokens to mint.

## Rationale

This standard enables compatibility between applications and allows minting and burning native tokens per use case. This
standard has been separated from the [SRF-20](./srf-20.md) standard to allow for the minting and burning for all
fungible tokens, irrelevant of whether they are Native Tokens or not.

## Backwards Compatibility

This standard is compatible with Sablier's Native Tokens. There are no other standards that require compatibility.

## Example Implementation

TODO

## Security Considerations

The `mint` function should have checks to ensure that only authorized users can mint tokens.

correctly if the [SRF-20](./src-20.md) standard is used.

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).
