## [QA-01] can_offer does not do anything

`can_offer` is supposed to give admins the ability to pause and unpause trading however, this function always returns a `false` value via `isClosed()`. I recommend implementing some functionality to pause the contract with the OpenZeppelin `Pausable` contract. 

This was identified in the following location:

- [https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L598](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L598)
- [https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L621](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L621)