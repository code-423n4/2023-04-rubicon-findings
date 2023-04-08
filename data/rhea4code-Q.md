# State Change In Modifier

## Vulnerability Detail

The main use case of modifiers is for automatically checking a condition, prior to executing a function. If the function does not meet the modifier requirement, an exception is thrown, and the function execution stops.

Its not advised to make state changes in the contract using the modifier, the best practice is to use the function itself for the state changes. In this case, the locked variable is being changed value. 
But the modifiers are only there for checks, so the state changes should be restricted to functions.

## Code Snippet

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269

## Tool used
VS Code for manual review.

