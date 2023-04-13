# Report

## Low Issues


| |Issue|
|-|:-|
| [L-1](#L-1) | Denominator can be zero |
| [L-2](#L-2) | Insufficient validation in `offer()` |
| [L-3](#L-3) | Unused functions should be removed |
| [L-4](#L-4) | `decimals()` is not an `ERC20` method |
| [L-5](#L-5) | Owner can renounce ownership |


### [L-1] Denominator can be zero
check if `feeType > 0`. This extra protection is recommended, as `FeeWrapper` is a third party helper that will be used externally by other protocols.

*Instances (2)*:



```solidity
File: utilities/FeeWrapper.sol

37:             fees[i] = (tokenAmounts[i] * feeValue) / feeType;
                amountsWithFee[i] = tokenAmounts[i] - fees[i];
            }

```

check if `_price > 0`

```solidity
File: utilities/poolsUtility/Position.sol

293:         _max = (_liq.mul(10**18)).div(_price);
             require(_max > 0, "_maxBorrow: can't borrow 0");
         }

```

### [L-2] Insufficient validation in `offer()`
The function should have an additional check to ensure  `recipient` and `owner` are not both `address(0)`, as in such case the `pay_gem` would be frozen in the market: users can neither cancel nor buy it (both reverting because cannot transfer to address(0))

*Instances (1)*:
```solidity
File: RubiconMarket.sol
532: info.recipient = recipient;
```

### [L-3] Unused functions should be removed

*Instances (1)*:
```solidity
File: RubiconMarket.sol
297: function bump(bytes32 id_) external can_buy(uint256(id_)) {
298:         uint256 id = uint256(id_);
299:         /// TODO: double check it is sound logic to kill this event
300:         /// emit LogBump(
301:         ///     id_,
302:         ///     keccak256(abi.encodePacked(offers[id].pay_gem, offers[id].buy_gem)),
303:         ///     offers[id].owner,
304:         ///     offers[id].pay_gem,
305:         ///     offers[id].buy_gem,
306:         ///     uint128(offers[id].pay_amt),
307:         ///     uint128(offers[id].buy_amt),
308:         ///     offers[id].timestamp
309:         /// );
310:     }
```

### [L-4] `decimals()` is not an `ERC20` method
decimals() is not part of the official ERC20 standard and might fail for tokens that do not implement it. While the tokens used by the protocols all have that function, it is still better practice to user the `ERC20Metadata` interface.

*Instances (1)*:
```solidity
File: BathHouseV2.sol

148:         _decimals = ERC20(_underlying).decimals();
         }
     }

```


### [L-5] Owner can renounce ownership
The `Ownable` library has a `renounceOwnership` function. This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner

*Instances (1)*:
```solidity
File: utilities/poolsUtility/Position.sol

15: contract Position is Ownable, DSMath {

```



## Non Critical Issues


| |Issue|
|-|:-|
| [NC-1](#NC-1) | Large numbers should use an underscore |
| [NC-2](#NC-2) | Constants on the left are better |
| [NC-3](#NC-3) | Use `delete` rather than assigning zero to clear values |
| [NC-4](#NC-4) | Inconsistent use of `uint` |
| [NC-5](#NC-5) | Commented code should be removed |
| [NC-6](#NC-6) | Order of functions not following Solidity standard practice | 
| [NC-7](#NC-7) | Events in admin setters should emit both the old and the new value |
| [NC-8](#NC-8) | Scientific Notation |
| [NC-9](#NC-9) | Use a more recent version of Solidity |
| [NC-10](#NC-10) | Address receiving ETH should be payable |
| [NC-11](#NC-11) | Assembly blocks should have comments |
| [NC-12](#NC-12) | Inconsistent spacing in comments |




### [NC-1] Large numbers should use an underscore
To improve readability, use an underscore for multiples of 1000

*Instances (3)*:
```solidity
File: utilities/poolsUtility/Position.sol

459:         uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);

481:         uint256 _fee = _minFill.mul(_feeBPS).div(10000);

490:         _fee = _payAmount.mul(_feeBPS).div(10000);

```

### [NC-2] Constants on the left are better
This is [safer in case you forget to put a double equals sign](https://www.moserware.com/2008/01/constants-on-left-are-better-but-this.html), as the compiler wil throw an error instead of assigning the value to the variable.

*Instances (44)*:
```solidity
File: BathHouseV2.sol

27:         require(msg.sender == admin, "onlyAdmin: !admin");

74:             "createBathToken: UNDERLYING == ADDRESS 0"

78:             "createBathToken: IMPLEMENTATION == ADDRESS 0"

```

```solidity
File: RubiconMarket.sol

36:         if (src == owner) {

55:         require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");

55:         require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");

254:             (msg.sender == getOwner(id)) ||

255:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))

326:             quantity == 0 ||

327:             spend == 0 ||

349:             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

436:         if (offers[id].pay_amt == 0) {

459:         _offer.owner == address(0) && msg.sender == _offer.recipient

459:         _offer.owner == address(0) && msg.sender == _offer.recipient

618:             (msg.sender == getOwner(id)) ||

620:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))

728:                 msg.sender == getOwner(id) ||

729:                 id == dustId ||

730:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0)),

899:             payAmts.length == payGems.length &&

900:                 payAmts.length == buyAmts.length &&

901:                 payAmts.length == buyGems.length,

1085:             require(offerId != 0, "offerId == 0");

1190:         if (amount == offers[id].pay_amt) {

1238:         if (pos == 0) {

1324:             if (t_pay_amt == 0 || t_buy_amt == 0) {

1324:             if (t_pay_amt == 0 || t_buy_amt == 0) {

1377:         pos = pos == 0 ||

1418:             _rank[id].delb == 0 && //assert id is in the sorted list

1424:             require(_rank[_rank[id].next].prev == id);

1433:             require(_rank[_rank[id].prev].next == id);

1451:         if (_head == id) {

```

```solidity
File: periphery/BathBuddy.sol

88:         require(msg.sender == owner);

96:             msg.sender == myBathTokenBuddy &&

105:         require(msg.sender == bathHouse, "You are not my beloved bath house!");

```

```solidity
File: utilities/FeeWrapper.sol

51:         if (msg.value == 0) {

115:         require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");

```

```solidity
File: utilities/poolsUtility/Position.sol

163:             if (vars.initAssetBalance == 0) {

267:         _borrow(_bathTokenQuote, _toBorrow);//@note after call TEST.balance == 19.4e18

312:         require(_err == 0, "_maxBorrow: ERROR");

313:         require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");

314:         require(_shortfall == 0, "_maxBorrow: SHORTFALL != 0");

542:             if (_limit == 0) {

574:             } else if (_assetAmount == _desiredAmount) {

```

### [NC-3] Use `delete` rather than assigning zero to clear values
This improves code readability, making the state change more obvious.

*Instances (3)*:
```solidity
File: RubiconMarket.sol

1067:                 pay_amt = 0; //All amount is sold

1109:                 buy_amt = 0; //All amount is bought

1219:         uint256 old_top = 0;

```

### [NC-4] Inconsistent use of `uint`
Throughout the contracts, `uint256` is used, but `uint` is used in some instances. For consistency, use only one or the other. This can also be a problem for [structured data signing](https://eips.ethereum.org/EIPS/eip-712#definition-of-typed-structured-data-%F0%9D%95%8A)

*Instances (4)*:
```solidity
File: RubiconMarket.sol

790:         uint pos, //position to insert offer, 0 should be used if unknown

904:         for (uint i = 0; i < payAmts.length; i++) {

916:         for (uint i = 0; i < ids.length; i++) {

929:         for (uint i = 0; i < ids.length; i++) {

```


### [NC-5] Commented code should be removed

*Instances (1)*:
```solidity
File: utilities/FeeWrapper.sol

File: RubiconMarket.sol
386:         /// emit LogTake(
387:         ///     bytes32(id),
388:         ///     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
389:         ///     _offer.owner,
390:         ///     _offer.pay_gem,
391:         ///     _offer.buy_gem,
392:         ///     msg.sender,
393:         ///     uint128(quantity),
394:         ///     uint128(spend),
395:         ///     uint64(block.timestamp)
396:         /// );

```

### [NC-6] Order of functions not following Solidity standard practice
Follow Solidity's [style guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) for function ordering: constructor, receive/fallback, external, public, internal and private.

*Instances (1)*:


```solidity
File: utilities/poolsUtility/Position.sol

86:     function borrowRate(address bathToken) public view returns (uint256 rate) {

93:     function buyAllAmountWithLeverage(//@audit external function defined after a public one


```


### [NC-7] Events in admin setters should emit both the old and the new value
This will make it easier to track the value change

*Instances (1)*:

```solidity
File: RubiconMarket.sol

713:         emit LogSetOwner(msg.sender);

```

### [NC-8] Scientific Notation
It is good practice to use scientific notation instead of using exponents or explicitly writing number of high decimals

*Instances (5)*:
```solidity
File: RubiconMarket.sol

338:         uint256 fee = mul(spend, feeBPS) / 100_000;

346:             uint256 mFee = mul(spend, makerFee()) / 100_000;

```

```solidity
File: utilities/poolsUtility/Position.sol


459:         uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);

481:         uint256 _fee = _minFill.mul(_feeBPS).div(10000);

490:         _fee = _payAmount.mul(_feeBPS).div(10000);

```

### [NC-9] Use a more recent version of Solidity
 Use a solidity version of at least 0.8.12 to get string.concat() to be used instead of abi.encodePacked(,), at least 0.8.13 to get the ability to use using for with a list of free functions

*Instances (2)*:
```solidity
File: RubiconMarket.sol

2: pragma solidity ^0.8.9;

```

```solidity
File: periphery/BathBuddy.sol

2: pragma solidity ^0.8.0;

```

### [NC-10] Address receiving ETH should be payable
Low level calls can be performed on non-payable addresses, even when sending value. But to follow best practice (see OpenZeppelin's [Address library](https://github.com/code-423n4/2022-11-paraspace/blob/c6820a279c64a299a783955749fdc977de8f0449/paraspace-core/contracts/dependencies/openzeppelin/contracts/Address.sol#L60-L65)) and add an extra layer of safety during compilation, consider casting these addresses as payable

*Instances (1)*:
```solidity
File: utilities/FeeWrapper.sol

82:         (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(

```

### [NC-11] Assembly blocks should have comments
Comments clearly explaining what each assembly instruction does will make it easier for users to review the code.

*Instances (2)*:
```solidity
File: RubiconMarket.sol

653:         assembly {

```

```solidity
File: utilities/poolsUtility/Position.sol

367:         assembly {

```




### [NC-12] Inconsistent spacing in comments
Most comments in the code have a space between the slash and the comment, these instances don't:

*Instances (91)*:
```solidity
File: RubiconMarket.sol

680:     bool public buyEnabled = true; //buy enabled TODO: review this decision!

681:     bool public matchingEnabled = true; //true: enable matching,

692:         uint256 next; //points to id of next higher offer

693:         uint256 prev; //points to id of previous lower offer

694:         uint256 delb; //the blocknumber where this entry was marked for delete

696:     mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids

697:     mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair

698:     mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook

699:     mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers

700:     mapping(uint256 => uint256) public _near; //next unsorted offer id

701:     uint256 public _head; //first unsorted offer id

767:         uint256 pay_amt, //maker (ask) sell how much

768:         ERC20 pay_gem, //maker (ask) sell which token

769:         uint256 buy_amt, //maker (ask) buy how much

770:         ERC20 buy_gem //maker (ask) buy which token

786:         uint256 pay_amt, //maker (ask) sell how much

787:         ERC20 pay_gem, //maker (ask) sell which token

788:         uint256 buy_amt, //maker (ask) buy how much

789:         ERC20 buy_gem, //maker (ask) buy which token

790:         uint pos, //position to insert offer, 0 should be used if unknown

808:         uint256 pay_amt, //maker (ask) sell how much

809:         ERC20 pay_gem, //maker (ask) sell which token

810:         uint256 buy_amt, //maker (ask) buy how much

811:         ERC20 buy_gem, //maker (ask) buy which token

812:         uint256 pos, //position to insert offer, 0 should be used if unknown

830:         uint256 pay_amt, //maker (ask) sell how much

831:         ERC20 pay_gem, //maker (ask) sell which token

832:         uint256 buy_amt, //maker (ask) buy how much

833:         ERC20 buy_gem, //maker (ask) buy which token

834:         uint256 pos, //position to insert offer, 0 should be used if unknown

835:         bool matching, //match "close enough" orders?

887:         return super.cancel(id); //delete the offer.

961:         ERC20 pay_gem, //token to assign minimum sell amount to

962:         uint256 dust //maker (ask) minimum sell amount

971:         ERC20 pay_gem //token for which minimum sell amount is queried

1044:             offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair

1045:             require(offerId != 0, "0 offerId"); //Fails if there are not more offers

1052:                 break; //We consider that all amount is sold

1056:                 fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator

1057:                 pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell

1058:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1065:                 fill_amt = add(fill_amt, baux); //Add amount bought to acumulator

1066:                 take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need

1067:                 pay_amt = 0; //All amount is sold

1084:             offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair

1092:                 break; //We consider that all amount is sold

1096:                 fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator

1097:                 buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

1098:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1107:                 ); //Add amount sold to acumulator

1108:                 take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need

1109:                 buy_amt = 0; //All amount is bought

1134:         uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair

1136:             fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount to buy accumulator

1137:             pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to pay

1140:                 offerId = getWorseOffer(offerId); //We look for the next best offer

1141:                 require(offerId != 0); //Fails if there are not enough offers to complete

1150:         ); //Add proportional amount of last offer to buy accumulator

1167:         uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair

1169:             fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount to pay accumulator

1170:             buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

1173:                 offerId = getWorseOffer(offerId); //We look for the next best offer

1174:                 require(offerId != 0); //Fails if there are not enough offers to complete

1183:         ); //Add proportional amount of last offer to pay accumulator

1206:             dustId = id; //enable current msg.sender to call cancel(id)

1267:         uint256 low, //lower priced offer's id

1268:         uint256 high //higher priced offer's id

1279:         uint256 t_pay_amt, //taker sell how much

1280:         ERC20 t_pay_gem, //taker sell which token

1281:         uint256 t_buy_amt, //taker buy how much

1282:         ERC20 t_buy_gem, //taker buy which token

1283:         uint256 pos, //position id

1284:         bool rounding, //match "close enough" orders?

1288:         uint256 best_maker_id; //highest maker id

1289:         uint256 t_buy_amt_old; //taker buy how much saved

1290:         uint256 m_buy_amt; //maker offer wants to buy this much token

1291:         uint256 m_pay_amt; //maker offer wants to sell this much token

1368:         uint256 id, //maker (ask) id

1369:         uint256 pos //position to insert into

1375:         uint256 prev_id; //maker (ask) id

1411:         uint256 id //id of maker (ask) offer to remove from sorted list

1418:             _rank[id].delb == 0 && //assert id is in the sorted list

1438:         _rank[id].delb = block.number; //mark _rank[id] for deletion

1444:         uint256 id //id of maker offer to remove from unsorted list

1446:         uint256 uid = _head; //id of an offer in unsorted offers list

1447:         uint256 pre = uid; //id of previous offer in unsorted offers list

1449:         require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list

1453:             _head = _near[id]; //set head to new first unsorted offer

1454:             _near[id] = 0; //delete order from unsorted order list

1466:         _near[pre] = _near[id]; //set previous unsorted offer to point to offer after offer id

1467:         _near[id] = 0; //delete order from unsorted order list

```

```solidity
File: periphery/BathBuddy.sol

55:     mapping(address => uint256) public lastUpdateTime; //Token specific

```
