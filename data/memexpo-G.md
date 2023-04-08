## GAS-1
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L31
Line 31: `isAuthorized` is used only once. If we delete the method and simplify line 31 (in auth() modifier) to `require(msg.sender == owner, "ds-auth-unauthorized");`, each method with auth modifier will consume about 68 less gas

## GAS-2
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L45
`DSMath` methods `add`, `sub`, `mul` have no reason to be used with solidity compiler of >0.8.0 version. Extra gas consumption for a lot of methods, no extra security

## GAS-3
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224
`bool locked` uses a single storage slot, so it is better to make it uint256 and change corresponding checks. Functions with `synchronized` modifier will consume 222 less gas.
Also if we change `locked` not from 0 to 1 and vice versa, but from for example 2 to 1 (magic numbers should be put into constants like LOCKED = 2, NOT_LOCKED = 1), we can use less gas either (but we shall init variable in initializer in the last case)

## GAS-4
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L288
mapping `offers` is public, so there is no need in getter `getOffer`

## GAS-5
result of `makerFee()` should be cashed as used twice. -100 gas in average case
two places:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L345
and
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L585

## GAS-6
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L460
-8 gas if we use msg.sender instead of _offer.recipient

## GAS-7
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L675
the contract is used only as proxy implementation, setting variable values here does nothing just extra gas while deploying implementation and confusing code

## GAS-8
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L965
getMinSell is a getter for already public _dust mapping. Better to make _dust private to consume less gas

