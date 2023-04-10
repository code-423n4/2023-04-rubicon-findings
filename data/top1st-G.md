## RubiconMarket.sol
### id is always > 0 don't need require check.
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1209

### Don't need dustId to be public variable in cancel
dustId is global variable, Don't need to be public. Make it private or internal and save deploy gas fee
Better solution is make internal _cancel function and remove dustId global variable.

