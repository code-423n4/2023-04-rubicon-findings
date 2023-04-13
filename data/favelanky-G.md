
## [G-1] Reorder the require statements to have the less gas consuming before the expensive one

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (**2100 gas**) in a function that may ultimately revert in the unhappy case.

There are 1 instances of this issue:

```solidity
File: /contracts/BathHouseV2.sol

-        require(
-           tokenToBathToken[underlying] == address(0),
-           "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
-       );
        require(
            underlying != address(0),
            "createBathToken: UNDERLYING == ADDRESS 0"
        );
        require(
            implementation != address(0),
            "createBathToken: IMPLEMENTATION == ADDRESS 0"
        );
+        require(
+           tokenToBathToken[underlying] == address(0),
+           "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
+       );

```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L68-L79

## [G-2] `internal` functions only called once can be inlined to save gas

There are 2 instances of this issue:

```solidity
File: /contracts/BathHouseV2.sol

137:  function _bathify(

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L137

```solidity
File: /contracts/RubiconMarket.sol

35:  function isAuthorized(address src) internal view returns (bool) {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L35

## [G-3] Use Custom Errors

[Source](https://blog.soliditylang.org/2021/04/21/custom-errors/)
Instead of using error strings, to reduce deployment and runtime cost, you should use Custom Errors. This would save both deployment and runtime cost.

There are 112 instances of this issue:

```solidity
File: /contracts/BathHouseV2.sol

26:  require(msg.sender == admin, "onlyAdmin: !admin");

32:  require(!initialized, "BathHouseV2 already initialized!");

67:  require(

71:  require(

75:  require(

143:  require(_underlying != address(0), "_bathify: ADDRESS ZERO");

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L26

```solidity
File: /contracts/V2Migrator.sol

40:  require(bathBalance > 0, "migrate: ZERO AMOUNT");

53:  require(

62:  require(

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L40

```solidity
File: /contracts/RubiconMarket.sol

30:  require(isAuthorized(msg.sender), "ds-auth-unauthorized");

46:  require((z = x + y) >= x, "ds-math-add-overflow");

50:  require((z = x - y) <= x, "ds-math-sub-underflow");

54:  require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");

245:  require(isActive(id));

251:  require(isActive(id));

252:  require(

264:  require(!locked);

320:  require(uint128(spend) == spend, "spend is not an int");

321:  require(uint128(quantity) == quantity, "quantity is not an int");

338:  require(

349:  require(

358:  require(

373:  require(

378:  require(

459:  ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))

460:   : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));

487:  require(cancel(uint256(id)));

518:  require(uint128(pay_amt) == pay_amt);

519:  require(uint128(buy_amt) == buy_amt);

520:  require(pay_amt > 0);

521:  require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6

522:  require(buy_amt > 0);

523:  require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6

524:  require(pay_gem != buy_gem);

537:  require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));

564:  require(buy(uint256(id), maxTakeAmount));

580:  require(amount > 0);

597:  require(!isClosed());

603:  require(isActive(id));

604:  require(!isClosed());

610:  require(isActive(id));

611:  require(

700:  require(!initialized, "contract is already initialized");

701:  require(_feeTo != address(0));

719:  require(isActive(id), "Offer was deleted or taken, or never existed.");

720:  require(

725:  "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."

752:  require(buy(uint256(id), maxTakeAmount));

756:  require(cancel(uint256(id)));

833:  require(!locked, "Reentrancy attempt");

834:  require(_dust[address(pay_gem)] <= pay_amt);

858:  require(!locked, "Reentrancy attempt");

873:  require(!locked, "Reentrancy attempt");

876:  require(_unsort(id));

878:  require(_hide(id), "can't hide");

892:  require(

937:  require(!locked);

939:  require(

1033:  require(!locked);

1039:  require(offerId != 0, "0 offerId"); //Fails if there are not more offers

1064:  require(fill_amt >= min_fill_amount, "min_fill_amount isn't filled");

1074:  require(!locked);

1079:  require(offerId != 0, "offerId == 0");

1106:  require(

1135:  require(offerId != 0); //Fails if there are not enough offers to complete

1168:  require(offerId != 0); //Fails if there are not enough offers to complete

1183:  require(buyEnabled);

1193:  require(super.buy(id, amount));

1208:  require(id > 0);

1225:  require(id > 0);

1353:  //     require(_dust[address(pay_gem)] <= pay_amt);

1365:  require(isActive(id));

1380:  //requirement below is satisfied by statements above

1381:  //require(_isPricedLtOrEq(id, pos));

1393:  //requirement below is satisfied by statements above

1394:  //require(!_isPricedLtOrEq(id, prev_id));

1409:  require(_span[pay_gem][buy_gem] > 0);

1411:  require(

1418:  require(_rank[_rank[id].next].prev == id);

1427:  require(_rank[_rank[id].prev].next == id);

1443:  require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list

1476:  require(newFeeTo != address(0));

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L30

```solidity
File: /contracts/utilities/FeeWrapper.sol

69:  require(_OK, "low-level call to the Rubicon failed");

85:  require(_OK, "low-level call to the router failed");

114:  require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");

118:  require(OK, "ETH transfer failed");

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L69

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

75:  require(pos.isActive, "borrowBalanceOfPos: POS ISN'T ACTIVE");

99:  require(

113:  require(

211:  require(pos.isActive, "closePosition: POS ISN'T ACTIVE");

227:  require(pos.isActive, "increaseMargin: POS ISN'T ACTIVE");

272:  require(

287:  require(

297:  require(

311:  require(_err == 0, "_maxBorrow: ERROR");

312:  require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");

313:  require(_shortfall == 0, "_maxBorrow: SHORTFALL != 0");

317:  require(_max > 0, "_maxBorrow: can't borrow 0");

339:  require(_errs[0] == 0);

343:  require(comptroller.exitMarket(_bathToken) == 0, "_exitMarket: ERROR");

358:  require(

384:  require(

590:  ? require(

594:   : require(

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L75

```solidity
File: /contracts/periphery/BathBuddy.sol

75:  require(!friendshipStarted, "I already have a buddy!");

87:  require(msg.sender == owner);

94:  require(

104:  require(msg.sender == bathHouse, "You are not my beloved bath house!");

121:  require(friendshipStarted, "I have not started a bathToken friendship");

142:  require(friendshipStarted, "I have not started a bathToken friendship");

216:  require(

235:  require(

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L75

## [G-4] Use nested if and, avoid multiple check combinations

Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

There are 24 instances of this issue:

```solidity
File: /contracts/RubiconMarket.sol

254:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))

348:  if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

458:         _offer.owner == address(0) && msg.sender == _offer.recipient

614:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))

724:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0)),

893:             payAmts.length == payGems.length &&

894:                 payAmts.length == buyAmts.length &&

940:  !isActive(id) &&

941:                 _rank[id].delb != 0 &&

1197:  isActive(id) &&

1216:  while (top != 0 && _isPricedLtOrEq(id, top)) {

1228:  while (pos != 0 && !isActive(pos)) {

1243:  while (pos != 0 && _isPricedLtOrEq(id, pos)) {

1251:  while (pos != 0 && !_isPricedLtOrEq(id, pos)) {

1324:             t_buy_amt > 0 &&

1325:             t_pay_amt > 0 &&

1412:             _rank[id].delb == 0 && //assert id is in the sorted list

1451:  while (uid > 0 && uid != id) {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L254

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

175:  if (i.add(1) == vars.limit && vars.lastBorrow != 0) {

391:  IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&

591:                 _leverage > _wad && _leverage <= _leverageMax,

595:                 _leverage >= _wad && _leverage <= _leverageMax,

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L175

```solidity
File: /contracts/periphery/BathBuddy.sol

95:  msg.sender == myBathTokenBuddy &&

96:  msg.sender != address(0) &&

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L95

## [G-5] Remove Unnecessary Functions

Remove math related to summation, subtraction... Solidity > 0.8.0 automatically checks overflow/underflow.

There are 3 instances of this issue:

```solidity
File: /contracts/RubiconMarket.sol

45: contract DSMath {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L45

These functions never used:

```solidity
File: /contracts/RubiconMarket.sol

297:  function bump(bytes32 id_) external can_buy(uint256(id_)) {

628:  function stop() external auth {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L297

## [G-6] OpenZeppelin’s ReentrancyGuard Contract Is Gas-optimized

There is 1 instances of this issue:

```solidity
File: /contracts/RubiconMarket.sol

264:  modifier synchronized() {
265:  require(!locked);
266:         locked = true;
267:  _;
268:         locked = false;
269:     }

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L264-L269

## [G-7] Use `storage` Instead Of `memory` For structs/arrays

It may not be obvious, but every time you copy a storage `struct`/`array`/`mapping` to a `memory` variable, you are copying each member by reading it from `storage`, which is expensive. And when you use the `storage` keyword, you are just storing a pointer to the storage, which is much cheaper. Exception: case when you need to read all or many members multiple times. In report included only cases that saved gas

There are 4 instances of this issue

```solidity
File: /contracts/RubiconMarket.sol

291:         OfferInfo memory _offer = offers[id];

291:         OfferInfo memory _offer = offers[id];

291:         OfferInfo memory _offer = offers[id];

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L291

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

75:         Position memory pos = positions[posId];

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L75

## [G-8] Function Can Be `pure`

There are 1 instances of this issue:

```solidity
File: /contracts/utilities/FeeWrapper.sol

28:  function calculateFee(

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L28

## [G-9] Use Local Variable Instead Of Global

There are 1 instances of this issue:

```solidity
File: /contracts/periphery/BathBuddy.sol

241:  emit RewardsDurationUpdated(rewardsDuration[token]);

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L241

## [G-10] Functions guaranteed to revert when called by normal users can be marked `payable`

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are `CALLVALUE`(2),`DUP1`(3),`ISZERO`(3),`PUSH2`(3),`JUMPI`(10),`PUSH1`(3),`DUP1`(3),`REVERT`(0),`JUMPDEST`(1),`POP`(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

There are 8 instances of this issue:

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

97:     ) external onlyOwner {

111:     ) external onlyOwner {

209:  function closePosition(uint256 posId) external onlyOwner {

225:  function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

241:  function withdraw(address token, uint256 amount) external onlyOwner {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L97

```solidity
File: /contracts/periphery/BathBuddy.sol

86:  modifier onlyOwner() {

193:     ) external onlyOwner updateReward(address(0), address(rewardsToken)) {

234:     ) external onlyOwner {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L86
