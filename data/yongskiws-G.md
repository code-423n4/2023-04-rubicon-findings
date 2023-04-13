## Summary

### Gas Optimizations Issues List
| Number |Issues Details|Instance|
|:--:|:-------|:--:|
|[G-01]| Using unchecked blocks to save gas | 19 |
|[G-02]| Use calldata instead of memory for function parameters | 3 |
|[G-03]| Using delete statement can save gas | 1 |
|[G-04]| Functions guaranteed to revert when called by normal users can be marked payable | 7 |
|[G-05]| Use hardcode address instead address(this) | 19 |
|[G-06]| Sort Solidity operations using short-circuit mode | 6 |
|[G-07]| Using unchecked blocks to save gas - Increments in for loop can be unchecked ( save 30-40 gas per loop iteration) | 6 |
|[G-08]| Using storage instead of memory for structs/arrays saves gas | 10 |
|[G-09]| fulfill(): requests[_positionId] should be cached in local storage | 1 |
|[G-10]| Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate | 15 |
|[G-11]| Using private rather than public for constants, saves gas | - |
|[G-12]| Using immutable for uint & address for save gas | - |
|[G-13]| internal functions not called by the contract should be removed to save deployment gas | 6 |
|[G-14]| x = x + y is cheaper than x += y; | 3 |
|[G-15]| Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead | - |
|[G-16]| Using > 0 costs more gas than != 0 when used on a uint in a require() statement | 16 |
|[G-17]| >= costs less gas than > | 11 |


## [G-01] Using unchecked blocks to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block
https://github.com/ethereum/solidity/issues/10695

Gas Saved For Using unchecked blocks to save gas

The above should be modified to:

19 results - 4 files:


``` diff

Position.sol
+     unchecked{
407:         lastPositionId++;
+     }

+     unchecked{
572:      _limit++;
+     }

+     unchecked{
576:   _limit++;
+     }

+     unchecked{
580:                 _limit++;
+     }


BathBuddy.sol
+     unchecked{
115:         return
116:             block.timestamp < periodFinish[token]
117:                 ? block.timestamp
118:                 : periodFinish[token];
+     }

Position.sol
+     unchecked{
449:         _buy
450:             ? _marketBuy(_asset, _quote, _fillLimit)
451:             : _marketSell(_asset, _quote, _fillLimit);
+     }

+     unchecked{
587:         uint256 _wad = WAD;
588:         uint256 _leverageMax = WAD.mul(3);
589: 
590:         _long // long can't be with 1x leverage
591:             ? require(
592:                 _leverage > _wad && _leverage <= _leverageMax,
593:                 "_leverageCheck{Long}: INVLAID LEVERAGE"
594:             )
595:             : require(
596:                 _leverage >= _wad && _leverage <= _leverageMax,
597:                 "_leverageCheck{Short}: INVLAID LEVERAGE"
598:             );
+     }

RubiconMarket.sol

46:     function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
+     unchecked{
47:         require((z = x + y) >= x, "ds-math-add-overflow");
+     }
48:     }
49: 
50:     function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {
+     unchecked{  
51:         require((z = x - y) <= x, "ds-math-sub-underflow");
+     }
52:     }
53: 
54:     function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
+     unchecked{ 
55:         require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");
+     }
56:     }
57: 
58:     function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
+     unchecked{ 
59:         return x <= y ? x : y;
+     }
60:     }
61: 
62:     function max(uint256 x, uint256 y) internal pure returns (uint256 z) {
+     unchecked{ 
63:         return x >= y ? x : y;
+     }
64:     }
65: 
66:     function imin(int256 x, int256 y) internal pure returns (int256 z) {
+     unchecked{ 
67:         return x <= y ? x : y;
+     }
68:     }
69: 
70:     function imax(int256 x, int256 y) internal pure returns (int256 z) {
+     unchecked{ 
71:         return x >= y ? x : y;
+     }
72:     }

+     unchecked{ 
458:         /// @dev V1 orders after V2 upgrade will point to address(0) in owner
459:         _offer.owner == address(0) && msg.sender == _offer.recipient
460:             ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))
461:             : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));
+     }

+     unchecked{ 
1304:                         rounding
1305:                             ? m_buy_amt + t_buy_amt + t_pay_amt + m_pay_amt
1306:                             : 0
+     }

+     unchecked{
569:         last_offer_id++;
570:         return last_offer_id;
+     }

+     unchecked{
1400:         _span[address(pay_gem)][address(buy_gem)]++;
+     }



FeeWrapper.sol
+     unchecked{ 
120:         _msgValue = msg.value - _feeAmount;
+     }

```


