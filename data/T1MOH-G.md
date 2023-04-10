## 01. Don't use safe math functions like add, sub, mul, div. Solidity already has overflow checking by default.

## 02. Cash the result of makerFee() to save 100 gas from SLOAD warm access
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L345-L346
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L585-L586

## 03. Remove redundant require to save gas
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L63-L66

## 04. Add require check of array lengths to save user's gas on wrong inputs.
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L115

## 05. Pack all mapping values to one struct and pack variables
uint256 rewardRates, uint256 rewardsPerTokensStored, uint80 lastUpdateTime, uint80 periodFinish, uint80 rewardsDuration. It saves 2 slots and recalculation of keccak256
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L52-L56

## 06. Cash value of rewardsDuration[address(rewardsToken)] and rewardRates[address(rewardsToken)] to avoid multiple SLOAD.
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L191-L228

## 07. You can inline function rewardPerToken() to cash value of rewardsPerTokensStored[token] and save 100 gas on SLOAD
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L247-L257