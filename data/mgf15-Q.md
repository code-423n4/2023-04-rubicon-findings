
## Non Critical Issues


| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | Missing checks for `address(0)` when assigning values to address state variables | 5 |
| [NC-2](#NC-2) |  `require()` / `revert()` statements should have descriptive reason strings | 45 |
| [NC-3](#NC-3) | Return values of `approve()` not checked | 4 |
| [NC-4](#NC-4) | TODO Left in the code | 1 |
| [NC-5](#NC-5) | Event is missing `indexed` fields | 17 |
| [NC-6](#NC-6) | Constants should be defined rather than using magic numbers | 1 |
| [NC-7](#NC-7) | Functions not used internally could be marked external | 10 |
### [NC-1] Missing checks for `address(0)` when assigning values to address state variables

*Instances (5)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

36:         proxyAdmin = _pAdmin;

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

26:         owner = owner_;

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

77:         owner = _owner;

78:         myBathTokenBuddy = newBud;

79:         bathHouse = _bathHouse;

```

### [NC-2]  `require()` / `revert()` statements should have descriptive reason strings

*Instances (45)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

246:         require(isActive(id));

252:         require(isActive(id));

253:         require(

265:         require(!locked);

460:             ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))

461:             : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));

488:         require(cancel(uint256(id)));

519:         require(uint128(pay_amt) == pay_amt);

520:         require(uint128(buy_amt) == buy_amt);

521:         require(pay_amt > 0);

522:         require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6

523:         require(buy_amt > 0);

524:         require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6

525:         require(pay_gem != buy_gem);

538:         require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));

565:         require(buy(uint256(id), maxTakeAmount));

581:         require(amount > 0);

598:         require(!isClosed());

604:         require(isActive(id));

605:         require(!isClosed());

611:         require(isActive(id));

612:         require(

702:         require(_feeTo != address(0));

753:         require(buy(uint256(id), maxTakeAmount));

757:         require(cancel(uint256(id)));

835:         require(_dust[address(pay_gem)] <= pay_amt);

877:                 require(_unsort(id));

938:         require(!locked);

940:         require(

1034:         require(!locked);

1075:         require(!locked);

1136:                 require(offerId != 0); //Fails if there are not enough offers to complete

1169:                 require(offerId != 0); //Fails if there are not enough offers to complete

1184:         require(buyEnabled);

1194:         require(super.buy(id, amount));

1209:         require(id > 0);

1226:         require(id > 0);

1366:         require(isActive(id));

1410:         require(_span[pay_gem][buy_gem] > 0);

1412:         require(

1419:             require(_rank[_rank[id].next].prev == id);

1428:             require(_rank[_rank[id].prev].next == id);

1477:         require(newFeeTo != address(0));

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

88:         require(msg.sender == owner);

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

340:         require(_errs[0] == 0);

```

### [NC-3] Return values of `approve()` not checked
Not all IERC20 implementations `revert()` when there's a failure in `approve()`. The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything

*Instances (4)*:
```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

53:         underlying.approve(bathTokenV2, amountWithdrawn);

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

297:         IERC20(_quote).approve(_bathTokenQuote, _amountToRepay);

465:         IERC20(_quote).approve(address(rubiconMarket), _maxFill);

500:         IERC20(_asset).approve(

```

### [NC-4] TODO Left in the code
TODOs may signal that a feature is missing or not ready for audit, consider resolving the issue and removing the TODO comment

*Instances (1)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

675:     bool public buyEnabled = true; //buy enabled TODO: review this decision!

```

### [NC-5] Event is missing `indexed` fields
Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

*Instances (17)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

23:     event BathTokenCreated(address bathToken, address underlying);

24:     event BuddySpawned(address bathToken, address bathBuddy);

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

97:     event LogItemUpdate(uint256 id);

662:     event LogMinSell(address pay_gem, uint256 min_amount);

664:     event LogUnsortedOffer(uint256 id);

665:     event LogSortedOffer(uint256 id);

667:     event LogDelete(address keeper, uint256 id);

668:     event LogMatch(uint256 id, uint256 amount);

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

261:     event RewardAdded(uint256 reward);

262:     event Staked(address indexed user, uint256 amount);

263:     event Withdrawn(address indexed user, uint256 amount);

264:     event RewardPaid(address indexed user, uint256 reward);

265:     event RewardsDurationUpdated(uint256 newDuration);

266:     event Recovered(address token, uint256 amount);

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

50:     event PositionOpened(uint256 positionId, Position position);

51:     event PositionClosed(uint256 positionId);

52:     event MarginIncreased(uint256 positionId, uint256 amount);

```

### [NC-6] Constants should be defined rather than using magic numbers

*Instances (1)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

650:             bar := calldataload(36)

```

### [NC-7] Functions not used internally could be marked external

*Instances (10)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

45:     function getBathTokenFromAsset(

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

288:     function getOffer(

574:     function getFeeBPS() public view returns (uint256) {

624:     function getTime() public view returns (uint64) {

700:     function initialize(address _feeTo) public {

733:     function make(

998:     function getOfferCount(

1010:     function getFirstUnsortedOffer() public view returns (uint256) {

1016:     function getNextUnsortedOffer(uint256 id) public view returns (uint256) {

1149:     function getPayAmountWithFee(

```


## Low Issues


| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) |  `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()` | 6 |
| [L-2](#L-2) | Do not use deprecated library functions | 1 |
| [L-3](#L-3) | Initializers could be front-run | 2 |
| [L-4](#L-4) | Unsafe ERC20 operation(s) | 20 |
| [L-5](#L-5) | Missing input validation on array lengths | 3 |
### [L-1]  `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`
Use `abi.encode()` instead which will pad items to 32 bytes, which will [prevent hash collisions](https://docs.soliditylang.org/en/v0.8.13/abi-spec.html#non-standard-packed-mode) (e.g. `abi.encodePacked(0x123,0x456)` => `0x123456` => `abi.encodePacked(0x1,0x23456)`, but `abi.encode(0x123,0x456)` => `0x0...1230...456`). "Unless there is a compelling reason, `abi.encode` should be preferred". If there is only one argument to `abi.encodePacked()` it can often be cast to `bytes()` or `bytes32()` [instead](https://ethereum.stackexchange.com/questions/30912/how-to-compare-strings-in-solidity#answer-82739).
If all arguments are strings and or bytes, `bytes.concat()` should be used instead

*Instances (6)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

369:                 keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

400:             keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

423:             keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

442:                 keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

476:             keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

555:             keccak256(abi.encodePacked(pay_gem, buy_gem)),

```

### [L-2] Do not use deprecated library functions

*Instances (1)*:
```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

358:         IERC20(_token).safeApprove(_bathToken, _amount);

```

### [L-3] Initializers could be front-run
Initializers could be front-run, allowing an attacker to either set their own values, take ownership of the contract, and in the best case forcing a re-deployment

*Instances (2)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

32:     function initialize(address _comptroller, address _pAdmin) external {

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

700:     function initialize(address _feeTo) public {

```

### [L-4] Unsafe ERC20 operation(s)

*Instances (20)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

340:             _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),

351:                     _offer.buy_gem.transferFrom(

360:                     _offer.buy_gem.transferFrom(msg.sender, _offer.owner, mFee),

375:             _offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend),

376:             "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"

380:             _offer.pay_gem.transfer(msg.sender, quantity),

381:             "_offer.pay_gem.transfer(msg.sender, quantity) failed"

460:             ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))

461:             : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));

538:         require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

44:         bathTokenV1.transferFrom(msg.sender, address(this), bathBalance);

53:         underlying.approve(bathTokenV2, amountWithdrawn);

59:         IERC20(bathTokenV2).transfer(

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

100:         IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);

102:         IERC20(_feeToken).transfer(_feeTo, _feeAmount);

105:         IERC20(_feeToken).approve(_target, (_totalAmount - _feeAmount));

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

297:         IERC20(_quote).approve(_bathTokenQuote, _amountToRepay);

465:         IERC20(_quote).approve(address(rubiconMarket), _maxFill);

493:             IERC20(_asset).transferFrom(

500:         IERC20(_asset).approve(

```

### [L-5] Missing input validation on array lengths 

The functions below fail to perform input validation on arrays to verify the lengths match.
A mismatch could lead to an exception or undefined behavior.

*Instances (3)*:
```solidity 
File: 2023-04-rubicon/contracts/RubiconMarket.sol

918:        uint[] calldata ids,
919:        uint[] calldata payAmts,
920:        address[] calldata payGems,
921:        uint[] calldata buyAmts,
922:        address[] calldata buyGems
923:        for (uint i = 0; i < ids.length; i++) {
```

```solidity 
File: 2023-04-rubicon/contracts/BathHouseV2.sol
115:    function claimRewards(
116:        address[] memory buddies,
117:        address[] memory rewardsTokens
118:    ) external {
119:        // claim rewards from comptroller
120:        comptroller.claimComp(msg.sender);
121:        // get rewards from bathBuddy
122:        for (uint256 i = 0; i < buddies.length; ++i) {

```

```solidity 
File: 2023-04-rubicon/contracts/V2Migrator.sol
30:  constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

```