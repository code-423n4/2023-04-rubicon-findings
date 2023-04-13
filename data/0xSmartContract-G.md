### Gas Optimizations List
| Number | Optimization Details | Context |
|:--:|:-------| :-----:|
| [G-01] |State variables only set in the constructor should be declared `immutable`| 4 |
| [G-02] |Structs can be packed into fewer storage slots| 6 |
| [G-03] |Pack state variables |1 |
| [G-04] |With assembly, `.call (bool success)` transfer can be done gas-optimized | 1 |
| [G-05] |Remove or replace unused state variables |2|
| [G-06] |Using ``delete``  instead of setting mapping/state variable ``0`` saves gas|3 |
| [G-07] |In 0.8>= pragma versions, the use of SafeMath is unnecessary | 2 |
| [G-08] | Avoid using ``state variable`` in emit |2 |
| [G-09] | Missing `zero-address` check in `constructor` | 5 |
| [G-10] |Gas overflow during iteration (DoS) | 4 |
| [G-11] | Multiple ``address``/ID mappings can be combined into a single ``mapping`` of an ``address``/ID to a ``struct``, where appropriate|15 |
| [G-12] |Multiple accesses of a mapping/array should use a local variable cache | 30 |
| [G-13] |The result of function calls should be cached rather than re-calling the function | 6 |
| [G-14] |Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead|2 |
| [G-15] |Avoid contract existence checks by using low level calls| 6 |
| [G-16] |Ternary operation is cheaper than if-else statement |2 |
| [G-17] |Change ``public`` function visibility to ``external`` | 9 |
| [G-18] |Do not calculate constants variables|1 |
| [G-19] |Use ``assembly`` to write _address storage values_ |7|
| [G-20] |Setting the _constructor_ to `payable` | 2 |
| [G-21] |Use a more recent version of solidity |6|
| [G-22] |Use ``double require`` instead of using ``&&``|6 |
| [G-23] |Use nested if and, avoid multiple check combinations | 1|
| [G-24] |Optimize names to save gas  | All contracts |
| [G-25] |``>=`` costs less gas than ``>`` | 2 |
| [G-26] |Functions guaranteed to revert_ when callled by normal users can be marked `payable`  | 8 |
| [G-27] |Upgrade Solidity's optimizer |  |

Total 27 issues

### [G-01] State variables only set in the constructor should be declared `immutable`

Avoids a Gsset (20000 gas) in the constructor and replaces the first access in each transaction (Gcoldsload - 2100 gas) and each access thereafter (Gwarmacces - 100 gas) with a PUSH32 (3 gas).

4 results - 1 file:
```diff
contracts\utilities\poolsUtility\Position.sol:

- 44:     Comptroller public comptroller;
+ 44:     Comptroller public immutable comptroller; 
- 45:     PriceOracle public oracle;
+ 45:     PriceOracle public immutable oracle;           
- 46:     RubiconMarket public rubiconMarket;
+ 46:     RubiconMarket public immutable rubiconMarket; 
- 47:     BathHouseV2 public bathHouseV2;
+ 47:     BathHouseV2 public immutable bathHouseV2; 

  54:     constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
  55:         oracle = PriceOracle(_oracle);
  56:         rubiconMarket = RubiconMarket(_rubiconMarket);
  57:         bathHouseV2 = BathHouseV2(_bathHouseV2);
  58:         comptroller = bathHouseV2.comptroller();
  59:     }

```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L54-L59


### [G-02] Structs can be packed into fewer storage slots

As the solidity EVM works with 32 bytes, variables less than 32 bytes should be packed inside a struct so that they can be stored in the same slot, this saves gas when writing to storage ~20000 gas.

6 results - 2 files:

|Title | Total Gas Saved | Instance|
|:--:|:-------:|:--:|
|Struct packed | 4k | 2 |

1. The ``CallParams`` structure can be packaged as suggested below  (from 4 slots to 3 slots)

    **1 slot win: 1 * 2k  =  2k gas saved**

