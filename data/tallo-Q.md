
[L-01] The variable ```matchingEnabled``` inside RubiconMarket is always true and the contract contains no functionality to alter it. This variable is used in several conditionals and it is likely the developers intention that this variable can be changed. Consider adding a specific function that allows this variable to be changed by a whitelisted address.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L676
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L714
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L863

[L-02] Useless function stop
The function ```stop``` inside expiring market sets the variable ```stopped``` to ```true``` but that variable isnt used anywhere else. This means both the variable and function are useless
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L594

[L-03] ```buy``` return value is not check inside _matcho
```buy``` has a return value of ```bool``` and can fail for any of the following reasons.
```
        if (
            quantity == 0 ||
            spend == 0 ||
            quantity > _offer.pay_amt ||
            spend > _offer.buy_amt
        ) {
            return false;
        }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1313
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L325-L332



[N-01]. Useless function ```bump```
The function "bump(bytes32)" is useless and has no functionality. The comment directly specifies that ```bump``` is one of the "main public entrypoints" so consider adding some functionality or removing the function.
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297

[N-02] Unnecessary use of ```DSMath```
There is no need to use DSMath/safe-math libraries for solidity versions >= 0.8 since underflows/overflows now revert. Unnecessary use of the library leads to extra gas costs/overhead when normal subtraction, addition, and multiplication is enough. 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L45

[N-03] Variable and function names should have both descriptive comments and descriptive names that indicate their functionality and use cases
It's important for code readability and understanding for auditors and developers who aren't fully familiar with the code base in order to quickly get up to speed. For instance, each of the different ```offer``` variations should have a comment that describes their individual purpose; especially if one version is calling ```_matcho``` where another calls ```offer```.

[N-04] Many of the re-entrancy checks do nothing
For instance, on offer there is a check that locked is not true. This actually doesn't do anything because locked is only set inside the modifier ```synchronized()```.
```
require(!locked, "Reentrancy attempt");
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L834
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L859
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L938
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1034
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1075