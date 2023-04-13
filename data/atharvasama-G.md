# Gas Optimizations list

| Number | Details                                                                                           | Instances |
| ------ | ------------------------------------------------------------------------------------------------- | --------- |
| 1      | ```x += y```/```x -= y``` COSTS MORE GAS THAN ```x = x + y```/```x = x - y``` FOR STATE VARIABLES | 2         |
| 2      | CAN DECLARE VARIABLE OUTSIDE LOOP TO SAVE GAS                                                     | 1         |
| 3      | MULTIPLE ADDRESS/ID MAPPINGS CAN BE COMNBINED INTO A SINGLE MAPPING TO A STRUCT                   | 8         |
| 4      | AVOID CONTRACT EXISTENCE CHECKS BY USING LOW LEVEL CALLS                                          | 7         |
| 5      | PUBLIC FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE DECLARED EXTERNAL INSTEAD                   | 10        |
| 6      | OPTIMIZE NAMES TO SAVE GAS                                                                        | 6         |
| 7      | REORDER STRUCTURE LAYOUT                                                                          | 2         |
| 8      | SHOULD USE LOCAL ARGUMENTS INSTEAD OF STATE VARIABLE                                              | 3         |
                                                   

# Gas Optimizations
## [G-01]  ```x += y```/```x -= y``` COSTS MORE GAS THAN ```x = x + y```/```x = x - y``` FOR STATE VARIABLES
Using the addition operator instead of plus-equals saves some gas for state variables.

contracts\utilities\poolsUtility\Position.sol
```js
431:         positions[_positionId].bathTokenAmount += _bathTokenAmount;
```

contracts\RubiconMarket.sol
```js
583:         _amount -= mul(amount, feeBPS) / 100_000;
```

## [G-02] CAN DECLARE VARIABLE OUTSIDE LOOP TO SAVE GAS 
Declaring the stack variable outside the loop will save gas that would otherwise be spent on declaring the variable over and over again.     

contracts\utilities\poolsUtility\Position.sol
```js
160:             uint256 assetBalance = IERC20(asset).balanceOf(address(this));
```

## [G-03] MULTIPLE ADDRESS/ID MAPPINGS CAN BE COMNBINED INTO A SINGLE MAPPING TO A STRUCT
If both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations. Depending on the circumstances and sizes of types, can avoid a Gsset per mapping combined.

contracts\RubiconMarket.sol
```js
692:     mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
693:     mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook
694:     mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
```

contracts\periphery\BathBuddy.sol
```js
52:     mapping(address => uint256) public periodFinish; // Token specific
53:     mapping(address => uint256) public rewardRates; // Token specific reward rates
54:     mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
55:     mapping(address => uint256) public lastUpdateTime; //Token specific
56:     mapping(address => uint256) public rewardsPerTokensStored; // Token specific
```

## [G-04] AVOID CONTRACT EXISTENCE CHECKS BY USING LOW LEVEL CALLS
Before Solidity 0.8.10 the compiler inserted extra code to check for contract existence for external function calls ```EXTCODESIZE``` (100 gas). In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence.

contracts\RubiconMarket.sol
```js
340:             _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),

350:                 require(
351:                     _offer.buy_gem.transferFrom(
352:                         msg.sender,
353:                         _offer.recipient,
354:                         mFee
355:                     ),
356:                     "Insufficient funds to cover fee"
357:                 );

375:             _offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend),

380:             _offer.pay_gem.transfer(msg.sender, quantity),

460:             ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))
461:             : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));

538:         require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));
```
## [G-05] PUBLIC FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE DECLARED EXTERNAL INSTEAD
Contracts are allowed to override their parents’ functions and change the visibility from public to external and can save gas by doing so.

contracts\BathHouseV2.sol
```js
45:     function getBathTokenFromAsset(
46:         address asset
47:     ) public view returns (address) {
```

contracts\RubiconMarket.sol
```js
288:     function getOffer(
289:         uint256 id
290:     ) public view returns (uint256, ERC20, uint256, ERC20) {

574:     function getFeeBPS() public view returns (uint256) {

624:     function getTime() public view returns (uint64) {

700:     function initialize(address _feeTo) public {

0998:     function getOfferCount(
0999:         ERC20 sell_gem,
1000:         ERC20 buy_gem
1001:     ) public view returns (uint256) {

1010:     function getFirstUnsortedOffer() public view returns (uint256) {

1016:     function getNextUnsortedOffer(uint256 id) public view returns (uint256) {

1149:     function getPayAmountWithFee(
1150:         ERC20 pay_gem,
1151:         ERC20 buy_gem,
1152:         uint256 buy_amt
1153:     ) public view returns (uint256 fill_amt) {
```

