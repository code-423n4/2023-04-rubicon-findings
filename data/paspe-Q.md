# [N1] Function isAuthorized() is useless, it is only called in the `modifier auth()` for the require statement

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L30-L42 
You can reduce code by removing internal isAuthorized() function. It can be written in a simple way.

```solidity
modifier auth() {
        require(msg.sender == owner, "ds-auth-unauthorized");
        _;
    }
```


# [N2] Commented code is useless and dead code - not executable
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L99-L116


# [N3] `event emitOffer` and `event emitTake` can have more better name according convention of naming events

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L118 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L152 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L174 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L198 

You can rename:
 emitOffer to `event LogMadeOffer(`
  emitTake to `event LogTransactionGemTokensFromSender`
emitCancel to `event LogCancelOfferIdPairOwnerOffer`
emitFee to LogFeeFromSenderToOfferOwner

# [N4] Modifier `can_offer` doesn’t make  any logic and it can be removed
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L260 

# [N5] `delete offers[id];` can produce an unused slot it is better to remove it with `.pop()`
 https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L437 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L456  

# [N6] `require(!locked);` is useless you can use `synchronized()` modifier instead
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1075 