```solidity
contracts\utilities\FeeWrapper.sol:
  12:     struct CallParams {
  13:         bytes4 selector; // function selector
  14:         bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
  15:         address target; // target contract to call
  16:         FeeParams feeParams;
  17:     }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L12-L17


```diff
contracts\utilities\FeeWrapper.sol:
  12:     struct CallParams {
  13:         bytes4 selector; // function selector
+ 15:         address target; // target contract to call
  14:         bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
- 15:         address target; // target contract to call
  16:         FeeParams feeParams;
  17:     }

```

2. The ``Position`` structure can be packaged as suggested below. (from 6 slots to 5 slots)

    **1 slot win: 1 * 2k  =  2k gas saved**

```solidity
contracts\utilities\poolsUtility\Position.sol:
  33:     struct Position {
  34:         address asset; // supplied as collateral
  35:         address quote; // borrowed token
  36:         uint256 borrowedAmount; // amount of borrowed quote
  37:         uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
  38:         uint256 blockNum; // block number on which position was opened
  39:         bool isActive; // false by default, when active - true
  40:     }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L33-L40


```diff
contracts\utilities\poolsUtility\Position.sol:
  33:     struct Position {
  34:         address asset; // supplied as collateral
+ 39:         bool isActive; // false by default, when active - true
  35:         address quote; // borrowed token
  36:         uint256 borrowedAmount; // amount of borrowed quote
  37:         uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
  38:         uint256 blockNum; // block number on which position was opened
- 39:         bool isActive; // false by default, when active - true
  40:     }

```

### [G-03] Pack state variables

1. State variable packaging in **RubiconMarket.sol** contract  (**1 * 2.1k = 2.1k gas saved**)

```diff
contracts\RubiconMarket.sol:
  219:     uint256 public last_offer_id;
  220: 
  221:     /// @dev The mapping that makes up the core orderbook of the exchange
  222:     mapping(uint256 => OfferInfo) public offers;
  223: 
- 224:     bool locked;
  225: 
  226:     /// @dev This parameter is in basis points
  227:     uint256 internal feeBPS;
  228: 
  229:     /// @dev This parameter provides the address to which fees are sent
  230:     address internal feeTo;
+ 224:     bool locked;
  231: 
  232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224

### [G-04] With assembly, `.call (bool success)` transfer can be done gas-optimized

`return` data `(bool success,)` has to be stored due to EVM architecture, but in a usage like below, 'out' and 'outsize' values are given (0,0), this storage disappears and gas optimization is provided.

https://twitter.com/pashovkrum/status/1607024043718316032?t=xs30iD6ORWtE2bTTYsCFIQ&s=19

There are 1 instance of the topic.

```diff
contracts\utilities\FeeWrapper.sol:

  117:    // transfer fee to the 3rd party protocol
- 118:    (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
+         address addr = payable(_feeTo);  
+         bool OK;
+         assembly {
+             sent := call(gas(), addr, _feeAmount, 0, 0, 0, 0)
+         }
  119: 		require(OK, "ETH transfer failed");
  120:    _msgValue = msg.value - _feeAmount;
  121: }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L117-L121


### [G-05] Remove or replace unused state variables

Saves a storage slot. If the variable is assigned a non-zero value, saves Gsset (20000 gas). If it's assigned a zero value, saves Gsreset (2900 gas). If the variable remains unassigned, there is no gas savings unless the variable is public, in which case the compiler-generated non-payable getter deployment cost is saved. If the state variable is overriding an interface's public function, mark the variable as constant or immutable so that it does not use a storage slot

2 results - 1 file:
```solidity
contracts\RubiconMarket.sol:
  

  682:     bool public AqueductDistributionLive;

  684:     address public AqueductAddress;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L681-L684


### [G-06] Using ``delete``  instead of setting mapping/state variable ``0`` saves gas

3 results - 2 files:
```solidity
contracts\RubiconMarket.sol:

  1449:    _near[id] = 0; //delete order from unsorted order list

  1462:    _near[id] = 0; //delete order from unsorted order list

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1449
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1462


