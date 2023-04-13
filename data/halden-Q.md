## [L-01] Using SafeMath when compiler is ^0.8.0
If `rewardsDuration[token]` is equal to zero during the calculation of rewardRatio in `notifyRewardAmount(...)` will revert.
[233](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L233)
Mitigation steps:
Add additional check for that

```
require(_rewardsDuration != 0, "Invalid duration time");
```

##  [I-01] Using SafeMath when compiler is ^0.8.0
There is no need to use SafeMath when compiler is ^0.8.0 because it has built-in under/overflow checks.

##  [I-02] Unused storage variable
Storage variable `stopped` is unused and does not affect the contracts. It should be removed or be used for something else.

## [I-03] Code is not properly formatted
Run a formatter on the code, for example use the prettier-solidity plugin.

## [I-04] NatSpec missing from external functions
Add NatSpec docs for all external functions so their intentions and signatures are clear.

## [I-05] Consider using custom errors instead of require statements with string error
Custom errors reduce the contract size and can provide easier integration with a protocol. Consider using those instead of require statements with string error

## [I-06] Open TODO in code
In some places in the code have a open TODOs