## [G-06] OPTIMIZE NAMES TO SAVE GAS
Function hashes that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, per sorted position shifted. Prioritize the most called functions and sort and rename them according to the function hashes/method IDs. For a better understanding please refer to [this link](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

The method IDs in the RubiconMarket.sol will be used the most. A lower method ID may be given to the most frequently used functions. [This](https://emn178.github.io/solidity-optimize-name/) is a useful tool that can be used for the same, if required.

```json
Sighash   |   Function Signature
========================
14210906  =>  getPayAmountWithFee(ERC20,ERC20,uint256)
35744059  =>  buyAllAmount(ERC20,uint256,ERC20,uint256)
49606455  =>  take(bytes32,uint128)
57597321  =>  _findpos(uint256,uint256)
67457022  =>  rmul(uint256,uint256)
13af4035  =>  setOwner(address)
fe9fbb80  =>  isAuthorized(address)
771602f7  =>  add(uint256,uint256)
b67d77c5  =>  sub(uint256,uint256)
c8a4ac9c  =>  mul(uint256,uint256)
7ae2b5c7  =>  min(uint256,uint256)
6d5433e6  =>  max(uint256,uint256)
c8e6c7d9  =>  imin(int256,int256)
ae94e448  =>  imax(int256,int256)
8b9af5c1  =>  wmul(uint256,uint256)
5fde731c  =>  wdiv(uint256,uint256)
0e2286d3  =>  rdiv(uint256,uint256)
fc741c7c  =>  makerFee()
82afd23b  =>  isActive(uint256)
c41a360a  =>  getOwner(uint256)
6d0cee75  =>  getRecipient(uint256)
4579268a  =>  getOffer(uint256)
779997c3  =>  bump(bytes32)
d6febde8  =>  buy(uint256,uint256)
40e58ee5  =>  cancel(uint256)
b4f9b6c8  =>  kill(bytes32)
6c3c74a5  =>  make(ERC20,ERC20,uint128,uint128)
e512c276  =>  offer(uint256,ERC20,uint256,ERC20,address,address)
e38d7e99  =>  _next_id()
6f7c1820  =>  getFeeBPS()
cea46aee  =>  calcAmountAfterFee(uint256)
c2b6b58c  =>  isClosed()
557ed1ba  =>  getTime()
07da68f5  =>  stop()
c4d66de8  =>  initialize(address)
de797626  =>  offer(uint256,)
5bdf5bb2  =>  batchOffer(uint256[],address[],uint256[],address[])
19fb4abf  =>  batchCancel(uint256[])
8755a474  =>  batchRequote(uint256[],uint256[],address[],uint256[],address[])
467f0b7b  =>  del_rank(uint256)
d2563c82  =>  setMinSell(ERC20,uint256)
d0417a69  =>  getMinSell(ERC20)
ac207e89  =>  getBestOffer(ERC20,ERC20)
943911bc  =>  getWorseOffer(uint256)
911550f4  =>  getBetterOffer(uint256)
ef4460bb  =>  getOfferCount(ERC20,ERC20)
8af82a2e  =>  getFirstUnsortedOffer()
61f54a79  =>  getNextUnsortedOffer(uint256)
d2b420ce  =>  isOfferSorted(uint256)
7991da45  =>  sellAllAmount(ERC20,uint256,ERC20,uint256)
33d1862e  =>  getBuyAmountWithFee(ERC20,ERC20,uint256)
821a2df9  =>  getBuyAmount(ERC20,ERC20,uint256)
57bb4ed3  =>  getPayAmount(ERC20,ERC20,uint256)
d8ee2ff4  =>  _buys(uint256,uint256)
1a85bd57  =>  _find(uint256)
b31aae3d  =>  _isPricedLtOrEq(uint256,uint256)
05b2252b  =>  _matcho(uint256,ERC20,uint256,ERC20,uint256,bool,address,address)
55e79f7d  =>  _offeru()
6513323a  =>  _sort(uint256,)
0d9e11a8  =>  _unsort(uint256)
e893fdd7  =>  _hide(uint256)
604b6a9c  =>  setFeeBPS(uint256)
b8bd3dbb  =>  setMakerFee(uint256)
f46901ed  =>  setFeeTo(address)
6611f528  =>  getFeeTo()
```

## [G-07] REORDER STRUCTURE LAYOUT
The following structs could be optimized by moving the position of certain variables in order to optimize storage.

contracts\utilities\FeeWrapper.sol
```js
12:     struct CallParams {
13:         bytes4 selector; // function selector
14:         bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
15:         address target; // target contract to call
16:         FeeParams feeParams;
17:     }
```

contracts\utilities\poolsUtility\Position.sol
```js
33:     struct Position {
34:         address asset; // supplied as collateral
35:         address quote; // borrowed token
36:         uint256 borrowedAmount; // amount of borrowed quote
37:         uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
38:         uint256 blockNum; // block number on which position was opened
39:         bool isActive; // false by default, when active - true
40:     }
```

## [G-08] SHOULD USE LOCAL ARGUMENTS INSTEAD OF STATE VARIABLE
Using the local arguments of the function while emitting an event instead of the state variable saves roughly 122 gas per emit.

contracts\RubiconMarket.sol
```js
27:         emit LogSetOwner(owner);
```

contracts\periphery\BathBuddy.sol
```js
241:         emit RewardsDurationUpdated(rewardsDuration[token]);
```

contracts\utilities\poolsUtility\Position.sol
```js
418:         emit PositionOpened(lastPositionId, pos);
```