## [G-02] Use calldata instead of memory for function parameters

In some cases, having function arguments in calldata instead of memory is more optimal.

Consider the following generic example:

``` solidity
contract C {
    function add(uint[] memory arr) external returns (uint sum) {
        uint length = arr.length;
        for (uint i = 0; i < arr.length; i++) {
            sum += arr[i];
        }
    }
}
```

In the above example, the dynamic array arr has the storage location memory. When the function gets called externally, the array values are kept in calldata and copied to memory during ABI decoding (using the opcode calldataload and mstore). And during the for loop, arr[i] accesses the value in memory using a mload. However, for the above example this is inefficient. Consider the following snippet instead:

``` solidity
contract C {
    function add(uint[] calldata arr) external returns (uint sum) {
        uint length = arr.length;
        for (uint i = 0; i < arr.length; i++) {
            sum += arr[i];
        }
    }
}
```

In the above snippet, instead of going via memory, the value is directly read from calldata using calldataload. That is, there are no intermediate memory operations that carries this value.

Gas savings: In the former example, the ABI decoding begins with copying value from calldata to memory in a for loop. Each iteration would cost at least 60 gas. In the latter example, this can be completely avoided. This will also reduce the number of instructions and therefore reduces the deploy time cost of the contract.

In short, use calldata instead of memory if the function argument is only read.

Note that in older Solidity versions, changing some function arguments from memory to calldata may cause "unimplemented feature error". This can be avoided by using a newer (0.8.*) Solidity compiler.

Examples Note: The following pattern is prevalent in the codebase:

``` solidity
function f(bytes memory data) external {
    (...) = abi.decode(data, (..., types, ...));
}
```

Here, changing to bytes calldata will decrease the gas. The total savings for this change across all such uses would be quite significant.

3 results - 3 files:


``` solidity
BathHouseV2.sol
114: 
115:     function claimRewards(
116:         address[] memory buddies,
117:         address[] memory rewardsTokens
118:     ) external {
119:         // claim rewards from comptroller
120:         comptroller.claimComp(msg.sender);
121:         // get rewards from bathBuddy
122:         for (uint256 i = 0; i < buddies.length; ++i) {
123:             IBathBuddy(buddies[i]).getReward(
124:                 IERC20(rewardsTokens[i]),
125:                 msg.sender
126:             );
127:         }
128:     }


FeeWrapper.sol
28:     function calculateFee(
29:         uint256[] memory tokenAmounts,
30:         uint256 feeType,
31:         uint256 feeValue
32:     )
33:         external
34:         view
35:         returns (uint256[] memory amountsWithFee, uint256[] memory fees)
36:     {
37:         amountsWithFee = new uint256[](tokenAmounts.length);
38:         fees = new uint256[](tokenAmounts.length);
39: 
40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {
41:             fees[i] = (tokenAmounts[i] * feeValue) / feeType;
42:             amountsWithFee[i] = tokenAmounts[i] - fees[i];
43:         }
44:     }

V2Migrator.sol
30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
31:         uint256 i = 0; i < bathTokensV1.length; ++i) {
32:             // set v1 to v2 bathTokens
33:             v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
34:         }
35:     }
```

## [G-03] Using delete statement can save gas

1 results - 1 files:

``` solidity
BathBuddy.sol
181:         tokenRewards[address(rewardsToken)][holderRecipient] = 0;
```

## [G-04] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier or require such as onlyOwner/onlyX is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

7 results - 2 files:

``` solidity
BathBuddy.sol
191:   function notifyRewardAmount(
192:         uint256 reward,
193:         IERC20 rewardsToken
194:     ) external onlyOwner


232:     function setRewardsDuration(
233:         uint256 _rewardsDuration,
234:         address token
235:     ) external onlyOwner

Position.sol
93:     function buyAllAmountWithLeverage(
94:         address quote,
95:         address asset,
96:         uint256 quotePayAmount,
97:         uint256 leverage
98:     ) external onlyOwner


107:     function sellAllAmountWithLeverage(
108:         address asset,
109:         address quote,
110:         uint256 assetPayAmount,
111:         uint256 leverage
112:     ) external onlyOwner

210:     function closePosition(uint256 posId) external onlyOwner {

226:     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242:     function withdraw(address token, uint256 amount) external onlyOwner {
```


