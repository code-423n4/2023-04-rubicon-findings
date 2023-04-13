# Summary

The code has a number of comments making the reader feels it is unfinished. Comments such as: `/// TODO: this event is not used in the contract, remove?` or `// TODO: definitely need to work on naming things`.

The transition from V1 to V2 seems handled carefully and looks overall safe.

The code use safe math function not necessary in this version of solidity anymore, making it unnecessarily harder to read.

It feels like some logic is left to be implemented like the expiry of `ExpiringMarket`.

## Non-Critical 1: wrong event value due to V2 transition

Due to migration from V1 to V2, the value used in event `emitFee()` during `buy()` will be wrong for V1 offers. It uses `_offer.owner` for `feeTo` while the fee is given to `_offer.recipient` for V1 offer and owner is `address(0)`: 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L368

Same for `emitTake()` with `taker`:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L402

Same for `emitDelete`:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L443

Same for `emitCancel`:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L477

## Non-Critical 2: ExpiringMarket or RubiconMarket never expire

The function `isClosed` constantly return false, the logic for market expiring after timestamp is missing:
```solidity
    function isClosed() public pure returns (bool closed) {
        return false;
    }
```

The function is not overriden by `RubiconMarket`.

## Non-Critical 3: RubiconMarket uses non proxy-friendly variables

RubiconMarket uses an `initialize()` function to set its variable in storage instead of constructor to be proxy-friendly, but it also declares storage variables:
```solidity
    bool public buyEnabled = true;
    bool public matchingEnabled = true;
```

Which is not proxy-friendly. It is currently not a problem because the variables are set to true in the initialize() function.

## Non-Critical 4: Naming of rounding / matching in offer functions

RubiconMarket defines multiple `offer()` functions, each with different input arguments. Some of these function use a boolean `rounding` or `matching`, which ends up passed to `_matcho()` as the `rounding` parameter. It represents whether offers with a difference of 1 in price will be accepted. 

`matching` should be renamed to `rounding` where present:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L830

## Non-Critical 5: Missing require(hide(id))

The function to hide an element of the unsorted list of offer returns `false` on failure. However the call to it in `_buys()` does not check the returned value.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1190

I don't think there is currently a way to abuse this problem so I mark it as non-critical but it should be safer in the future to replace the call with `require(_hide(id))`

## Non-Critical 6: sell/buyAllAmount should break when no more offer

The functions used to match multiple offers sellAllAmount and buyAllAmount loop through the best offers until the user has sold/bought enough. If there is no offer to buy/sell anymore, the call will revert due to `require(offerId != 0, "offerId == 0")`. It should `break` when no offers can be matched anymore and let the requirement on `fill_amt` revert in the case not enough has been bought / sold for the user.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1040

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1080

This will improve the matchability for the user.

## Low 1: RubiconMarket.buy() does not require buyEnabled

The function `RubiconMarket.buy()` calls either `_buys()` or `super.buy()` depending on the value of `matchingEnabled`. 

The function `_buys()` checks `require(buyEnabled)`:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1183-L1184

But this check is not present in `super.buy()`. This check will be by-passed when `matchingEnabled` is false and users will be able to buy while `buyEnabled` is false.

This is low as the `buyEnabled` and `matchingEnabled` values are currently constantly true. If the contract evolve it could become a problem.

## Low 2: sell/buyAllAmount has arbitrary breaking criteria

sellAllAmount loops over best offers and matches them until the user has sold enough. It has the following breaking criteria:

```solidity
            if (
                mul(pay_amt, 1 ether) <
                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
            ) {  
                break; //We consider that all amount is sold
            }
```

Meaning that it breaks when `pay_amt < offer_buy_amt / offer_pay_amt`. It breaks when `pay_amt` is lower than the sell price. This looks quite arbitrary and may often break for tokens with different decimals.

buyAllAmount has the same break check.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1083-L1088

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1043-L1048
