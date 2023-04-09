## Remove unused imports
BathHouseV2.sol - [8](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L8)

V2Migrator.sol - [4](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L4)

FeeWrapper.sol - [5](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L5)

## Missing array length checks
BathHouseV2.sol - [115](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L115-L128)

V2Migrator.sol - [30](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30-L35)

RubiconMarket.sol - [917](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917-L933)

## No need of `else` if there is a return statement above
FeeWrapper.sol - [53](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L53)

RubiconMarket.sol - [38](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L38), [1236](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1236), [1251](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1251)

## Function is missing a reentrancy guard(it makes a call to an external contracts)
FeeWrapper.sol - [48](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L48)

## use safeTransfer/safeTransferFrom
FeeWrapper.sol - [100](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L100), [102](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L102)

RubiconMarket.sol - [340](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L340), [351](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L351), [360](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L360), [375](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L375), [380](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L380), [460](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L460), [461](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L461), [538](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L538)

## Use the latest solidity version with a stable pragma statement
[BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2), [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2)

## Modifiers shouldn’t be used to update contract state
BathBuddy.sol - [247](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L247-L255)

## Unused function parameters
Position.sol - [528](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L528)

## It is best practice to use two-step ownership transfer
RubiconMarket.sol - [25](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25-L28) 

## `can_offer` modifier is empty
RubiconMarket.sol - [260](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260-L262)

## Confusing revert strings
RubiconMarket.sol - [321](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L321), [322](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L322) 

## Storage variables are re-initialized in constructor
RubiconMarket.sol - [714](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L714), [715](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L715)

## `require(!locked)` will always pass because the default value of `locked` is `false`, use reentrancy guard(`synchronized` modifier) instead
RubiconMarket.sol - [938](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L938), [1034](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1034), [1075](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1075)