## [G-05] Use hardcode address instead address(this)


Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address. Foundry's script.sol and solmate's LibRlp.sol contracts can help achieve this.


References: https://book.getfoundry.sh/reference/forge-std/compute-create-address

https://twitter.com/transmissions11/status/1518507047943245824

19 results - 6 files:

``` solidity
BathBuddy.sol
216:   uint256 balance = rewardsToken.balanceOf(address(this));

BathHouseV2.sol
104:         buddy.spawnBuddy(admin, bathToken, address(this));

FeeWrapper.sol
100:         IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);

Position.sol
66:         balance = CTokenInterface(bathToken).borrowBalanceCurrent(
67:             address(this)
68:         );
139:         /// @dev save initial balance of asset to calculate then amount to borrow
140:         vars.initAssetBalance = IERC20(asset).balanceOf(address(this));
141: 
142:         // transfer initial margin amount
143:         IERC20(asset).safeTransferFrom(msg.sender, address(this), initMargin);
144: 
160:             uint256 assetBalance = IERC20(asset).balanceOf(address(this));
161: 
233:         IERC20(asset).safeTransferFrom(msg.sender, address(this), amount);
234: 
286:         uint256 _quoteBalance = IERC20(_quote).balanceOf(address(this));
287: 
355:         uint256 _initBathTokenAmount = IERC20(_bathToken).balanceOf(
356:             address(this)
357:         );
363:         uint256 _currentBathTokenAmount = IERC20(_bathToken).balanceOf(
364:             address(this)
365:         );
487:        uint256 _assetBalance = IERC20(_asset).balanceOf(address(this));
488: 
537:         uint256 _minted = IERC20(_bathToken).balanceOf(address(this));

RubiconMarket.sol
538:         require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));
539: 

V2Migrator.sol
44:         bathTokenV1.transferFrom(msg.sender, address(this), bathBalance);
45: 
59:         IERC20(bathTokenV2).transfer(
60:             msg.sender,
61:             IERC20(bathTokenV2).balanceOf(address(this))
62:         );
63:         require(
64:             IERC20(bathTokenV2).balanceOf(address(this)) == 0,
65:             "migrate: BATH TOKENS V2 STUCK IN THE CONTRACT"
66:         );
67: 
```

## [G-06] Sort Solidity operations using short-circuit mode

Short-circuiting is a solidity contract development model that uses OR/AND logic to sequence different cost operations. It puts low gas cost operations in the front and high gas cost operations in the back, so that if the front is low If the cost operation is feasible, you can skip (short-circuit) the subsequent high-cost Ethereum virtual machine operation.

6 results - 1 files:


``` solidity
RubiconMarket.sol
253:         require(
254:             (msg.sender == getOwner(id)) ||
255:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))
256:         );

325:        if (
326:             quantity == 0 ||
327:             spend == 0 ||
328:             quantity > _offer.pay_amt ||
329:             spend > _offer.buy_amt
330:         ) {
331:             return false;
332:         }

612:         require(
613:             (msg.sender == getOwner(id)) ||
614:                 isClosed() ||
615:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))
616:         );

612:         require(
613:             (msg.sender == getOwner(id)) ||
614:                 isClosed() ||
615:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))
616:         );

721:         require(
722:             isClosed() ||
723:                 msg.sender == getOwner(id) ||
724:                 id == dustId ||
725:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
726:             "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
727:         );

1021:         return
1022:             _rank[id].next != 0 ||
1023:             _rank[id].prev != 0 ||
1024:             _best[address(offers[id].pay_gem)][address(offers[id].buy_gem)] ==
1025:             id;

1319:             if (t_pay_amt == 0 || t_buy_amt == 0) {
1320:                 break;
1321:             }
```

## [G-07] Using unchecked blocks to save gas - Increments in for loop can be unchecked ( save 30-40 gas per loop iteration)