```solidity
contracts\periphery\BathBuddy.sol:

  181:     tokenRewards[address(rewardsToken)][holderRecipient] = 0;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L181


**Recommendation code:**
```diff
contracts\periphery\BathBuddy.sol:

- 181:     tokenRewards[address(rewardsToken)][holderRecipient] = 0;
+ 181:     delete tokenRewards[address(rewardsToken)][holderRecipient];

```
### [G-07]  In 0.8>= pragma versions, the use of SafeMath is unnecessary

Although pragma version 0.8 and above is used in the project, OZ SafeMath library is used. This may increase the gas consumption of the contract.

```solidity
contracts\periphery\BathBuddy.sol:
  6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L6

```solidity
contracts\utilities\poolsUtility\Position.sol:
  6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L6



### [G-08] Avoid using ``state variable`` in emit
Using a state variable in ``SetOwner`` emits wastes gas.

There are two examples of the issue:

```solidity
contracts\RubiconMarket.sol:
  25:     function setOwner(address owner_) external auth {
  26:         owner = owner_;
  27:         emit LogSetOwner(owner);
  28:     }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L26-L27


```diff
contracts\RubiconMarket.sol:
  25:     function setOwner(address owner_) external auth {
  26:         owner = owner_;
- 27:         emit LogSetOwner(owner);
+ 27:         emit LogSetOwner(owner_);
  28:     }

```


```solidity
contracts\periphery\BathBuddy.sol:
  232:     function setRewardsDuration(
  233:         uint256 _rewardsDuration,
  234:         address token
  235:     ) external onlyOwner {
  236:         require(
  237:             block.timestamp > periodFinish[token],
  238:             "Previous rewards period must be complete before changing the duration for the new period"
  239:         );
  240:         rewardsDuration[token] = _rewardsDuration;
  241:         emit RewardsDurationUpdated(rewardsDuration[token]);
  242:     }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L240-L241


```diff
contracts\periphery\BathBuddy.sol:
  232:     function setRewardsDuration(
  233:         uint256 _rewardsDuration,
  234:         address token
  235:     ) external onlyOwner {
  236:         require(
  237:             block.timestamp > periodFinish[token],
  238:             "Previous rewards period must be complete before changing the duration for the new period"
  239:         );
  240:         rewardsDuration[token] = _rewardsDuration;
- 241:         emit RewardsDurationUpdated(rewardsDuration[token]);
+ 241:         emit RewardsDurationUpdated(_rewardsDuration);
  242:     }

```

###  [G-09] Missing `zero-address` check in `constructor`

There is no zero value check in the constructor. If these state variables are initialized with a possible value of "0", the contract must be redistributed. This possibility means gas consumption.Because of the EVM design, zero value control is the most error-prone value control, as zero value is assigned in case of no value input.

5 results - 2 files:
```solidity
contracts\BathHouseV2.sol:

  32:     function initialize(address _comptroller, address _pAdmin) external {
  34:         comptroller = Comptroller(_comptroller);
  36:         proxyAdmin = _pAdmin;
  39:     }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L34
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L36

```solidity
contracts\utilities\poolsUtility\Position.sol:

  54:     constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
  55:         oracle = PriceOracle(_oracle);
  56:         rubiconMarket = RubiconMarket(_rubiconMarket);
  57:         bathHouseV2 = BathHouseV2(_bathHouseV2);

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L55-L57


### [G-10] Gas overflow during iteration (DoS)

Each iteration of the cycle requires a gas flow. A moment may come when more gas is required than it is allocated to record one block. In this case, all iterations of the loop will fail.

4 results - 2 files:
```diff
contracts\BathHouseV2.sol:

+  uint256 private constant MAXBUDDIESLENGTH = 333; 

  115:     function claimRewards(
  116:         address[] memory buddies,
  117:         address[] memory rewardsTokens
  118:     ) external {
  119:         // claim rewards from comptroller
  120:         comptroller.claimComp(msg.sender);
  121:         // get rewards from bathBuddy
+              if (buddies.length > MAXBUDDIESLENGTH) revert maxLength();
  122:         for (uint256 i = 0; i < buddies.length; ++i) {
  123:             IBathBuddy(buddies[i]).getReward(
  124:                 IERC20(rewardsTokens[i]),
  125:                 msg.sender
  126:             );
  127:         }
  128:     }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L115-L128


