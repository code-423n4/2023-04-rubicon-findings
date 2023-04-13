# Gas Optimizations

## - Cheaper input validations should come before expensive operations

They can be moved to the beginning of the function:

```solidity
File: contracts/RubiconMarket.sol
322:         require(uint128(quantity) == quantity, "quantity is not an int");
```

```solidity
File: contracts/BathHouseV2.sol
72:         require(
73:             underlying != address(0),
74:             "createBathToken: UNDERLYING == ADDRESS 0"
75:         );
76:         require(
77:             implementation != address(0),
78:             "createBathToken: IMPLEMENTATION == ADDRESS 0"
79:         );
```

```solidity
File: contracts/utilities/FeeWrapper.sol
115:         require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");
```

## - Use `immutable` and `constant`
Use `immutable` if you want to assign a permanent value at construction. Use `constants` if you already know the permanent value.

The following variables can be `immutable` because they are set only once in the constructor and are never changed.
```solidity
File: contracts/utilities/poolsUtility/Position.sol
44:     Comptroller public comptroller;
45:     PriceOracle public oracle;
46:     RubiconMarket public rubiconMarket;
47:     BathHouseV2 public bathHouseV2;
```

## - Use `immutable` instead of `constant` for keccak variables
[The hash contained in the `constant` will be recalculated each time it is used.](https://github.com/ethereum/solidity/issues/9232)

```solidity
File: contracts/RubiconMarket.sol
232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```

## - `x += y` costs more gas than `x = x + y` for state variables
Same as `x -= y`.

```solidity
File: contracts/utilities/poolsUtility/Position.sol
431:         positions[_positionId].bathTokenAmount += _bathTokenAmount;
```

## - `internal` and `private` functions only called once can be inlined

The `_buys` function is only used once.
```solidity
File: contracts/RubiconMarket.sol
1183:     function _buys(uint256 id, uint256 amount) internal returns (bool) {
```
The `_matcho` function is only used once.
```solidity
File: contracts/RubiconMarket.sol
1273:     function _matcho(
```
The `_sort` function is only used once.
```solidity
File: contracts/RubiconMarket.sol
1362:     function _sort(
```
The `_bathify` function is only used once.
```solidity
File: contracts/BathHouseV2.sol
137:     function _bathify(
```

The `_rubicall` function is only used once.
```solidity
File: contracts/utilities/FeeWrapper.sol
60:     function _rubicall(
```

The `_rubicallPayable` function is only used once.
```solidity
File: contracts/utilities/FeeWrapper.sol
76:     function _rubicallPayable(
```