The majority of Solidity for loops increment a uint256 variable that starts at 0. These increment operations never need to be checked for over/underflow because the variable will never reach the max number of uint256 (will run out of gas long before that happens). The default over/underflow check wastes gas in every iteration of virtually every for loop . eg.

e.g Let's work with a sample loop below.
``` solidity
for(uint256 i; i < 10; i++){
//doSomething
}
```
can be written as shown below.
``` diff
for(uint256 i; i < 10;) {
  // loop logic
+  unchecked { i++; }
}
```
We can also write it as an inlined function like below.
``` solidity
function inc(i) internal pure returns (uint256) {
  unchecked { return i + 1; }
}
for(uint256 i; i < 10; i = inc(i)) {
  // doSomething
}
```

The above should be modified to:

6 results - 4 files:

``` diff
BathHouseV2.sol
-122:         for (uint256 i = 0; i < buddies.length; ++i) {
+122:         for (uint256 i = 0; i < buddies.length;) {
123:             IBathBuddy(buddies[i]).getReward(
124:                 IERC20(rewardsTokens[i]),
125:                 msg.sender
126:             );
+             unchecked {
+                ++i;
+             }
127:         }


FeeWrapper.sol
-40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {
+40:         for (uint256 i = 0; i < tokenAmounts.length;) {
41:             fees[i] = (tokenAmounts[i] * feeValue) / feeType;
42:             amountsWithFee[i] = tokenAmounts[i] - fees[i];
+             unchecked {
+                ++i;
+             }
43:         }


Position.sol
-158:         for (uint256 i = 0; i < vars.limit; ++i) {
+158:         for (uint256 i = 0; i < vars.limit;) {
.....
+             unchecked {
+                ++i;
+             }
192:         }

RubiconMarket.sol
-899:         for (uint i = 0; i < payAmts.length; i++) {
+899:         for (uint i = 0; i < payAmts.length;) {
900:             this.offer(
901:                 payAmts[i],
902:                 ERC20(payGems[i]),
903:                 buyAmts[i],
904:                 ERC20(buyGems[i])
905:             );
+             unchecked {
+                ++i;
+             }
906:         }

-911:         for (uint i = 0; i < ids.length; i++) {
+911:         for (uint i = 0; i < ids.length;) {
912:             cancel(ids[i]);
+             unchecked {
+                ++i;
+             }
913:         }

-924:         for (uint i = 0; i < ids.length; i++) {
+924:         for (uint i = 0; i < ids.length;) {  
925:             cancel(ids[i]);
926:             this.offer(
927:                 payAmts[i],
928:                 ERC20(payGems[i]),
929:                 buyAmts[i],
930:                 ERC20(buyGems[i])
931:             );
+             unchecked {
+                ++i;
+             }
932:         }

```

## [G-08] Using storage instead of memory for structs/arrays saves gas

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

10 results - 4 files:

``` solidity

FeeWrapper.sol
37:         amountsWithFee = new uint256[](tokenAmounts.length);
38:         fees = new uint256[](tokenAmounts.length);

BathHouseV2.sol
82:         (string memory name, string memory symbol, uint8 decimals) = _bathify(
83:             underlying
84:         );

Position.sol
75:     Position memory pos = positions[posId];

135:         PosVars memory vars;

211:         Position memory pos = positions[posId];

227:         Position memory pos = positions[posId];

337:         address[] memory _bathTokens = new address[](1);
338:         _bathTokens[0] = _bathToken;
339:         uint256[] memory _errs = comptroller.enterMarkets(_bathTokens);

RubiconMarket.sol
291:   OfferInfo memory _offer = offers[id];

455:   OfferInfo memory _offer = offers[id];

527:   OfferInfo memory info;

```

## [G-09] fulfill(): requests[_positionId] should be cached in local storage
1 results - 1 files:
``` soliditiy
Position.sol
431:         positions[_positionId].bathTokenAmount += _bathTokenAmount;
```

## [G-10] Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas)

15 results - 4 files:

