
[L-01] The variable ```matchingEnabled``` inside RubiconMarket is always true and the contract contains no functionality to alter it. This variable is used in several conditionals and it is likely the developers intention that this variable can be changed. Consider adding a specific function that allows this variable to be changed by a whitelisted address.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L676
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L714
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L863

[L-02] Useless function stop
The function ```stop``` inside expiring market sets the variable ```stopped``` to ```true``` but that variable isnt used anywhere else. This means both the variable and function are useless
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L594

[N-01]. Useless function ```bump```
The function "bump(bytes32)" is useless and has no functionality. The comment directly specifies that ```bump``` is one of the "main public entrypoints" so consider adding some functionality or removing the function.
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297


