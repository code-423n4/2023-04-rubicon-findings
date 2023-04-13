|          | Issue                                                                             | Instances | Estimated Gas Savings |
| -------- | :-------------------------------------------------------------------------------- | :-------: | --------------------- |
| [GAS-1]  | Refactor `Position` struct                                                        |     1     | 43600 / 3900          |
| [GAS-2]  | Refactor `CallParams` struct in the `FeeWrapper` contract                         |     1     | 22100                 |
| [GAS-3]  | Optimize `getOffer()` function                                                    |     1     | 4400                  |
| [GAS-4]  | Refactor functions to use `storage` rather than `memory`                          |     3     | 2100                  |
| [GAS-5]  | Refactor `offer()` function in the `RubiconMarket` contract                       |     1     | 217                   |
| [GAS-6]  | Cache state variable values in memory                                             |     7     | 200                   |
| [GAS-7]  | Cache call to `getOwner(id)` in the `can_cancel()` function                       |     2     | 200                   |
| [GAS-8]  | Use `unchecked` for `lastPositionId` counter                                      |     1     | 120                   |
| [GAS-9]  | `x += y` costs more gas than `x = x + y` for state variables                      |     1     | 113                   |
| [GAS-10] | Simplify `isAuthorized`() function                                                |     1     | 35                    |
| [GAS-11] | Usage of `uint`s smaller than 32 bytes (256 bits) incurs overhead                 |    21     | 12 / 188              |
| [GAS-12] | Optimize `isActive()` function                                                    |     1     | 12                    |
| [GAS-13] | Use nested `if` instead of multiple `&&` combinations                             |     4     | 18                    |
| [GAS-14] | `require()/revert()` strings >= 32 chars cost extra gas                           |    12     | 18                    |
| [GAS-15] | Change return value of `getTime()` to `uint256`                                   |     1     | 9                     |
| [GAS-16] | Refactor `batchOffer()` and `batchRequote()` to use `struct`                      |     1     |
| [GAS-17] | Use separate `require` statements instead of `&&` operators                       |     5     | 10 \* n               |
| [GAS-18] | Use `calldata` instead of `memory` for function arguments that do not get mutated |     3     |
| [GAS-19] | State variables only set in the constructor should be declared `immutable`        |     4     |
| [GAS-20] | Optimize function names to save gas                                               |     X     |
| [GAS-21] | Use solidity version >=0.8.10 to avoid contract existence checks                  |     2     | 22 \* n               |

In the following report, gas optimizations are sorted by the estimated amount of gas savings.

All gas estimations were calculated using Remix IDE by deploying two separate contracts that differ only in one small part that was being tested.

Settings used: `{version: "0.8.17", settings: {optimizer: {enabled: true, runs: 100000}`

---

## [GAS-1] Refactor `Position` struct

The refactoring allows for the `asset` and `isActive` fields to be packed into a single storage slot.

Additionally, changing the type of the `blockNum` field from `uint256` to `uint64` also saves gas by packing it with the `quote` field. Since `blockNum` is always expected to be a block number, it does not need the full range of a `uint256` and can be safely represented as a `uint64`.

Overall, the refactoring reduces the total storage required for a `Position` struct by two storage slots, leading to significant gas savings.

Gas savings:

- creating new position: 43600 gas
- getting a position: 3900 gas

### Recommendation code:

```solidity
File: contracts/utilities/poolsUtility/Position.sol
- 34:        address asset; // supplied as collateral
- 35:        address quote; // borrowed token
- 36:        uint256 borrowedAmount; // amount of borrowed quote
- 37:        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
- 38:        uint256 blockNum; // block number on which position was opened
- 39:        bool isActive; // false by default, when active - true

+ 34:        uint256 borrowedAmount; // amount of borrowed quote
+ 35:        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
+ 36:        address asset; // supplied as collateral
+ 37:        bool isActive; // false by default, when active - true
+ 38:        address quote; // borrowed token
+ 39:        uint64 blockNum; // block number on which position was opened

```

