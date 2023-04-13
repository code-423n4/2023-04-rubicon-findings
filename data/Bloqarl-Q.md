## QA Report

# [L-01] Access Control Vulnerability - `spawnBuddy()` can be accessed by anyone

`spawnBuddy()` is external and inside we are setting the owner of the contract which is a few lines later used for the `onlyOwner()` modifier. 

This means, that those functions expected to be restricted by onlyOwner like `notifyRewardAmount()` and `setRewardsDuration()` can be accessed by anyone considering the fact that the owner can be set to an attacker through `spawnBuddy()`.

To solve this the access control of `spawnBuddy()` should be more restrictive.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L75


# [L-02] Unused function bump() & stop()
The function bump() seems that it's not planned to be used and most likely can be removed.

However, the function `stop()` it is unused but there is risk that there was an expected functionality which has been forgotten or not implemented accordingly. It would be convenient to review the feature and make sure where was `stop()` function meant to be used.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L297
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L628


# [L-03] return directly last_offer_id++;
`_nextId()` internal function can be reduced to one line by making the increase in the return statement such as:

```
function _next_id() internal returns (uint256) {
    return last_offer_id++;
}
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L568

# [L-04] isClosed() Feature is wrongly implemented as it has no effect

In `RubiconMarket` there is the function `isClosed()` which is hardcoded to always return false

```
function isClosed() public pure returns (bool closed) {
    return false;
}
```

And this function is used in multiple places such as a require() in modifiers `can_offer()`, `can_offer()` and `can_cancel` where for instance can_offer() has no utility because it is always going to be true and has no restriction whatsoever.

And in an if() statement in can_cancel() modifier happens the same thing where the return will always be false


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L597
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L603
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L610
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L614


# [L-05] Avoid magic numbers

In order to make the code more readable for future changes, reviews or other usages, it is always convenient to not use numbers which are not named. That's why it's always define such numbers as a constant or variable depending of its usage.

For instance if you see in the code 

```
assembly {
    foo := calldataload(4)
    bar := calldataload(36)
    wad := callvalue()
}
```

and you didn't write it yourself it is very possible that the reviewer will not know what those numbers represent.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L648


# [L-06] Named return parameter unused
There are a few instances where the function is declaring a named parameter and then it is not used in the return.

It can be removed.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L276
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L280
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L284
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L518
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L873


# [L-07] There should be a check to validate feeType is not zero

Division by zero is an exception that will result in a reverted transaction. Since it might cause issues, it is convenient to validate it before using it.

In `FeeWrapper.sol` we have the function `calculateFee()` which is passing the parameter feeType which is later used as dividend.

To avoid this, there should be a check before this operation is done to avoid risk of being reverted the transaction.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L41









