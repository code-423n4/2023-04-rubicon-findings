
1. Unnecessary use of ```DSMath```
There is no need to use DSMath/safe-math libraries for solidity versions >= 0.8 since underflows/overflows now revert. Unnecessary use of the library leads to extra gas costs when normal subtraction, addition, and multiplication is enough.
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L45