[Position.sol#L33-L39](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L33-L39)

---

## [GAS-2] Refactor `CallParams` struct in the `FeeWrapper` contract

In Solidity, `struct` fields are packed tightly, which means that adjacent fields that fit in the same storage slot are packed into it. Moving the `args` field to the top of the struct, allows the `selector` and the `target` fields to be packed into a single storage slot, reducing the total number of storage slots required for the struct. This optimization can help to reduce the gas cost of contract deployment and storage operations.

Gas savings: 22100 gas

### Recommendation code:

```solidity
File: contracts/utilities/FeeWrapper.sol
- 12:    struct CallParams {
- 13:        bytes4 selector; // function selector
- 14:        bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
- 15:        address target; // target contract to call
- 16:        FeeParams feeParams;
- 17:    }

+ 12:    struct CallParams {
+ 13:        bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
+ 14:        bytes4 selector; // function selector
+ 15:        address target; // target contract to call
+ 16:        FeeParams feeParams;
+ 17:    }

```

[FeeWrapper.sol#L12-L17](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L12-L17)

---

## [GAS-3] Optimize `getOffer()` function

The current implementation saves the entire `OfferInfo` struct in memory, even though only four fields from it are used in the `return` statement. Accessing every unused slot wastes 2100 gas due to cold access to the storage variable.

Gas savings: 4400 gas

### Recommendation code example:

```solidity
File: contracts/RubiconMarket.sol
- 291:        OfferInfo memory _offer = offers[id];
- 292:        return (_offer.pay_amt, _offer.pay_gem, _offer.buy_amt, _offer.buy_gem);

+ 291:        return (offers[id].pay_amt, offers[id].pay_gem, offers[id].buy_amt, offers[id].buy_gem);
```

[RubiconMarket.sol#L291-L292](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L291-L292)

---

## [GAS-4] Refactor functions to use `storage` rather than `memory`

Creating a copy of the `Position` struct in memory incurs significant gas costs due to cold access to each storage slot of the struct. It might only make sense if every field of the struct is used more than once later in a function's code.

Therefore, it is recommended to replace copying the struct to memory with a storage pointer in the following functions: `borrowBalanceOfPos()`, `closePosition()`, and `increaseMargin()`.

Gas savings: 2100 gas per each cold access

3 instances in 1 file

### Recommendation code:

```solidity
File: contracts/utilities/poolsUtility/Position.sol
- 75:        Position memory pos = positions[posId];
+ 75:        Position storage pos = positions[posId];

- 211:        Position memory pos = positions[posId];
+ 211:        Position storage pos = positions[posId];

- 227:        Position memory pos = positions[posId];
+ 227:        Position storage pos = positions[posId];
```

[Position.sol#L75](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L75)

[Position.sol#L211](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L211)

[Position.sol#L227](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L227)

---

## [GAS-5] Refactor `offer()` function in the `RubiconMarket` contract

The refactoring optimizes the gas cost of the `offer()` function by eliminating the need to declare and initialize a memory variable to store the `OfferInfo` struct before assigning it to the `offers` mapping. Instead, the `offers[id]` mapping is assigned directly within the function, avoiding unnecessary memory allocation and reducing gas usage.

Additionally, the `_next_id()` function is simplified by using the `unchecked` keyword to suppress integer overflow checking, allowing for more efficient gas usage when incrementing the `last_offer_id` counter.

Gas savings: 217 gas

### Recommendation code:

```solidity
File: contracts/RubiconMarket.sol

- 527:        OfferInfo memory info;
- 528:        info.pay_amt = pay_amt;
- 529:        info.pay_gem = pay_gem;
- 530:        info.buy_amt = buy_amt;
- 531:        info.buy_gem = buy_gem;
- 532:        info.recipient = recipient;
- 533:        info.owner = owner;
- 534:        info.timestamp = uint64(block.timestamp);
- 535:        id = _next_id();
- 536:        offers[id] = info;

- 568:    function _next_id() internal returns (uint256) {
- 569:        last_offer_id++;
- 570:        return last_offer_id;
- 571:    }

+        unchecked {last_offer_id++;}
+        id = last_offer_id;
+        offers[id].pay_amt = pay_amt;
+        offers[id].pay_gem = pay_gem;
+        offers[id].buy_amt = buy_amt;
+        offers[id].buy_gem = buy_gem;
+        offers[id].recipient = recipient;
+        offers[id].owner = owner;
+        offers[id].timestamp = uint64(block.timestamp);

```

[RubiconMarket.sol#L527-L536](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L527-L536)

[RubiconMarket.sol#L568-L571](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L568-L571)

---

## [GAS-6] Cache state variable values in memory

Every call to the state variable after the first one incurs a cost of 100 gas (warm access). Therefore, it is advisable to cache the result of the first call in memory, in order to avoid wasting gas for the following calls to that variable.

Gas savings: ~200 gas

7 instances in 2 files:

```solidity
File: contracts/RubiconMarket.sol

// cache calls to `makerFee()`
314:    function buy(

// cache offers[offerId].buy_amt, offers[offerId].pay_amt
1028:    function sellAllAmount(

// cache offers[offerId].buy_amt, offers[offerId].pay_amt
1069:    function buyAllAmount(

// cache offers[offerId].buy_amt
1157:    function getPayAmount(

// cache offers[id].pay_amt
1183:    function _buys(uint256 id, uint256 amount) internal returns (bool) {

// cache _rank[id].delb
1405:    function _unsort(
```

```solidity
File: contracts/periphery/BathBuddy.sol

// cache periodFinish[token]
112:    function lastTimeRewardApplicable(
```

[RubiconMarket.sol#L314](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1314)

[RubiconMarket.sol#L1028](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1028)

[RubiconMarket.sol#L1069](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1069)

[RubiconMarket.sol#L1157](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1157)

[RubiconMarket.sol#L1183](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1183)

[RubiconMarket.sol#L1405](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1405)

[BathBuddy.sol#L112](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L112)

---

## [GAS-7] Cache call to `getOwner(id)` in the `can_cancel()` function

Each time `getOwner(id)` is called (after the first one), it incurs a cost of 100 gas due to the warm access to the corresponding state variable. As a result, it is advisable to cache the `getOwner(id)` value in memory prior to usage, in order to avoid incurring the same gas cost multiple times.

Gas savings: ~200 gas (when triggered the second check)

2 instances in 1 file:

[RubiconMarket.sol#L253-256](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L253-L256)

[RubiconMarket.sol#L612-L616](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L612-L616)

### Recommendation code:

```solidity
File: contracts/RubiconMarket.sol
- 253:        require(
- 254:            (msg.sender == getOwner(id)) ||
- 255:                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
- 256:        );

+ 253:        address cacheOwner = getOwner(id);
+            require(
+               (msg.sender == cacheOwner) ||
+                   (msg.sender == getRecipient(id) && cacheOwner == address(0))
+           );


- 612:        require(
- 613:            (msg.sender == getOwner(id)) ||
- 614:                isClosed() ||
- 615:                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
- 616:        );

+ 612:    address cacheOwner = getOwner(id);
+        require(
+            (msg.sender == cacheOwner) ||
+                isClosed() ||
+                (msg.sender == getRecipient(id) && cacheOwner == address(0))
+        );

```

---

## [GAS-8] Use `unchecked` for `lastPositionId` counter

It is safe to assume that `lastPositionId` won't exceed the maximum value for `uint256` type, so using `unchecked` to suppress overflow/underflow checks saves gas.

Gas savings: 120 gas

### Recommendation code:

```solidity
File: contracts/utilities/poolsUtility/Position.sol
- 407:        lastPositionId++;

+ 407:        unchecked {lastPositionId++;}
```

[Position.sol#L407](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L407)

---

## [GAS-9] `x += y` costs more gas than `x = x + y` for state variables

`x += y` costs more than `x = x + y`. The same applies for `x -= y`

Gas savings: 113 gas

1 instance in 1 file:

### Recommendation code:

```solidity
File: contracts/utilities/poolsUtility/Position.sol
- 431:        positions[_positionId].bathTokenAmount += _bathTokenAmount;

+ 431:        positions[_positionId].bathTokenAmount = positions[_positionId].bathTokenAmount + _bathTokenAmount;

```

[Position.sol#L431](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L431)

---

## [GAS-10] Simplify `isAuthorized()` function

The original function `isAuthorized()` checks if the provided address `src` is equal to the `owner` address, and returns `true` if they are the same, otherwise it returns `false`. However, since the comparison expression `src == owner` already returns a boolean value, it can be directly returned by the function without the need for an `if-else` statement.

Gas savings: 35 gas

### Recommendation code:

```solidity
File: contracts/RubiconMarket.sol
- 36:        if (src == owner) {
- 37:            return true;
- 38:        } else {
- 39:            return false;
- 40:        }

+ 36:        return (src == owner)
```

[RubiconMarket.sol#L36-L40](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L36-L40)

---

## [GAS-11] Usage of `uint`'s smaller than 32 bytes incurs overhead

For instance:

- get function for smaller `uint`'s costs 12 extra gas (for a storage variable)

- set function for smaller `uint`'s costs 188 extra gas (for a storage variable)

[From the Solidity docs:](https://docs.soliditylang.org/en/v0.8.18/internals/layout_in_storage.html#layout-of-state-variables-in-storage)

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

It might be beneficial to use reduced-size types if you are dealing with storage values because the compiler will pack multiple elements into one storage slot, and thus, combine multiple reads or writes into a single operation. If you are not reading or writing all the values in a slot at the same time, this can have the opposite effect, though: When one value is written to a multi-value storage slot, the storage slot has to be read first and then combined with the new value such that other data in the same slot is not destroyed.

21 instances in 1 file:

```solidity
File: contracts/RubiconMarket.sol
124:        uint128 pay_amt,
125:        uint128 buy_amt

159:        uint128 take_amt,
160:        uint128 give_amt

180:        uint128 pay_amt,
181:        uint128 buy_amt

405:            uint128(quantity),
408:            uint128(spend)

480:            uint128(_offer.pay_amt),
481:            uint128(_offer.buy_amt)

494:        uint128 pay_amt,
495:        uint128 buy_amt

559:            uint128(pay_amt),
560:            uint128(buy_amt)

564:    function take(bytes32 id, uint128 maxTakeAmount) external virtual {

736:        uint128 pay_amt,
737:        uint128 buy_amt

752:    function take(bytes32 id, uint128 maxTakeAmount) public override {

1061:                take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need

1093:                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1103:                take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need

```

### Recommendation:

Replace smaller `uint`'s with `uint256`

[RubiconMarket.sol#L124](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L124)

---

## [GAS-12] Optimize `isActive()` function

Using `pay_amt` instead of `timestamp`, or any other `uint256` variable from the `OfferInfo` struct, can save gas because it eliminates the need for the compiler to convert `uint64` to `uint256` before the actual check.

Gas savings: 12 gas

### Recommendation code:

```solidity
File: contracts/RubiconMarket.sol
- 277:        return offers[id].timestamp > 0;

+ 277:        return offers[id].pay_amt > 0;
```

[RubiconMarket.sol#L276-277](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L276-L277)

---

## [GAS-13] Use nested `if` instead of multiple `&&` combinations

Using nested `if` statements is cheaper than using multiple `&&` combinations. It also results in more readable code and better coverage reports.

Gas savings: 18 gas

4 instances in 2 files:

```solidity
File: contracts/utilities/poolsUtility/Position.sol
176:            if (i.add(1) == vars.limit && vars.lastBorrow != 0) {

391:        if (
392:            IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
393:            borrowBalance(_bathTokenAsset) == 0
394:        ) {
```

### Recommendation code example:

```solidity
File: contracts / RubiconMarket.sol
- 349:            if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

+                if (_offer.owner == address(0))
+                    if(getRecipient(id) != address(0)) {

- 1324:        if (
- 1325:            t_buy_amt > 0 &&
- 1326:            t_pay_amt > 0 &&
- 1327:            t_pay_amt >= _dust[address(t_pay_gem)]
- 1328:        ) {

+             if (t_buy_amt > 0)
+                 if(_pay_amt > 0)
+                     if(t_pay_amt >= _dust[address(t_pay_gem)])
```

[RubiconMarket.sol#L349](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L349)

[RubiconMarket.sol#L1324-L1328](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1324-L1328)

[Position.sol#L176](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L176)

[Position.sol#L391-L394](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L391-L394)

---

## [GAS-14] `require()/revert()` strings >= 32 chars cost extra gas

Even strings with `lengh = 32` cost extra 3 gas. Anything above that would cost additional 15 gas.

Gas savings: 18 gas (per each extra 32 chars)

The following items are missing in the automated findings report.

12 instances in 4 files:

```solidity
File: contracts / RubiconMarket.sol
376:            "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"

381:            "_offer.pay_gem.transfer(msg.sender, quantity) failed"

726:       "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
```

```solidity
File: contracts / BathHouseV2.sol
70:            "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"

74:            "createBathToken: UNDERLYING == ADDRESS 0"

78:            "createBathToken: IMPLEMENTATION == ADDRESS 0"
```

```solidity
File: contracts/periphery/BathBuddy.sol
238:            "Previous rewards period must be complete before changing the duration for the new period"
```

```solidity
File: contracts/utilities/poolsUtility/Position.sol
102:            "buyAllAmountWithLeverage: FAILED TO OPEN POSITION"

116:            "sellAllAmountWithLeverage: FAILED TO OPEN POSITION"

290:            "_repay: balance of quote lt. debt"

592:                "_leverageCheck{Long}: INVLAID LEVERAGE"

596:                "_leverageCheck{Short}: INVLAID LEVERAGE"
```

### Recommendation:

Try to limit `require` strings to 31 character.

[RubiconMarket.sol#L376](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L376)

[BathHouseV2.sol#L70](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L70)

[BathBuddy.sol#L238](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L238)

[Position.sol#L102](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L102)

---

## [GAS-15] Change `return` value of `getTime()` to `uint256`

By changing the return type to `uint256`, the compiler will not need to perform an implicit conversion, which can save gas costs.

Gas savings: 9 gas

### Recommendation code:

```solidity
File: contracts / RubiconMarket.sol

- 624:    function getTime() public view returns (uint64) {
- 625:        return uint64(block.timestamp);


+ 624:    function getTime() public view returns (uint256) {
+ 625:        return block.timestamp;
```

[RubiconMarket.sol#L624-L626](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L624-L626)

---

## [GAS-16] Refactor `batchOffer()` and `batchRequote()` to use `struct`

The refactoring simplifies the function's interface by consolidating four separate arrays into a single array of a new `struct` type, `BatchOffer`. The use of a `struct` type eliminates the need to check for array length consistency, reducing the complexity of the function and making it easier to read and maintain.

### Recommendation code example:

```solidity
File: contracts / RubiconMarket.sol
- 887:    function batchOffer(
- 888:        uint[] calldata payAmts,
- 889:        address[] calldata payGems,
- 890:        uint[] calldata buyAmts,
- 891:        address[] calldata buyGems
- 892:    ) external {
- 893:        require(
- 894:            payAmts.length == payGems.length &&
- 895:                payAmts.length == buyAmts.length &&
- 896:                payAmts.length == buyGems.length,
- 897:            "Array lengths do not match"
- 898:        );

+        struct BatchOffer {
+        uint256 payAmt;
+        address payGem;
+        uint256 buyAmt;
+        address buyGem;
+    }

+    function batchOffer(BatchOffer[] calldata offers) external {

```

[RubiconMarket.sol#L887-L898](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L887-L898)

[RubiconMarket.sol#L917-L923](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L917-L923)

---

## [GAS-17] Use separate `require` statements instead of `&&` operators

Each `require` statement is compiled into a separate `JUMP` instruction, whereas using `&&` operators creates a single `JUMP` instruction for all the conditions combined. When a `require` statement fails, it immediately terminates the execution of the function, which can save gas in the case where multiple conditions are being checked and only some of them fail. Additionally, using separate `require` statements can improve code readability and make debugging easier.

Gas savings: 10 gas per each split

5 instances in 3 files:

```solidity
File: contracts/RubiconMarket.sol
940:        require(
941:            !isActive(id) &&
942:                _rank[id].delb != 0 &&
943:                _rank[id].delb < block.number - 10
944:        );


1412:        require(
1413:            _rank[id].delb == 0 && //assert id is in the sorted list
1414:                isOfferSorted(id)
1415:        );
```

```solidity
File: contracts/periphery/BathBuddy.sol
95:        require(
96:            msg.sender == myBathTokenBuddy &&
97:                msg.sender != address(0) &&
98:                friendshipStarted,
99:            "You are not my buddy!"
100:        );
```

```solidity
File: contracts/utilities/poolsUtility/Position.sol
592:                _leverage > _wad && _leverage <= _leverageMax,

596:                _leverage >= _wad && _leverage <= _leverageMax,
```

### Recomendation code example:

```solidity
File: contracts/RubiconMarket.sol
- 940:        require(
- 941:            !isActive(id) &&
- 942:                _rank[id].delb != 0 &&
- 943:                _rank[id].delb < block.number - 10
- 944:        );

+        require(!isActive(id);
+        uint256 delbCache = _rank[id].delb;
+        require(delbCache != 0);
+        require(delbCache < block.number - 10);
```

[RubiconMarket.sol#L940-L944](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L940-L944)

[RubiconMarket.sol#L1412-L1415](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1412-L1415)

[BathBuddy.sol#L95-L100](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L95-L100)

[Position.sol#L591-L592](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L591-L592)

[Position.sol#L595-L596](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L595-L596)

---

## [GAS-18] Use `calldata` instead of `memory` for function arguments that do not get mutated

The following items are missing in the automated findings report.

3 instances in 1 files:

```solidity
File: contracts/utilities/FeeWrapper.sol
77:        CallParams memory \_params

93:    function _chargeFee(FeeParams memory _feeParams, address _target) internal {

109:        FeeParams memory _feeParams
```

[FeeWrapper.sol#L77](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L77)

[FeeWrapper.sol#L93](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L93)

[FeeWrapper.sol#L109](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L109)

---

## [GAS-19] State variables only set in the constructor should be declared `immutable`

When state variables are declared as `immutable`, the compiler is able to perform additional optimizations that can result in significant gas savings at runtime. This is because the `immutable` keyword informs the compiler that the value of the variable will not change after it is set in the constructor. As a result, the compiler can replace the variable with its value in the bytecode, which eliminates the need for additional storage reads and reduces the overall gas cost of the contract.

4 instances in 1 file:

```solidity
File: contracts/utilities/poolsUtility/Position.sol
44:    Comptroller public comptroller;
45:    PriceOracle public oracle;
46:    RubiconMarket public rubiconMarket;
47:    BathHouseV2 public bathHouseV2;
```

[Position.sol#L44-L47](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L44-L47)

---

## [GAS-20] Optimize function names to save gas </a>

Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID), because 22 gas are added to the cost of a function for every position that came before it.

It might be especially beneficial for the contracts containing many functions. For instance, `RubiconMarket` contract has 62 function declarations. Using analytics data from the V1, it is possible to optimize function names, at least for the most common used ones, thus saving considerable amounts of gas.

---

## [GAS-21] Use solidity version >=0.8.10 to avoid contract existence checks

Prior to `0.8.10` the compiler inserted extra opcode `EXTCODESIZE` (100 gas) to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value.

2 instances in 2 files:

```solidity
File: contracts/RubiconMarket.sol
2: pragma solidity ^0.8.9;
```

```solidity
File: contracts/periphery/BathBuddy.sol
2: pragma solidity ^0.8.9;
```

---
