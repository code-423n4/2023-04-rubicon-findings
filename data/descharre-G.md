# Summary
|ID     | Finding| Gas saved | Instances |
|:----: | :---           |  :----:         |:----:         |
|1       |Ternary operation is cheaper than if-else statement|30| 1 |
|2       |No need to check for overflow|192| 2 |
|3       |`_next_id()` can be incremented and returned in one line|192| 2 |
|4       |`auth` modifier shouldn't call another function|192| 2 |
|4       |Using double if/require statement instead of && consumes less gas|192| 2 |


# Details
## [G-01] Ternary operation is cheaper than if-else statement
Around 15 gas can be saved per optimization

[RubiconMarket.sol#L35-L41](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35-L41)
```diff
    function isAuthorized(address src) internal view returns (bool) {
-        if (src == owner) {
-            return true;
-        } else {
-            return false;
-        }
+        return src==owner ? true : false;
    }
```
Can also be done at:
- [RubiconMarket.sol#L876-L880](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L876-L880)
- [RubiconMarket.sol#L1186-L1191](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1186-L1191)
- [FeeWrapper.sol#L51-L55](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L51-L55)
## [G-02] No need to check for overflow
[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L46-L56) has a contract DSMath where it has math function and checks for under/overflow. This is not necassary because solidity performs these checks under the hood when the compilier is ^0.8.0.

Around 50 gas can be saved for every optimization

[RubiconMarket.sol#L1051-L1052](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1051-L1052)
```diff
-       fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator
-       pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell
+       fill_amt += offers[offerId].pay_amt; //Add amount bought to acumulator
+       pay_amt -= offers[offerId].buy_amt; //Decrease amount to sell
```
This can be done for every function that used add, sub or mul. This also counts for every function that uses the safeMath library.
## [G-03] `_next_id()` can be incremented and returned in one line
95 gas will be saved for this optimization
[RubiconMarket.sol#L568-L571](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568-L571)
```diff
    function _next_id() internal returns (uint256) {
-        last_offer_id++;
-        return last_offer_id;
+        return ++last_offer_id;
    }
```
## [G-04] `auth` modifier shouldn't call another function
The [`auth`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L30-L33) modifier calls the function `isAuthorized()`. However this is not necessary and the modifier can just be one line. 70 gas will be saved every time the `auth` modifier is used.
```diff
    modifier auth() {
-       require(isAuthorized(msg.sender), "ds-auth-unauthorized");
+       require(msg.sender == owner, "ds-auth-unauthorized");
        _;
    }
```
## [G-05] Using double if/require statement instead of && consumes less gas
Saves around 15 gas for require statements and 40 for if statements
```diff
-        require(
-            payAmts.length == payGems.length &&
-                payAmts.length == buyAmts.length &&
-                payAmts.length == buyGems.length,
-            "Array lengths do not match"
-        );
        require(payAmts.length == payGems.length, "Array lengths do not match");
        require(payAmts.length == buyAmts.length, "Array lengths do not match");
        require(payAmts.length == buyGems.length, "Array lengths do not match");
```

- [RubiconMarket.sol#L349](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349)
- [RubiconMarket.sol#L893-L897](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L893-L897)
- [RubiconMarket.sol#L940-L944](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L940-L944)
- [RubiconMarket.sol#L1197-L1203](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1197-L1203)
- [RubiconMarket.sol#L1324-L1328](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1324-L1328)
- [RubiconMarket.sol#L1412-L1415](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1412-L1415)
- [BathBuddy.sol#L95-L100](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L95-L100)
- [Position.sol#L391-L396](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L391-L396)