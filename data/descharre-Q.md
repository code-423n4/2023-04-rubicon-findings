# Summary
## Low Risk
|ID     | Finding| Instances |
|:----: | :---           |   :----:         |
|1       |`buyEnabled` and `matchingEnabled` can't be set to false|1|
|2       |Account existence check for low-level calls|1|


## Non critical
|ID     | Finding| Instances |
|:----: | :---           |   :----:         |
|1       |Using safemath when compiler is ^0.8.0| 1 |
|2       |Use scientific notation (1E18) rather than exponentation (10**18)| 9 |
|3       |Remove empty modifiers| 1 |
|4       |Remove functions that have no purpose|1|
|5       |Unspecific compiler version pragma|1|
|6       |Owner should be set to msg.sender when offer is created|1|
|7       |Unused variables|1|
|8       |`batchRequote()` has no array length checks|1|
|9       |No error message on require statements|20|
|10      |Not using the named return variables in a function|3|
# Details
## Low Risk 
## [L-01] `buyEnabled` and `matchingEnabled` can't be set to false
The two state variables [`buyEnabled` and `matchingEnabled`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L675-L676) are being set to true during initalization and in the `initialize()` function. However there is no way to set these variables to false.

For `buyEnabled` this means that [`_buys()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1184) function will never revert on the first require statement. For `matchingEnabled` it means that in the function [`buy()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L863) `_buys` will always be used and the if statement in [`cancel()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L871-L883) will always be true.
## [L-02] Account existence check for low-level calls
Low-level calls call/delegatecall/staticcall return true even if the account called is non-existent (per EVM design). Account existence must be checked before the call.
- [FeeWrapper.sol#L82](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L82)

## Non critical
## [N-01] Using safemath when compiler is ^0.8.0
There is no need to use SafeMath when compiler is ^0.8.0 because it has built-in under/overflow checks. This applies to the BathBuddy and Position contract.

[`DSMath`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L46-L56) in the RubiconMarket.sol file has also some custom math functions for over/underflow. However this is also not necassary because solidity performs these checks under the hood.
## [N-02] Use scientific notation (1E18) rather than exponentation (10**18)
Scientific notation should be used for better code readability.
- [RubiconMarket.sol#L74-L75](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74-L75)
- [RubiconMarket.sol#L1057](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1057)
- [RubiconMarket.sol#L1059](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1059)
- [RubiconMarket.sol#L1099](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1099)
- [RubiconMarket.sol#L1101](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1101)
- [RubiconMarket.sol#L1175](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1175)
- [RubiconMarket.sol#L1177](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1177)
- [Position.sol#L317](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L317)
- [Position.sol#L331](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L331)
## [N-03] Remove empty modifiers
An empty modifier should be removed. Even when it's overriden in the sub contract
- [RubiconMarket.sol#L260-L262](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260-L262)
## [N-04] Remove functions that have no purpose
The function [`bump()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297-L310) just saves an id to a memory variable but doesn't even return it. Hereby the function does nothing and should be removed.
## [N-05] Unspecific compiler version pragma
Avoid floating pragmas for non-library contracts. Contracts should be deployed with the same compiler version and flags that they have been tested the most with. Locking the pragma helps ensure that contracts do not accidentally get deployed using, for example, the latest compiler which may have higher risks of undiscovered bugs. Contracts may also be deployed by others and the pragma indicates the compiler version intended by the original authors.
## [N-06] Owner should be set to msg.sender when offer is created
When someone creates a new offer, the owner is passed as a parameter. Instead the owner should be set to msg.sender to avoid the risk of setting a wrong owner.

[RubiconMarket.sol#L511-L562](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511-L562)
```diff
L533:
-    info.owner = owner;
+    info.owner = msg.sender;
```
## [N-07] Unused variables
[`stopped`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L594) can only be set to true in the `stop()` function. However stopped is nowhere else used which makes it useless. 
## [N-08] `batchRequote()` has no array length checks
The function [`batchRequote()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917-L933) does not check if all arrays have the same length. Unlike the [`batchOffer()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L887-L907) function which does have these checks.
## [N-09] No error message on require statements
When a require statement has no error message, it's difficult to know why a function reverted. For a better experience, require statements should always have error messages.
- [BathBuddy.sol#L88](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L88)
- [RubiconMarket.sol#L598](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L598)
- [RubiconMarket.sol#L604](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L604)
- [RubiconMarket.sol#L612](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L612)
- [RubiconMarket.sol#L702](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L702)
- [RubiconMarket.sol#L835)](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L835)
- [RubiconMarket.sol#L877](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L877)
- [RubiconMarket.sol#L938-L944](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L938-L944)
- [RubiconMarket.sol#L1034](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1034)
- [RubiconMarket.sol#L1075)](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1075)
- [RubiconMarket.sol#L1136](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1136)
- [RubiconMarket.sol#L1169](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1169)
- [RubiconMarket.sol#L1184](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1184)
- [RubiconMarket.sol#L1194](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1194)
- [RubiconMarket.sol#L1209](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1209)
- [RubiconMarket.sol#L1226](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1226)
- [RubiconMarket.sol#L1366](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1366)
- [RubiconMarket.sol#L1410](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1410)
- [RubiconMarket.sol#L1428](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1428)
- [RubiconMarket.sol#L1477](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1477)
## [N-10] Not using the named return variables in a function
function() returns(uint x) and not using x in the function

A named return variable should be used or otherwise removed:
- [RubiconMarket.sol#L276-L286](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L276-L286)
- [RubiconMarket.sol#L620-L622](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L620-L622)