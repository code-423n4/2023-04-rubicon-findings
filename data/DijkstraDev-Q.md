
#### [L-01] Returned value by the `_unsort()` function is not used.
- On line [RubiconMarket#L877](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L877), the return value is used within a `require`, but on line [RubiconMarket#L1188](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1188) the return value is *not checked*. Recommendation: Unify criteria in the use of the returned value.

---

#### **[QA-01]** Remove commented code and functions that do nothing 
[RubiconMarket.sol#L297](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L297)

#### **[QA-02]** Remove duplicate checks. 
- In the current implementation, the `_hide` function is called only if the `isOfferSorted` function returns `false`. Checking this again inside the `_hide` function is redundant and wastes unnecessary gas -> [RubiconMarket.sol#L1444](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1444)
 
- In the current implementation, the `_unsort` function is called only if the `isOfferSorted` function returns `true`. Checking this again inside the `_unsort` function is redundant and wastes unnecessary gas -> [RubiconMarket.sol#L1405](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1405)

- In the current implementation, the `_bathify` function is _internal_ and is called only inside of `createBathToken` function which already check for `underlying != 0`. Checking this again inside the `_bathify` function is redundant and wastes unnecessary gas -> [BathHouseV2.sol#L144](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L144)

#### **[QA-03]** `stoped` is never used. Can be removed.
[RubiconMarket#L629](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L629)

#### **[QA-04]** `getTime()` is never used. Can be removed o changed to `external`.
[RubiconMarket#L624](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L624)

#### **[QA-05]** `AqueductDistributionLive` and `AqueductAddress` are marked as _deprecated_.
Can be removed.
[RubiconMarket#L624](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L682)
[RubiconMarket#L624](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L684)

#### **[QA-06]** Remove unused modifier. 
[BathBuddy.sol#L94](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathBuddy.sol#L94)

#### **[QA-07]** Remove unused param
[Position.sol#L528](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/Position.sol#L528)

#### **[QA-08]** Typos
[RubiconMarket.sol#L70](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L70)

[FeeWrapper.sol#L7](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L7)

#### **[QA-9]** Avoid Shadowing
[FeeWrapper.sol#L33](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L33)



### **[QA-10]** Lack of return value checks of the `cancel` function.
Sometimes the value is checked, sometimes not. It is recommended to standardize its use.

[RubiconMarket.sol#L871](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L871)

### **[QA-11]** `matchingEnabled` value is always true. Redundant check.
The `matchingEnabled` value is always `true` after initialize. So, checking `if (matchingEnabled){}` is redundant and an unnecessary waste of gas.

[RubiconMarket.sol#L863](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L863)
[RubiconMarket.sol#L876](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L876)

### **[QA-12]** Unnecessary checks when calling `offer`.
External `offer` functions call the public `offer` function, which already performs that check. 

Public function -> [RubiconMarket.sol#L824](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L824)

[RubiconMarket.sol#L802](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L802)

[RubiconMarket.sol#L780](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L780)

[RubiconMarket.sol#L761](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L761)

### **[QA-13]** Swap `require` statements order.
It makes more sense to first check if the `underlying` _address != address(0)_ before checking if a BathToken exists for that token. Also, if it is zero, it would always pass the first check.

[RubiconMarket.sol#L68](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L68)

[RubiconMarket.sol#L72](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L72)