```solidity
contracts\RubiconMarket.sol:
  886      /// @notice Batch offer functionality - multuple offers in a single transaction
  887:     function batchOffer(
  888:         uint[] calldata payAmts,
  889:         address[] calldata payGems,
  890:         uint[] calldata buyAmts,
  891:         address[] calldata buyGems
  892:     ) external {
  893:         require(
  894:             payAmts.length == payGems.length &&
  895:                 payAmts.length == buyAmts.length &&
  896:                 payAmts.length == buyGems.length,
  897:             "Array lengths do not match"
  898:         );
  899:         for (uint i = 0; i < payAmts.length; i++) {
  900:             this.offer(
  901:                 payAmts[i],
  902:                 ERC20(payGems[i]),
  903:                 buyAmts[i],
  904:                 ERC20(buyGems[i])
  905:             );
  906:         }
  907:     }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L887-L907


```solidity
contracts\RubiconMarket.sol:

  910:     function batchCancel(uint[] calldata ids) external {
  911:         for (uint i = 0; i < ids.length; i++) {
  912:             cancel(ids[i]);
  913:         }
  914:     }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L910-L914


```solidity
contracts\RubiconMarket.sol:

  917:     function batchRequote(
  918:         uint[] calldata ids,
  919:         uint[] calldata payAmts,
  920:         address[] calldata payGems,
  921:         uint[] calldata buyAmts,
  922:         address[] calldata buyGems
  923:     ) external {
  924:         for (uint i = 0; i < ids.length; i++) {
  925:             cancel(ids[i]);
  926:             this.offer(
  927:                 payAmts[i],
  928:                 ERC20(payGems[i]),
  929:                 buyAmts[i],
  930:                 ERC20(buyGems[i])
  931:             );
  932:         }
  933:     }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917-L933


### [G-11] Multiple ``address``/ID mappings can be combined into a single ``mapping`` of an ``address``/ID to a ``struct``, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.


15 results - 4 files:
```solidity
contracts\BathHouseV2.sol:

  20:     mapping(address => address) private tokenToBathToken;
  21:     mapping(address => address) private bathTokenToBuddy;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L20-L21


```solidity
contracts\RubiconMarket.sol:

  691:     mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
  692:     mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
  693:     mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L691-L693


```solidity
contracts\periphery\BathBuddy.sol:
 
  52:     mapping(address => uint256) public periodFinish; // Token specific
  53:     mapping(address => uint256) public rewardRates; // Token specific reward rates
  54:     mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific


  52:     mapping(address => uint256) public periodFinish; // Token specific
  53:     mapping(address => uint256) public rewardRates; // Token specific reward rates
  54:     mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
  55:     mapping(address => uint256) public lastUpdateTime; //Token specific
  56:     mapping(address => uint256) public rewardsPerTokensStored; // Token specific


  
  58:     // Token then user always
  59:     mapping(address => mapping(address => uint256))
  60:         public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
  61:     mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
 
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L52-L54

### [G-12] Multiple accesses of a mapping/array should use a local variable cache

The instances below point to the second+ access of a value inside a mapping/array, within a function. Caching a mapping's value in a local storage or calldata variable when the value is accessed multiple times, saves ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations. Caching an array's struct avoids recalculating the array offsets into memory/calldata.

