## [G-1]  Use unit based locked mutex on `synchronized` modifier 
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-#L269

Instead of toggling `bool locked` between `false` and `true`, you can toggle `uint locked` between `1` and `2`. This will reduce your method's gas consumption significantly, because changing a state variable from zero to non-zero costs a lot more than changing it from non-zero to non-zero.
```solidity
modifier synchronized() {
require(!locked);
locked = true;
_;
locked = false;
}
```


## [G-2] Calling and writting state varable twice in same function.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568-L571
before gas: 22498
after gas: 22393 saves `105` gas in each call.

```git
function _next_id() internal returns (uint256) {
-- last_offer_id++;
-- return last_offer_id;
++ return ++last_offer_id;
}
```

## [G-3] remove `can_offer()` modifier as it does nothing

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L597
The modifier calls another function `isClosed()` which always returns `false`. and require use `!` so it  always becomes true making `can_offer() `modifier do nothing.

```solidity
modifier can_offer() override {
require(!isClosed());//@audit no use of isclosed
_;
}

function isClosed() public pure returns (bool closed) {
return false;
}
```
and also remove it from these functions
```shell
RubiconMarket.sol:518:    ) public virtual can_offer synchronized returns (uint256 id) {
RubiconMarket.sol:766:    ) external can_offer returns (uint256) {
RubiconMarket.sol:787:    ) external can_offer returns (uint256) {
RubiconMarket.sol:810:    ) external can_offer returns (uint256) {
RubiconMarket.sol:833:    ) public can_offer returns (uint256) {
```

## [G-4] In funcion `_hide()` uid!=id will only happen if uid == 0  
 Because of a `while` loop before this if statment if it will be false on uid\==0 then it enters if statment.
 saves `40` gas each time that if statment is executed.
```git
function _hide(
uint256 id //id of maker offer to remove from unsorted list
) internal returns (bool) {
.
.
while (uid > 0 && uid != id) {
//find offer in unsorted order list
pre = uid;
uid = _near[uid];
}
-- if (uid != id) {
++ if (uid == 0){//uid!=id will only happen if uid == 0 
//did not find offer id in unsorted offers list
return false;
}
.
.
}
```
## [G-5] avoid setting state vairable that are already set with same values in initialize.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L714-#L715

```git
bool public buyEnabled = true; //buy enabled TODO: review this decision!
bool public matchingEnabled = true; //true: enable matching,
.
.
function initialize(address _feeTo) public {
.
.
-- matchingEnabled = true;//these are already set true
-- buyEnabled = true;
}
```


## [G-6] Make state varables private
```solidity
RubiconMarket.sol:691:    mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
RubiconMarket.sol:692:    mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
RubiconMarket.sol:693:    mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook
RubiconMarket.sol:694:    mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
RubiconMarket.sol:695:    mapping(uint256 => uint256) public _near; //next unsorted offer id
RubiconMarket.sol:696:    uint256 public _head; //first unsorted offer id
```

