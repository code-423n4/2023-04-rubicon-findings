# [L-1] User can bypass the _leverageCheck and open a long position with 1x leverage

The contract uses a boolean variable `_long` to identify whether a user opens a long or a short position. If it is true, the user opens a long position; if it is false, the user opens a short position. However, there is a way for a user to open a long position with 1x leverage, which is not allowed by the design of the protocol. The user can do this by calling `sellAllAmountWithLeverage` and swapping the `asset` and `quote` arguments. This will set the `_long` argument to `false` and pass the `_leverageCheck` even though the user is effectively opening a long position with 1x leverage.

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/utilities/poolsUtility/Position.sol#L586-L599
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/utilities/poolsUtility/Position.sol#L107-L118

## Recommended Mitigation Steps

This exploit violates the design of the protocol and may have unintended consequences for the market dynamics and the user experience. However, it does not cause any loss of funds for the protocol or the users. The protocol team could consider removing the `_leverageCheck` or adding additional checks to prevent this exploit.

# [L-2] Critical Address Changes Should Use Two-step Procedure

The function setOwner() transfer ownership to a new address. In case a wrong address is supplied ownership is inaccessible.

Instance:

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L25

## Recommended Mitigation Steps

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two-step procedure on the critical functions.

# [L-3] Emit events in important operations

Instance:

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L1476-L1480
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L1471-L1474
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L1466-L1469

## Recommended Mitigation Steps

Add events to above critical functions.

# [L-4] Relying on setters for initialisation of critical parameters is risky

Critical contract parameters should have a fail-safe default value enforced during declaration or in constructor/initialize functions without relying on admin-controlled setters to be called. Missed timely calling of such setters may result in type-dependent default values which could be dangerous because default value may be unexpected for the protocol.

In this case `rewardsDuration` parameter has no default value.

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/periphery/BathBuddy.sol#L232-L242


## Recommended Mitigation Steps

Add default value when initilizing the variable.

# [L-5] Low test coverage

Test coverage is 60% which is low. Only the main path is coverage, not the edge cases.

        - What is the overall line coverage percentage provided by your tests?:  60%

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/README.md#L173

Tool used: 

npx hardhat coverage
https://hardhat.org/plugins/solidity-coverage.html

## Recommended Mitigation Steps

Ensure the coverage report produced via `npx hardhat coverage` covers all functions within the smart contract suite.

# [N-1] Dead code and comments

To remove dead code and comments from code base to make code clean.

Example instances:

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/periphery/BathBuddy.sol#L94-L102
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L297-L310
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L99-L116
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L99-L116
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L163-L172
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L185-L195
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L464-L472

# [N-2] Add parameter to Event-Emit

The event `emitFee` should have an additional parameter called recipient, because the fee can be received by either the owner or the recipient. If the owner = address(0), the recipient address is lost in the event.

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L365-L372

# [N-3] Unneccesary variable convert to uint128

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L519-L520
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L559-L560
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L321-L322
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L405-L406
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L480-L481

# [N-4] Constants may be used

Change 10000 to constant `BASIS_POINTS`

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L583
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L586

# [N-5] Remove unused functions and modifier related to close and stop

The contract does not have any logic related to closing or stopping. The function `isClosed` always returns `false` and does not affect any other function. Therefore, it should be removed.

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L620-L622
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L597-L600

Similarly, the state variable `stopped` and the function `stop()` have no effect on the contract’s behavior and should be deleted.

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L594
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L628-L630

# [N-6] Simplify code by removing matchingEnabled and buyEnable

The state variable `matchingEnabled` and `buyEnable` always true and cannot be modified by any function. Therefore, they are unnecessary and can be removed. The condition checks related to `matchingEnabled` can also be deleted to simplify the code. Each unnecessary condition in code can lead to more cases and bugs

`matchingEnabled`: 
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L863
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L875
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L676

`buyEnable`:
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L715
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L1184
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L715

# [N-7] Lack of Input Validation

The parameters that are arrays should have the same length, but this is not checked.

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L917-L933

# [N-8] Remove contract note

This contract has the sole purpose of emitting events dynamically. However, it is only used in one location. Therefore, it should be removed and the event should be emitted directly.

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L633-L658
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L958

# [N-9] msg.sender cannot be address(0)

The require statement is redundant and can be deleted. `msg.sender` cannot be address(0)

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/periphery/BathBuddy.sol#L97

# [N-10] Move borrowBalance function to borrow section instead of view section

Function `borrowBalance` in Ctoken contract is not a view function. It computes the borrow balance with accrued interest. Therefore, it should be placed in the MAIN section of the contract.

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/utilities/poolsUtility/Position.sol#L65-L69

# [N-11] Remove unused paramters in the function

Parameter `_asset` in function `_borrowLimit`

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/utilities/poolsUtility/Position.sol#L528

# [N-12] Delete unused code for unsorted order book.

The unsorted order book is incomplete and unnecessary. The code does not include any logic to create and update the `head` and `near` variables. Therefore, we should delete all the code that refers to them.

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L695-L696
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L1438-L1464

# [N-13] Refactor for clear cases:

Having too many cases in code can make it hard to read, understand, and maintain. It can also increase the risk of errors and bugs. Refactoring can help simplify the logic, eliminate unnecessary cases, and improve the code quality.

```diff
-            if (vars.initAssetBalance == 0) {
-               vars.currentAssetBalance = assetBalance;
-            } else if (vars.initAssetBalance < assetBalance) {
-                vars.currentAssetBalance = assetBalance.sub(
-                    vars.initAssetBalance
-                );
+           if (vars.initAssetBalance <= assetBalance) {
+               vars.currentAssetBalance = assetBalance.sub(
+                    vars.initAssetBalance
+                );
            } else {
                vars.currentAssetBalance = vars.initAssetBalance.sub(
                    assetBalance
                );
            }
```
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/utilities/poolsUtility/Position.sol#L163-L173

# [N-14] Float pragma

Yieldly.Finance contracts use the floating pragma ^0.8.0. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using another pragma, for example, either an outdated pragma version that might introduce bugs that affect the contract system negatively or a recently released pragma version which has not been extensively tested.

Instance:

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L2

# [N-15] Missing natspec

Instances:

https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L487-L489
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L491-L507
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L564-L566
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L733-L758
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L780-L799
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L824-L852
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L1028-L1067
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L1069-L1112
https://github.com/code-423n4/2023-04-rubicon/blob/9ca03938f21aa8c5f439bc360d68cb9516d893af/contracts/RubiconMarket.sol#L1466-L1480