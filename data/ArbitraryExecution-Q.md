# QA Report (low/non-critical)

## `MAKER_FEE_SLOT` has a known keccak256 preimage 

### Description 

The address of [`MAKER_FEE_SLOT`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L232) is computed from the keccak hash of the string "WOB_MAKER_FEE".

Since collision attacks require a knowledge of the preimage, this is considered less secure than choosing an address without a known preimage.

### Recommendation

Consider subtracting 1 from the keccak256 hash:

```solidity
bytes32 internal constant MAKER_FEE_SLOT = bytes32(uint256(keccak256("WOB_MAKER_FEE"))-1);
```

## Usage of `uint`

### Description
Throughout the `RubiconMarket.sol` file, there are instances where the `uint` data type is used instead of `uint256`.  Use of `uint256` instead of `uint` is more explicit.

### Recommendation
Consider changing all instances of `uint` to `uint256` for explicitness and consistency.

## `isClosed` always returns `false`

### Description
The [`isClosed`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620) function always returns `false`.  This function is used in a number of `require` statements throughout the contracts in `RubiconMarket.sol`:

```
/// @dev After close_time has been reached, no new offers are allowed.
modifier can_offer() override {
    require(!isClosed());
    _;
}
...
function isClosed() public pure returns (bool closed) {
    return false;
}
```

Since `isClosed` always returns `false`, its inclusion in these checks does not improve the security posture of the protocol.

### Recommendation
Consider removing the `isClosed` function.

## Use of floating compiler version pragma

### Description
The following contracts in this repository float their Solidity compiler versions (e.g. `pragma solidity >=0.8.4`).

- [`RuibconMarket.sol`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2)
- [`BathBuddy.sol`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2)

Locking the compiler version prevents accidentally deploying the contracts with a different version than what was used for testing. The current pragma prevents contracts from being deployed with an outdated compiler version, but still allows contracts to be deployed with newer compiler versions that may have higher risks of undiscovered bugs.

It is best practice to deploy contracts with the same compiler version that is used during testing and development.

### Recommendation
Consider locking the compiler pragma to the specific version of the Solidity compiler used during testing and development.

## Unhandled return values of `transferFrom`, `transfer`, and `approve`

### Description
The [`_chargeFee`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L93) function of the `FeeWrapper` contract ignores return values for the `transferFrom`, `transfer` and `approve` external calls.

ERC20 implementations are not always consistent and the calls to `transferFrom`, `transfer` and `approve` could return false instead of reverting.

### Recommendation

Consider checking the return value and revert on `0` or `false`.