30 results - 1 file:
```solidity
contracts\RubiconMarket.sol:

  // @audit offers[id]
  334:         offers[id].pay_amt = sub(_offer.pay_amt, quantity);
  335:         offers[id].buy_amt = sub(_offer.buy_amt, spend);

  436:         if (offers[id].pay_amt == 0) {



  // @audit _rank[id]
  942:                 _rank[id].delb != 0 &&
  943:                 _rank[id].delb < block.number - 10



  // @audit _rank[id], offers[id]
  1022:             _rank[id].next != 0 ||
  1023:             _rank[id].prev != 0 ||
  1024:             _best[address(offers[id].pay_gem)][address(offers[id].buy_gem)] ==


  // @audit offers[offerId]

  1045:                 wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)

  1049:             if (pay_amt >= offers[offerId].buy_amt) {
     
  1051:                 fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator
  1052:                 pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell
  1053:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

  1058:                     rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)


  // @audit offers[offerId]
  1085:                 wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)

  1089:             if (buy_amt >= offers[offerId].pay_amt) {
 
  1091:                 fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator
  1092:                 buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy
  1093:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

  1100:                         rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)


  // @audit offers[offerId]
  1130:         while (pay_amt > offers[offerId].buy_amt) {
  1131:             fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount to buy accumulator
  1132:             pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to pay

  1143:                 rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)


  // @audit offers[offerId]
  1163:         while (buy_amt > offers[offerId].pay_amt) {
  1164:             fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount to pay accumulator
  1165:             buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

  1176:                 rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)

  
  // @audit offers[id]
  1185:         if (amount == offers[id].pay_amt) {

  1199:             offers[id].pay_amt < _dust[address(offers[id].pay_gem)]
  1200:         ) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L334-L335

### [G-13] The result of function calls should be cached rather than re-calling the function

6 results - 2 files:
```solidity
contracts\utilities\poolsUtility\Position.sol:

  // @audit IERC20(asset).balanceOf()
  140:         vars.initAssetBalance = IERC20(asset).balanceOf(address(this));

  160:             uint256 assetBalance = IERC20(asset).balanceOf(address(this));


  // @audit IERC20(_bathToken).balanceOf()
  355:         uint256 _initBathTokenAmount = IERC20(_bathToken).balanceOf(
  356              address(this)

  363:         uint256 _currentBathTokenAmount = IERC20(_bathToken).balanceOf(
  364              address(this)

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L140

```solidity
contracts\periphery\BathBuddy.sol:
 
  // @audit IERC20(myBathTokenBuddy).totalSupply()
  124:         if (IERC20(myBathTokenBuddy).totalSupply() == 0) {

  133:                     .div(IERC20(myBathTokenBuddy).totalSupply())

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L124

### [G-14] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contracts gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html 

Use a larger size then downcast where needed.

2 results 2 files:
```solidity
contracts\BathHouseV2.sol:

  // @audit uint8 _decimals
  148:         _decimals = ERC20(_underlying).decimals();

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L148


```solidity
contracts\RubiconMarket.sol:

  // @audit uint64 block.timestamp
  625:         return uint64(block.timestamp);

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L625


### [G-15] Avoid contract existence checks by using low level calls

Prior to 0.8.10 the compiler inserted extra code, including ``EXTCODESIZE`` (**100 gas**), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence.

6 results - 2 files:
```solidity
contracts\RubiconMarket.sol:
  
  // @audit transferFrom()
  538:         require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L538

```solidity
contracts\periphery\BathBuddy.sol:

  // @audit totalSupply() 
  124:         if (IERC20(myBathTokenBuddy).totalSupply() == 0) {

  133:                     .div(IERC20(myBathTokenBuddy).totalSupply())


  // @audit balanceOf()
  146:             IERC20(myBathTokenBuddy) // Care with this?
  147:                 .balanceOf(account)


  // @audit safeTransfer()
  182:             rewardsToken.safeTransfer(holderRecipient, reward);

  // @audit balanceOf()
  216:         uint256 balance = rewardsToken.balanceOf(address(this));

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L124


### [G-16] Ternary operation is cheaper than if-else statement

There are instances where a ternary operation can be used instead of if-else statement. In these cases, using ternary operation will save modest amounts of gas.

2 results - 2 files:
```diff
contracts\RubiconMarket.sol:

  35:     function isAuthorized(address src) internal view returns (bool) {
- 36:         if (src == owner) {
- 37:             return true;
- 38:         } else {
- 39:             return false;
+             return (src == owner) ? true : false;
- 40:         }
  41:     }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35-L41


```diff
contracts\utilities\FeeWrapper.sol:

  48:     function rubicall(
  49:         CallParams memory params
  50:     ) external payable returns (bytes memory) {
- 51:         if (msg.value == 0) {
- 52:             return _rubicall(params);
- 53:         } else {
- 54:             return _rubicallPayable(params);
+             return (msg.value == 0) ? _rubicall(params) : _rubicallPayable(params);
  55:         }
  56:     }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L48-L56


### [G-17] Change ``public`` function visibility to ``external``

Since the following public functions are not called from within the contract, their visibility can be made external for gas optimization.

9 results - 2 files:
```solidity
contracts\BathHouseV2.sol:

  45:     function getBathTokenFromAsset(
  46:         address asset
  47:     ) public view returns (address) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L45-L49


```solidity
contracts\RubiconMarket.sol:

   288:     function getOffer(
   289:         uint256 id
   290:     ) public view returns (uint256, ERC20, uint256, ERC20) {

   574:     function getFeeBPS() public view returns (uint256) {

   624:     function getTime() public view returns (uint64) {

   700:     function initialize(address _feeTo) public {

   998:     function getOfferCount(
   999:         ERC20 sell_gem,
  1000:         ERC20 buy_gem
  1001:     ) public view returns (uint256) {

  1010:     function getFirstUnsortedOffer() public view returns (uint256) {

  1016:     function getNextUnsortedOffer(uint256 id) public view returns (uint256) {

  1149:     function getPayAmountWithFee(
  1150:         ERC20 pay_gem,
  1151:         ERC20 buy_gem,
  1152:         uint256 buy_amt
  1153:     ) public view returns (uint256 fill_amt) {

````
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L288-L293


### [G-18] Do not calculate constants variables

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.

1 result - 1 file:

```solidity
contracts\RubiconMarket.sol:
 
232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232


**Recommendation Code:**
```diff
- 232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

  // @audit keccak256("WOB_MAKER_FEE")
+ 232:     bytes32 internal constant MAKER_FEE_SLOT = 0x55a583bf144a5516c8de6ffe85b1859d93234d5e765ed3e895106330766044b8


```

### [G-19] Use ``assembly`` to write _address storage values_ 

7 results - 3 files:

```solidity
contracts\RubiconMarket.sol:

  25:     function setOwner(address owner_) external auth {
  26:         owner = owner_;

  700:     function initialize(address _feeTo) public {
  705:         feeTo = _feeTo;

  1476:     function setFeeTo(address newFeeTo) external auth returns (bool) {
  1478:         feeTo = newFeeTo;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L26


```solidity
contracts\BathHouseV2.sol:

  32:     function initialize(address _comptroller, address _pAdmin) external {
  36:         proxyAdmin = _pAdmin;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L36


```solidity
contracts\periphery\BathBuddy.sol:

  71:     function spawnBuddy(
  77:         owner = _owner;
  78:         myBathTokenBuddy = newBud;
  79:         bathHouse = _bathHouse;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L77-L79


**Recommendation Code:**
```diff

contracts\periphery\BathBuddy.sol:

  71:     function spawnBuddy(
- 77:         owner = _owner;
+                  assembly {                      
+                      sstore(owner .slot, _owner)
+                  }                               
          
```

### [G-20] Setting the _constructor_ to `payable`

You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of ```msg.value == 0``` and saves ```13 gas``` on deployment with no security risks.

2 results - 2 files:

```solidity
contracts\V2Migrator.sol:

  30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30

```solidity
contracts\utilities\poolsUtility\Position.sol:

  54:     constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L54


**Recommendation:**
Set the constructor to ```payable```


### [G-21] Use a more recent version of solidity

> Solidity 0.8.10 has a useful change that reduced gas costs of external calls which expect a return value. 
> 
> In 0.8.15 the conditions necessary for inlining are relaxed. Benchmarks show that the change significantly decreases the bytecode size (which impacts the deployment cost) while the effect on the runtime gas usage is smaller.
> 
> In 0.8.17 prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call; Simplify the starting offset of zero-length operations to zero. More efficient overflow checks for multiplication.
> 
> In 0.8.18 Allow replacing the previously hard-coded cleanup sequence by specifying custom steps after a colon delimiter (:) in the sequence string. Eliminate keccak256 calls if the value was already calculated by a previous call and can be reused.


6 results - 6 files:
```solidity
contracts\BathHouseV2.sol:

  2: pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L2


```solidity
contracts\RubiconMarket.sol:

  2: pragma solidity ^0.8.9;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2


```solidity
contracts\V2Migrator.sol:

  2: pragma solidity 0.8.17;
 
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L2


```solidity
contracts\periphery\BathBuddy.sol:

  2: pragma solidity ^0.8.0;
 
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2


```solidity
contracts\utilities\FeeWrapper.sol:

  2: pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L2


```solidity
contracts\utilities\poolsUtility\Position.sol:

  2: pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L2


### [G-22] Use ``double require`` instead of using ``&&``

Using double require instead of operator && can save more gas
When having a require statement with 2 or more expressions needed, place the expression that cost less gas first.

6 results - 2 files:
```solidity
contracts\RubiconMarket.sol:

   253:         require(
   254:             (msg.sender == getOwner(id)) ||
   255:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))
   256:         ); 
   

   612:         require(
   613:             (msg.sender == getOwner(id)) ||
   614:                 isClosed() ||
   615:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))
   616          );

   721:         require(
   722:             isClosed() ||
   723:                 msg.sender == getOwner(id) ||
   724:                 id == dustId ||
   725:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
   726:             "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
   727:         );
  
  940:         require(
  941:             !isActive(id) &&
  942:                 _rank[id].delb != 0 &&
  943:                 _rank[id].delb < block.number - 10
  944:         );

  1412:         require(
  1413:             _rank[id].delb == 0 && //assert id is in the sorted list
  1414:                 isOfferSorted(id)
  1415:         );

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L253-L256