``` solidity
BathBuddy.sol
51:     /// @dev set to block.timestamp + rewards duration to track an active rewards period after notifyRewardAmount()
52:     mapping(address => uint256) public periodFinish; // Token specific
53:     mapping(address => uint256) public rewardRates; // Token specific reward rates
54:     mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
55:     mapping(address => uint256) public lastUpdateTime; //Token specific
56:     mapping(address => uint256) public rewardsPerTokensStored; // Token specific
57: 
58:     // Token then user always
59:     mapping(address => mapping(address => uint256))
60:         public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
61:     mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
62: 
BathHouseV2.sol
20:     mapping(address => address) private tokenToBathToken;
21:     mapping(address => address) private bathTokenToBuddy;


Position.sol
42:     mapping(uint256 => Position) public positions;

RubiconMarket.sol
691:     mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
692:     mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
693:     mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook
694:     mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
695:     mapping(uint256 => uint256) public _near; //next unsorted offer id
```

## [G-11] Using private rather than public for constants, saves gas

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

There are ALL CONTRACT instances of this issue eg. :

``` diff
- uint256 public override example;
+ uint256 private override example;
```

## [G-12] Using immutable for uint & address for save gas
Avoids a Gsset (20000 gas) in the constructor, and replaces the first access in each transaction (Gcoldsload - 2100 gas) and each access thereafter (Gwarmacces - 100 gas) with a PUSH32 (3 gas).

While strings are not value types, and therefore cannot be immutable/constant if not hard-coded outside of the constructor, the same behavior can be achieved by making the current contract abstract with virtual functions for the string accessors, and having a child contract override the functions with the hard-coded implementation-specific values.

There are ALL CONTRACT instances of this issue eg. :

``` diff
- address public override example;
+ address public immutable override example;
```

## [G-13] internal functions not called by the contract should be removed to save deployment gas
If the functions are required by an interface, the contract should inherit from that interface and use the override keyword

6 results - 2 files:

``` solidity
Position.sol
306:     function _maxBorrow(
307:         address _bathToken
308:     ) internal view returns (uint256 _max) {

322:     function _calculateDebt(
323:         address _bathToken,
324:         uint256 _startBlock,
325:         uint256 _borrowedAmount
326:     ) internal view returns (uint256 _debt) {

RubiconMarket.sol
35:     function isAuthorized(address src) internal view returns (bool) {

1208:     function _find(uint256 id) internal view returns (uint256) {

1225:     function _findpos(uint256 id, uint256 pos) internal view returns (uint256) {

1264:     ) internal view returns (bool) {

```

## [G-14] x = x + y is cheaper than x += y;

3 results - 1 files:

``` solidity
Position.sol
184:             vars.currentBathTokenAmount += _borrowLoop(
185:                 asset,
186:                 quote,
187:                 bathTokenAsset,
188:                 bathTokenQuote,
189:                 vars.currentAssetBalance,
190:                 vars.toBorrow
191:             );

431:         positions[_positionId].bathTokenAmount += _bathTokenAmount;

560:             _assetAmount += _loopBorrowed;

```

## [G-15] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Use a larger size then downcast where needed


## [G-16] Using > 0 costs more gas than != 0 when used on a uint in a require() statement

The optimization works until solidity version 0.8.13 where there is a regression in gas costs.

11 results - 4 files:

``` solidity


FeeWrapper.sol
51:         if (msg.value == 0) {

Position.sol
163:     if (vars.initAssetBalance == 0) {

542:    if (_limit == 0) {

RubiconMarket.sol
436:         if (offers[id].pay_amt == 0) {

1233:         if (pos == 0) {

1319:             if (t_pay_amt == 0 || t_buy_amt == 0) {
1320:                 break;
1321:             }

345:    if (makerFee() > 0) {

585:    if (makerFee() > 0) {

1133:   if (pay_amt > 0) {

1166:   if (buy_amt > 0) {

BathBuddy.sol

180:       if (reward > 0) {
124:       if (IERC20(myBathTokenBuddy).totalSupply() == 0) {

```


## [G-17] >= costs less gas than >

The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=

5 results - 2 files:

``` solidity

BathBuddy.sol
180:       if (reward > 0) {
  
236:      require(
237:             block.timestamp > periodFinish[token],
238:             "Previous rewards period must be complete before changing the duration for the new period"
239:         );

Position.sol
293:      if (_amountToRepay > _borrowBalance) {
294:             _amountToRepay = _borrowBalance;
295:         }

313:         require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");

318:         require(_max > 0, "_maxBorrow: can't borrow 0");

562:             if (_assetAmount > _desiredAmount) {
```

