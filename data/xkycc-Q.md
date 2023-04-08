### FINDING 1: Error calling `.add()` on `.wmul()`

In `_borrowLimit()` in `Position.sol`, `.add()` is attempted to be called on the result of a call to `.mul()`. This results in the following error:
```
TypeError: Member "add" not found or not visible after argument-dependent lookup in uint256.
  --> contracts/utilities/poolsUtility/Position.sol:548:74:
``` 

`add()` is defined as follows (in `RubiconMarket.sol`):
```solidity
function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
    require((z = x + y) >= x, "ds-math-add-overflow");
}
```

`wmul()` is defined as follows (in `RubiconMarket.sol`):
```solidity
function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
    z = add(mul(x, y), WAD / 2) / WAD;
}
```


Proposed new code:
```solidity
548: _loopBorrowed = add(wmul(_assetAmount, _collateralFactor), _max);
```

*Instances (1)*
```solidity
File: contracts/utilities/poolsUtility/Position.sol

548:  _loopBorrowed = wmul(_assetAmount, _collateralFactor).add(
549:       _max
550:  );
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol)