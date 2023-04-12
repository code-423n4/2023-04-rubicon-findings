### Gas Optimizations
| |Issue|Instances|Total Gas Saved|
|:-:|:-|:-:|:-:|
| [G-01] | Multiple accesses of a storage variable should use a local variable cache | 57 |  2394 |
| [G-02] | `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables | 1 |  113 |
| [G-03] | Use nested `if` statements to avoid multiple check combinations using `&&` | 3 |  - |
| [G-04] | Emit memory value instead of state variable | 2 |  194 |
| [G-05] | Structs can be packed in to fewer storage slots | 2 |  40000 |
| [G-06] | Do not need to use SafeMath for solidity version above `0.8.0` | 2 |  - |
| [G-07] | Remove unused storage state variables | 1 | 76946 |
| [G-08] | Modifier `RubiconMarket.synchronized` should use uint for `locked` variable | 1 | 20100 |
| [G-09] | Use delete instead of zero assignment to clear storage variables | 3 | -|
| [G-10] | Remove unecessary variable declaration `calcAmountAfterFee` | 1 | 3 |
| [G-11] | Refactor `DSAuth.isAuthorized` function  | 1 | ~ 20 |
| [G-12] | Refactor `RubiconMarket._next_id()` function  | 1 | ~ 107 |
| [G-13] | Refactor `SimpleMarket.buy()` function   | 1 | - |
| [G-14] | `set` functions do not require a return boolean value   | 3 | ~ 57345 |
| [G-15] | State variables only set in constructor can be declared immutable  | 3 | 80000 |
| [G-16] | Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate  | 12 | 240000 |

| Total Found Issues | 16 |
|:--:|:--:|
| Total Gas Savings | Estimated >= 517222 |


Note: Most values for gas savings are estimations using remix so take with discretion

### [G-01] Multiple accesses of a mapping/array should use a local variable cache
### `SimpleMarket.offers()` mapping
[RubiconMarket.sol#L334-L335](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L334-L335)
[RubiconMarket.sol#L436-L437](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L436-L437)
```solidity
57 results - 2 files

/RubiconMarket.sol
314:    function buy
        ...
334:        offers[id].pay_amt = sub(_offer.pay_amt, quantity);
335:        offers[id].buy_amt = sub(_offer.buy_amt, spend);
        ...
436:        if (offers[id].pay_amt == 0)
437:            delete offers[id];
```
[RubiconMarket.sol#L1024](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1024)
```solidity
1020:    function isOfferSorted(uint256 id) public view returns (bool)
1024:            _best[address(offers[id].pay_gem)][address(offers[id].buy_gem)] 
```
[RubiconMarket.sol#L1045](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1045)
[RubiconMarket.sol#L1049-L1053](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1049-L1053)
[RubiconMarket.sol#L1058](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1058)
```solidity
1028:    function sellAllAmount
         ...
1045:                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
         ...