```solidity
contracts\utilities\poolsUtility\Position.sol:

   591:             ? require(
   592:                 _leverage > _wad && _leverage <= _leverageMax,
   593:                 "_leverageCheck{Long}: INVLAID LEVERAGE"
   594:             )

   595:             : require(
   596:                 _leverage >= _wad && _leverage <= _leverageMax,
   597:                 "_leverageCheck{Short}: INVLAID LEVERAGE"
   598:             );
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L591-L598


**Recommendation Code:**
 ```diff
contracts\RubiconMarket.sol:

-  253:         require(
-  254:             (msg.sender == getOwner(id)) ||
-  255:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))
-  256:         );

+ 253:         require(
+ 254:             (msg.sender == getOwner(id)) ||
+ 255:                 (msg.sender == getRecipient(id)));
+               require(getOwner(id) == address(0)); 

```

### [G-23] Use nested if and, avoid multiple check combinations

Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

1 result - 1 file:
```solidity
contracts\RubiconMarket.sol:

  349:             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349


**Recomendation Code:**
```diff
contracts\RubiconMarket.sol:

- 349:             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
+ 349:             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {


**Recomendation Code:**
```diff
contracts\RubiconMarket.sol:

- 349:             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
+ 349:             if (_offer.owner == address(0)) {
+                         if (getRecipient(id) != address(0)) {

```

### [G-24] Optimize names to save gas

Contracts most called functions could simply save gas by function ordering via ```Method ID```. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because ```22 gas``` are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

**Context:** 
All Contracts


**Recommendation:** 
Find a lower ```method ID``` name for the most called functions for example Call() vs. Call1() is cheaper by ```22 gas```
For example, the function IDs in the ```Position.sol``` contract will be the most used; A lower method ID may be given.

**Proof of Consept:**
https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

Position.sol function names can be named and sorted according to METHOD ID

```js
Sighash   |   Function Signature
========================
4d73e9ba  =>  borrowBalance(address)
cc1b34d2  =>  borrowBalanceOfPos(uint256)
08c728e1  =>  borrowRate(address)
0a2636e7  =>  buyAllAmountWithLeverage(address,address,uint256,uint256)
f00ae278  =>  sellAllAmountWithLeverage(address,address,uint256,uint256)
6ef1a28f  =>  openPosition(address,address,uint256,uint256)
a126d601  =>  closePosition(uint256)
4fa36084  =>  increaseMargin(uint256,uint256)
f3fef3a3  =>  withdraw(address,uint256)
2b4b1080  =>  _borrowLoop(address,address,address,address,uint256,uint256)
c7a0b618  =>  _borrow(address,uint256)
83b9d7f9  =>  _repay(address,address,uint256)
63eb3355  =>  _maxBorrow(address)
c2b2c9a4  =>  _calculateDebt(address,uint256,uint256)
bcbfcdb7  =>  _enterMarkets(address)
2f3d95d9  =>  _exitMarket(address)
c5f03615  =>  _supply(address,address,uint256)
4dfad511  =>  _redeem(address,uint256)
ec38f2b3  =>  _savePosition(address,address,uint256,uint256)
df093a05  =>  _removePosition(uint256)
f5f4ac4a  =>  _updateMargin(uint256,uint256,uint256)
b7dafe46  =>  _rubiconSwap(address,address,uint256,bool)
882c0110  =>  _marketBuy(address,address,uint256)
024d7eb7  =>  _marketSell(address,address,uint256)
db79202c  =>  _borrowLimit(address,address,uint256,uint256)
c72663a3  =>  _leverageCheck(uint256,bool)

```

### [G-25] ``>=`` costs less gas than ``>``

The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=, which saves 3 gas

2 results - 2 files:
```solidity
contracts\RubiconMarket.sol:

  1300:             if (
  1301:                 mul(m_buy_amt, t_buy_amt) >
  1302:                 mul(t_pay_amt, m_pay_amt) +
  1303:                     (
  1304:                         rounding
  1305:                             ? m_buy_amt + t_buy_amt + t_pay_amt + m_pay_amt
  1306:                             : 0
  1307:                     )
  1308:             ) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1300-L1308


```solidity
contracts\periphery\BathBuddy.sol:
  115:         return
  116:             block.timestamp < periodFinish[token]
  117:                 ? block.timestamp
  118:                 : periodFinish[token];

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L115-L119

### [G-26]  Functions guaranteed to revert_ when callled by normal users can be marked `payable` 

If a function modifier or require such as onlyOwner-admin is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.


8 results - 3 files:
```solidity
contracts\BathHouseV2.sol:

  66:     ) external onlyAdmin {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L66


```solidity
contracts\periphery\BathBuddy.sol:

  194:     ) external onlyOwner updateReward(address(0), address(rewardsToken)) {

  235:     ) external onlyOwner {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L194


```solidity
contracts\utilities\poolsUtility\Position.sol:

   98:     ) external onlyOwner {

  112:     ) external onlyOwner {

  210:     function closePosition(uint256 posId) external onlyOwner {

  226:     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

  242:     function withdraw(address token, uint256 amount) external onlyOwner {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L98


**Recommendation:**

Functions guaranteed to revert when called by normal users can be marked payable  (for only ```onlyAdmin```, ``onlyOwner``, ``onlyBuddy``, ``onlyBathHouse``, ``onlyBathHouse`` modifiers)

### [G-27] Upgrade Solidity's optimizer

Make sure Solidity’s optimizer is enabled. It reduces gas costs. If you want to gas optimize for contract deployment (costs less to deploy a contract) then set the Solidity optimizer at a low number. If you want to optimize for run-time gas costs (when functions are called on a contract) then set the optimizer to a high number.


```js
hardhat.config.ts:
  12:     compilers: [
  13:       {
  14:         version: "0.8.17",
  15:         settings: {
  16:           optimizer: {
  17:             enabled: true,
  18:             runs: 5,
  19:           },
  20:         },
  21:       },

```