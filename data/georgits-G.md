## Function state mutability can be restricted to pure
FeeWrapper.sol - [28](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L28)

## Function state mutability can be restricted to pure
RubiconMarket.sol - [297](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297) 

## Split require statement to save gas
BathBuddy.sol - [95](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L95-L99)

## Refactor to `return src == owner`
RubiconMarket.sol - [36-40](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L36-L40)

## it is more gas efficient to use `uint` instead of `bool` for reentrancy guard
RubiconMarket.sol - [264](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269) 

## emit events with function params to save gas
BathBuddy.sol - [241](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L241)