1049:            if (pay_amt >= offers[offerId].buy_amt) {
1050:                //If amount to sell is higher or equal than current offer amount to buy
1051:                fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator
1052:                pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell
1053:                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
         ...
1058:                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
```
[RubiconMarket.sol#L1085](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1085)
[RubiconMarket.sol#L1089-L1093](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1089-L1093)
[RubiconMarket.sol#L1100](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1100)
```solidity
1069:    function buyAllAmount
         ...
1085:                wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
         ...
1089:            if (buy_amt >= offers[offerId].pay_amt) {
1090:                //If amount to buy is higher or equal than current offer amount to sell
1091:                fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator
1092:                buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy
1093:                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
         ...
1100:                        rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
```
[RubiconMarket.sol#L1130-L1132](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1130-L1132)
[RubiconMarket.sol#L1143](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1143)
```solidity
1124:    function getBuyAmount
         ...
1130:        while (pay_amt > offers[offerId].buy_amt) {
1131:            fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount to buy accumulator
1132:            pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to pay
        ...
1143:                rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
```
[RubiconMarket.sol#L1163-L1165](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1163-L1165)
[RubiconMarket.sol#L1176](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1176)
```solidity
1157:    function getPayAmount
         ...
1163:        while (buy_amt > offers[offerId].pay_amt) {
1164:            fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount to pay accumulator
1165:            buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy
         ...
1176:                rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
```
[RubiconMarket.sol#L1185](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1185)
[RubiconMarket.sol#L1199](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1199)
```solidity
1183:    function _buys(uint256 id, uint256 amount) internal returns (bool)
         ...
1185:        if (amount == offers[id].pay_amt) 
         ...
1199:            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]
```
[RubiconMarket.sol#L1211-L1212](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1211-L1212)_
```solidity
1208:    function _find(uint256 id) internal view returns (uint256)
         ...
1211:        address buy_gem = address(offers[id].buy_gem);
1212:        address pay_gem = address(offers[id].pay_gem);
```
[RubiconMarket.sol#L1266-L1267](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1266-L1267)
```solidity
1261:    function _isPricedLtOrEq
         ...
1266:            mul(offers[low].buy_amt, offers[high].pay_amt) >=
1267:            mul(offers[high].buy_amt, offers[low].pay_amt);
```
[RubiconMarket.sol#L1291-L1292](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1291-L1292)
```solidity
1273:    function _matcho
         ...
1291:            m_buy_amt = offers[best_maker_id].buy_amt;
1292:            m_pay_amt = offers[best_maker_id].pay_amt;
```
[RubiconMarket.sol#L1368-L1369](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1368-L1369)
```solidity
1362:    function _sort
         ...
1368:        ERC20 buy_gem = offers[id].buy_gem;
1369:        ERC20 pay_gem = offers[id].pay_gem;
```
[RubiconMarket.sol#L1408-L1409](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1408-L1409)
```solidity
1405:    function _unsort
         ...
1408:        address buy_gem = address(offers[id].buy_gem);
1409:        address pay_gem = address(offers[id].pay_gem);
```

### `RubiconMarket._rank()` mapping
[RubiconMarket.sol#L942-L945](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L942-L945)
```solidity
/RubiconMarket.sol

937:    function del_rank(uint256 id) external returns (bool)
         ...
942:                _rank[id].delb != 0 &&
943:                _rank[id].delb < block.number - 10
944:        );
945:        delete _rank[id];
```
[RubiconMarket.sol#L1419-L1420](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1419-L1420)
[RubiconMarket.sol#L1426-L1429](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1426-L1429)
[RubiconMarket.sol#L1433](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1433)
```solidity
1405:    function _unsort
         ...
1419:            require(_rank[_rank[id].next].prev == id);
1420:            _rank[_rank[id].next].prev = _rank[id].prev;
         ...
1426:        if (_rank[id].prev != 0) {
1427:            //offers[id] is not the lowest offer
1428:            require(_rank[_rank[id].prev].next == id);
1429:            _rank[_rank[id].prev].next = _rank[id].next;
         ...
1433:        _rank[id].delb = block.number; //mark _rank[id] for deletion
```
The instances above point to the second+ access of a value inside a mapping/array, within a function. Caching a mapping’s value in a local storage or calldata variable when the value is accessed multiple times, saves ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations. Caching an array’s struct avoids recalculating the array offsets into memory/calldata.

### [G-02] `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
[Position.sol#L431](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L431)
```solidity
1 result - 1 file

/Position.sol
431:        positions[_positionId].bathTokenAmount += _bathTokenAmount;
```
Using the addition operator instead of plus-equals saves 113 gas. 

### [G-03] Use nested `if` statements to avoid multiple check combinations using `&&`
[RubiconMarket.sol#L349](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349)
[RubiconMarket.sol#L1324-L1328](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1324-L1328)
```solidity
4 results - 2 files

/RubiconMarket.sol
349:            if (_offer.owner == address(0) && getRecipient(id) != address(0)) 

1324:        if (
1325:            t_buy_amt > 0 &&
1326:            t_pay_amt > 0 &&
1327:            t_pay_amt >= _dust[address(t_pay_gem)]
1328:        )
```
[poolsUtility/Position.sol#L176](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L176)
[poolsUtility/Position.sol#L391-L394](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L391-L394)
```solidity
/Position.sol
176:            if (i.add(1) == vars.limit && vars.lastBorrow != 0)

391:        if (
392:            IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
393:            borrowBalance(_bathTokenAsset) == 0
394:        )
```

Description:
Using nested `if` statements is cheaper than using `&& ` for multiple check combinations. Additionally, it improves readability of code and better coverage reports.


### [G-04] Emit memory value instead of state variable
[RubiconMarket.sol#L27](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L27)
```solidity
2 result - 2 files

/RubiconMarket.sol
25:    function setOwner(address owner_) external auth {
26:        owner = owner_;
27:        emit LogSetOwner(owner);
28:    }
```
[BathBuddy.sol#L241](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L241)
```solidity
/BathBuddy.sol
232:    function setRewardsDuration(
233:        uint256 _rewardsDuration,
234:        address token
235:    ) external onlyOwner {
236:        require(
237:            block.timestamp > periodFinish[token],
238:            "Previous rewards period must be complete before changing the duration for the new period"
239:        );
240:        rewardsDuration[token] = _rewardsDuration;
241:        emit RewardsDurationUpdated(rewardsDuration[token]);
242:    }
```
The above instances can be refactored to emit the argument inputted stored in memory instead of emitting the storage variable. This saves gas as it replaces a SLOADs (Gwarmaccess, 100 gas) with a MLOAD (3 gas)

### [G-05] Structs can be packed in to fewer storage slots
[Position.sol#L33-L40](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L33-L40)
```solidity
2 results - 2 files

/Position.sol
33:    struct Position {
34:        address asset; // supplied as collateral
35:        address quote; // borrowed token
36:        uint256 borrowedAmount; // amount of borrowed quote
37:        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
38:        uint256 blockNum; // block number on which position was opened
39:        bool isActive; // false by default, when active - true
40:    }
```
[FeeWrapper.sol#L12-L17](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L12-L17)
```solidity
/FeeWrapper.sol
12:    struct CallParams {
13:        bytes4 selector; // function selector
14:        bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
15:        address target; // target contract to call
16:        FeeParams feeParams;
17:    }
```

Adhere to storage packing rules to avoid an extra Gsset (20000gas) for the first setting of the struct.

Example:
```solidity
/Position.sol
33:    struct Position {
34:        address asset; // supplied as collateral
35:        address quote; // borrowed token
36:        bool isActive; // false by default, when active - true
37:        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
38:        uint256 blockNum; // block number on which position was opened
39:        uint256 borrowedAmount; // amount of borrowed quote
40:    }
```

```solidity
/FeeWrapper.sol
12:    struct CallParams {
13:        bytes4 selector; // function selector
14:        address target; // target contract to call
15:        bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
16:        FeeParams feeParams;
17:    }
```


### [G-06] Do not need to use SafeMath for solidity version above `0.8.0`
[BathBuddy.sol#L6](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L6)
[Position.sol#L6](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L60)
```solidity
2 results - 2 files

/BathBuddy.sol
6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

/Position.sol
6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";
```
Solidity versions greater than `0.8.0` introduces inbuilt internal overflow and underflow checks so SafeMath is not required


### [G-07] Remove unused storage state variables
[RubiconMarket.sol#L682](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L682)
[RubiconMarket.sol#L684](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L684)
```solidity
2 results - 2 files

/RubiconMarket.sol
682:    bool public AqueductDistributionLive;

684:    address public AqueductAddress;
```
Remove unused storage state variables to save gas on deployment

### [G-08] Modifier `RubiconMarket.synchronized` should use uint for `locked` variable
[RubiconMarket.sol#L264-L269](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269)
```solidity
1 result - 1 file

/RubiconMarket.sol
264:    modifier synchronized() {
265:        require(!locked);
266:        locked = true;
267:        _;
268:        locked = false;
269:    }
```
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. See this [link.](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27)

### [G-09] Use delete instead of zero assignment to clear storage variables
[RubiconMarket.sol#L1449](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1449)
[RubiconMarket.sol#L1462](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1462)
```solidity
3 results - 2 files

/RubiconMarket.sol
1449:            _near[id] = 0;

1462:        _near[id] = 0;
```
[BathBuddy.sol#L181](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L181)
```solidity
/BathBuddy.sol
181:            tokenRewards[address(rewardsToken)][holderRecipient] = 0;
```
Use `delete` as it has the same effect of assigning variables to its default value based on its type and also saves gas

### [G-10] Remove unecessary variable declaration `calcAmountAfterFee`
[RubiconMarket.sol#L582-L583](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L582-L583)
```solidity
578:    function calcAmountAfterFee(
579:        uint256 amount
580:    ) public view returns (uint256 _amount) {
581:        require(amount > 0);
582:        _amount = amount;
583:        _amount -= mul(amount, feeBPS) / 100_000;
584:
585:        if (makerFee() > 0) {
586:            _amount -= mul(amount, makerFee()) / 100_000;
587:        }
589:    }
```

The `SimpleMarket.calcAmountAfterFee` can be refactored to:

```solidity
function calcAmountAfterFee(
    uint256 amount
) public view returns (uint256 _amount) {
    require(amount > 0);
    _amount -= mul(amount, feeBPS) / 100_000;

    if (makerFee() > 0) {
        _amount -= mul(amount, makerFee()) / 100_000;
    }
}
```

This avoids unecessary assignment of `_amount` variable twice, which saves gas since it avoids a additional MLOAD (3 gas)

### [G-11] Refactor `DSAuth.isAuthorized` function 
[RubiconMarket.sol#L35-L40](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35-L40)
```solidity
35:    function isAuthorized(address src) internal view returns (bool) {
36:        if (src == owner) {
37:            return true;
38:        } else {
39:            return false;
40:        }
41:    }
```

The function `isAuthorized()` can be refactored to 

```solidity
function isAuthorized(address src) internal view returns (bool) {
    return src == owner;
}
```

This also saves gas by removing unecessary `return` and `if..else ` operators as well as `true` and `false` values.


### [G-12] Refactor `RubiconMarket._next_id()` function
[RubiconMarket.sol#L568-L571)](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568-L571)
```solidity
568:    function _next_id() internal returns (uint256) {
569:        last_offer_id++;
570:        return last_offer_id;
571:    }
```
The function `_next_id()` can be refactored to 

```solidity
function _next_id() internal returns (uint256) {
    return ++last_offer_id;
}
```

This also saves gas from using pre-decrements and only accessing the state variable `last_offer_id` once (saves 1 SLOAD)
 
 
### [G-13] Refactor `SimpleMarket.buy()` function 
[RubiconMarket.sol#L314-L322](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L314-L322)
```solidity
314:    function buy(
315:        uint256 id,
316:        uint256 quantity
317:    ) public virtual can_buy(id) synchronized returns (bool) {
318:        OfferInfo memory _offer = offers[id];
319:        uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;
320:
321:        require(uint128(spend) == spend, "spend is not an int");
322:        require(uint128(quantity) == quantity, "quantity is not an int");
            ...
```
The function `buy()` limits `spend` calculated and input `quantity` to a maximum of 2**128-1. If quantity passed or spend calculated exceeds `type(uint128).max`, the typecasting will downcast the value and the `require` checks will fail. 

```solidity
function buy(
    uint256 id,
    uint128 quantity
) public virtual can_buy(id) synchronized returns (bool) {
    OfferInfo memory _offer = offers[id];
    uint128 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;
    ...
```
This function can be refactored to simply declaring `quantity` and `spend` as `uint128` so the `require` checks are not required since this variables will auto revert if the value assigned to them is greater than `type(uint128).max`. This can reduce overall SLOC and have massive gas savings on deployment and everytime function `buy` is called

### [G-14] `set` functions do not require a return boolean value
[RubiconMarket.sol#L1466-L1469](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466-L1469)
[RubiconMarket.sol#L1471-L1474](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471-L1474)
[RubiconMarket.sol#L1476-L1480](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1476-L1480)
```solidity
3 results - 1 file

/RubiconMarket.sol
1466:    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
1467:        feeBPS = _newFeeBPS;
1468:        return true;
1469:    }

1471:    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
1472:        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
1473:        return true;
1474:    }

1476:    function setFeeTo(address newFeeTo) external auth returns (bool) {
1477:        require(newFeeTo != address(0));
1478:        feeTo = newFeeTo;
1479:        return true;
1480:    }
```
Consider removing the return bool value as it is uneeded an incurs unecessary gas.

### [G-15] State variables only set in constructor can be declared immutable
[Position.sol#L55-L58](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L55-L58)
```solidity
4 results - 1 file

/Position.sol
54:    constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
55:        oracle = PriceOracle(_oracle);
56:        rubiconMarket = RubiconMarket(_rubiconMarket);
57:        bathHouseV2 = BathHouseV2(_bathHouseV2);
58:        comptroller = bathHouseV2.comptroller();
59:    }
```

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

### [G-16] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate
[RubiconMarket.sol#L692-L694](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L692-L694)
```solidity
12 results - 3 files

/RubiconMarket.sol
692:    mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
693:    mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook
694:    mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
```
[BathHouseV2.sol#L20-L21](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L20-L21)
```solidity
/BathHouseV2.sol
20:    mapping(address => address) private tokenToBathToken;
21:    mapping(address => address) private bathTokenToBuddy;
```
[BathBuddy.sol#L52-L56](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L51-L56)
[BathBuddy.sol#L59-L61](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L59-L61)
```solidity
/BathBuddy.sol
52:    mapping(address => uint256) public periodFinish; // Token specific
53:    mapping(address => uint256) public rewardRates; // Token specific reward rates
54:    mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
55:    mapping(address => uint256) public lastUpdateTime; //Token specific
56:    mapping(address => uint256) public rewardsPerTokensStored; // Token specific

59:    mapping(address => mapping(address => uint256))
60:        public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
61:    mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
```

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.