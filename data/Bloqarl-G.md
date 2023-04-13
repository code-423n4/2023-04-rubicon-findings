## Gas Optimization report

# [G-01] Not using the named return variables in a function returns, wastes gas

In the `buy()` function it is returned a `bool`. And in the `_find()` function it is returned a `uint256` If we use a named parameter instead of using the return keyword, some extra gas can be saved. Considering that it is a function that will be used a lot, it will be important change.

There are two instances.

Gas saved per instance: 13 gas

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L317
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1208


# [G-02] <x> += <y> Costs More Gas Than <x> = <x> + <y>

Inside `openPosition()` function that it's going to be used very often we can save some gas by using `x = x + y` instead of `x += y`.

There are three instances in `Position.sol`. One of them is in a loop and another one is in a while. Which means that it can be saved an important amount of gas.

There is one instance of x -= y, which could also be changed to x = x - y in the file `RubiconMarket.sol`


Gas saved per instance: 13 gas


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L184
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L431
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L560
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L586




