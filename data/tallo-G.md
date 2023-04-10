
1. Unnecessary use of ```DSMath```
There is no need to use DSMath/safe-math libraries for solidity versions >= 0.8 since underflows/overflows now revert. Unnecessary use of the library leads to extra gas costs when normal subtraction, addition, and multiplication is enough.
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L45

2. Useless modifier ```can_offer```
the modifier "can_offer" inside RubiconMarket is useless and has no functionality
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260

3. Useless function ```bump```
The function "bump(bytes32)" is useless and has no functionality
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297

4. Useless function stop
The function ```stop``` inside expiring market sets the variable ```stopped``` to ```true``` but that variable isnt used anywhere else. This means both the variable and function are useless
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L594

5. Useless ```matchingEnabled``` variable and functionality
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L676
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L714
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L863