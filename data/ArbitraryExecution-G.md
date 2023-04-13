# Gas Savings

## `bump` function is unnecessary

### Description

The [`bump`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L297) function does not make any non-local state changes nor does it return a value. As such, its inclusion in the contract does not provide any benefit.

### Recommendation

Consider removing the `bump` function.