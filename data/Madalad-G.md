# Gas Optimizations Summary
| |Issue|Instances|
|:-:|:-|:-:|
|[G-01]|Functions guaranteed to revert when called by normal users can be marked `payable`|8|
|[G-02]|Use assembly to calculate hashes|6|
|[G-03]|Division/Multiplication by 2 should use bit shifting|4|
|[G-04]|`require`/`revert` strings longer than 32 bytes cost extra gas|7|
|[G-05]|Use `indexed` to save gas|17|
|[G-06]|Use `constant`, `immutable` where applicable|6|
|[G-07]|Inline `internal` functions that are only called once|23|
|[G-08]|Expressions for constant values such as a call to `keccak256` should use `immutable` rather than `constant`.|1|
|[G-09]|Refactor modifiers to call a local function|15|
|[G-10]|Use `unchecked` for operations that cannot overflow/underflow|11|
|[G-11]|Change `public` functions to `external`|10|
|[G-12]|Use a more recent version of Solidity|2|
|[G-13]|Naming a return variable and still calling the `return` keyword wastes gas|10|
|[G-14]|Usage of `uint` smaller than 32 bytes (256 bits) incurs overhead|6|
|[G-15]|Use of `DSMath`, `SafeMath` is redundant|3|
|[G-16]|Use named return values|37|
|[G-17]|State variables can be packed more efficiently|1|
|[G-18]|Remove useless function|1|

Total issues: 18

Total instances: 168

&nbsp;
# Gas Optimizations
## [G-01] Functions guaranteed to revert when called by normal users can be marked `payable`

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost (2400 per instance).

Instances: 8
```solidity
File: contracts/BathHouseV2.sol

60:     function createBathToken(

```
- [contracts/BathHouseV2.sol#L60](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L60)

```solidity
File: contracts/periphery/BathBuddy.sol

191:     function notifyRewardAmount(

232:     function setRewardsDuration(

```
- [contracts/periphery/BathBuddy.sol#L191](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L191)
- [contracts/periphery/BathBuddy.sol#L232](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L232)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

93:     function buyAllAmountWithLeverage(

107:     function sellAllAmountWithLeverage(

210:     function closePosition(uint256 posId) external onlyOwner {

226:     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242:     function withdraw(address token, uint256 amount) external onlyOwner {

```
- [contracts/utilities/poolsUtility/Position.sol#L93](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L93)
- [contracts/utilities/poolsUtility/Position.sol#L107](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L107)
- [contracts/utilities/poolsUtility/Position.sol#L210](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L210)
- [contracts/utilities/poolsUtility/Position.sol#L226](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L226)
- [contracts/utilities/poolsUtility/Position.sol#L242](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L242)


&nbsp;
## [G-02] Use assembly to calculate hashes

Saves 5000 deployment gas per instance and 80 runtime gas per instance.

### Unoptimized
```solidity
function solidityHash(uint256 a, uint256 b) public view {
	//unoptimized
	keccak256(abi.encodePacked(a, b));
}
```

### Optimized
```solidity
function assemblyHash(uint256 a, uint256 b) public view {
	//optimized
	assembly {
		mstore(0x00, a)
		mstore(0x20, b)
		let hashedVal := keccak256(0x00, 0x40)
	}
}
```

Instances: 6
```solidity
File: contracts/RubiconMarket.sol

369:                 keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

400:             keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

423:             keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

442:                 keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

476:             keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

555:             keccak256(abi.encodePacked(pay_gem, buy_gem)),

```
- [contracts/RubiconMarket.sol#L369](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L369)
- [contracts/RubiconMarket.sol#L400](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L400)
- [contracts/RubiconMarket.sol#L423](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L423)
- [contracts/RubiconMarket.sol#L442](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L442)
- [contracts/RubiconMarket.sol#L476](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L476)
- [contracts/RubiconMarket.sol#L555](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L555)


&nbsp;
## [G-03] Division/Multiplication by 2 should use bit shifting

`<x> * 2` is equivalent to `<x> << 1` and `<x> / 2` is the same as `<x> >> 1`. The `MUL` and `DIV` opcodes cost 5 gas, whereas `SHL` and `SHR` only cost 3 gas.

Instances: 4
```solidity
File: contracts/RubiconMarket.sol

78:         z = add(mul(x, y), WAD / 2) / WAD;

82:         z = add(mul(x, y), RAY / 2) / RAY;

86:         z = add(mul(x, WAD), y / 2) / y;

90:         z = add(mul(x, RAY), y / 2) / y;

```
- [contracts/RubiconMarket.sol#L78](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L78)
- [contracts/RubiconMarket.sol#L82](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L82)
- [contracts/RubiconMarket.sol#L86](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L86)
- [contracts/RubiconMarket.sol#L90](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L90)


&nbsp;
## [G-04] `require`/`revert` strings longer than 32 bytes cost extra gas

Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met. If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

Instances: 7
```solidity
File: contracts/RubiconMarket.sol

720:         require(isActive(id), "Offer was deleted or taken, or never existed.");

```
- [contracts/RubiconMarket.sol#L720](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L720)

```solidity
File: contracts/periphery/BathBuddy.sol

105:         require(msg.sender == bathHouse, "You are not my beloved bath house!");

122:         require(friendshipStarted, "I have not started a bathToken friendship");

143:         require(friendshipStarted, "I have not started a bathToken friendship");

```
- [contracts/periphery/BathBuddy.sol#L105](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L105)
- [contracts/periphery/BathBuddy.sol#L122](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L122)
- [contracts/periphery/BathBuddy.sol#L143](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L143)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

76:         require(pos.isActive, "borrowBalanceOfPos: POS ISN'T ACTIVE");

```
- [contracts/utilities/poolsUtility/Position.sol#L76](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L76)

```solidity
File: contracts/utilities/FeeWrapper.sol

70:         require(_OK, "low-level call to the Rubicon failed");

86:         require(_OK, "low-level call to the router failed");

```
- [contracts/utilities/FeeWrapper.sol#L70](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L70)
- [contracts/utilities/FeeWrapper.sol#L86](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L86)


&nbsp;
## [G-05] Use `indexed` to save gas
Using `indexed` for value type event parameters (address, uint, bool) saves at least 80 gas in emitting the event (https://gist.github.com/Tomosuke0930/9bf61e01a8c3e214d95a9b84dcb41d97).

Note that for other types however (string, bytes), it is more expensive.

Instances: 17
```solidity
File: contracts/RubiconMarket.sol

97:     event LogItemUpdate(uint256 id);

662:     event LogMinSell(address pay_gem, uint256 min_amount);

664:     event LogUnsortedOffer(uint256 id);

665:     event LogSortedOffer(uint256 id);

667:     event LogDelete(address keeper, uint256 id);

668:     event LogMatch(uint256 id, uint256 amount);

```
- [contracts/RubiconMarket.sol#L97](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L97)
- [contracts/RubiconMarket.sol#L662](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L662)
- [contracts/RubiconMarket.sol#L664](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L664)
- [contracts/RubiconMarket.sol#L665](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L665)
- [contracts/RubiconMarket.sol#L667](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L667)
- [contracts/RubiconMarket.sol#L668](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L668)

```solidity
File: contracts/BathHouseV2.sol

23:     event BathTokenCreated(address bathToken, address underlying);

24:     event BuddySpawned(address bathToken, address bathBuddy);

```
- [contracts/BathHouseV2.sol#L23](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L23)
- [contracts/BathHouseV2.sol#L24](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L24)

```solidity
File: contracts/periphery/BathBuddy.sol

261:     event RewardAdded(uint256 reward);

262:     event Staked(address indexed user, uint256 amount);

263:     event Withdrawn(address indexed user, uint256 amount);

264:     event RewardPaid(address indexed user, uint256 reward);

265:     event RewardsDurationUpdated(uint256 newDuration);

266:     event Recovered(address token, uint256 amount);

```
- [contracts/periphery/BathBuddy.sol#L261](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L261)
- [contracts/periphery/BathBuddy.sol#L262](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L262)
- [contracts/periphery/BathBuddy.sol#L263](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L263)
- [contracts/periphery/BathBuddy.sol#L264](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L264)
- [contracts/periphery/BathBuddy.sol#L265](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L265)
- [contracts/periphery/BathBuddy.sol#L266](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L266)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

50:     event PositionOpened(uint256 positionId, Position position);

51:     event PositionClosed(uint256 positionId);

52:     event MarginIncreased(uint256 positionId, uint256 amount);

```
- [contracts/utilities/poolsUtility/Position.sol#L50](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L50)
- [contracts/utilities/poolsUtility/Position.sol#L51](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L51)
- [contracts/utilities/poolsUtility/Position.sol#L52](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L52)


&nbsp;
## [G-06] Use `constant`, `immutable` where applicable

Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving SLOAD. Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around 20 000 gas per variable) and replace the expensive storage-reading operations (around 2100 gas per reading) to a less expensive value reading (3 gas).

Instances: 6
```solidity
File: contracts/RubiconMarket.sol

682:     bool public AqueductDistributionLive;

684:     address public AqueductAddress;

```
- [contracts/RubiconMarket.sol#L682](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L682)
- [contracts/RubiconMarket.sol#L684](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L684)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

44:     Comptroller public comptroller;

45:     PriceOracle public oracle;

46:     RubiconMarket public rubiconMarket;

47:     BathHouseV2 public bathHouseV2;

```
- [contracts/utilities/poolsUtility/Position.sol#L44](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L44)
- [contracts/utilities/poolsUtility/Position.sol#L45](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L45)
- [contracts/utilities/poolsUtility/Position.sol#L46](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L46)
- [contracts/utilities/poolsUtility/Position.sol#L47](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L47)


&nbsp;
## [G-07] Inline `internal` functions that are only called once

Saves 20-40 gas per instance. See https://blog.soliditylang.org/2021/03/02/saving-gas-with-simple-inliner/ for more details.

Instances: 23
```solidity
File: contracts/RubiconMarket.sol

31:         require(isAuthorized(msg.sender), "ds-auth-unauthorized");

535:         id = _next_id();

840:             _matcho(

1339:             _sort(id, pos);

1377:             : _findpos(id, pos);

```
- [contracts/RubiconMarket.sol#L31](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L31)
- [contracts/RubiconMarket.sol#L535](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L535)
- [contracts/RubiconMarket.sol#L840](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L840)
- [contracts/RubiconMarket.sol#L1339](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1339)
- [contracts/RubiconMarket.sol#L1377](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1377)

```solidity
File: contracts/BathHouseV2.sol

82:         (string memory name, string memory symbol, uint8 decimals) = _bathify(

```
- [contracts/BathHouseV2.sol#L82](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L82)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

82:         balance = _calculateDebt(bathTokenQuote, blockNum, borrowedAmount);

147:         _enterMarkets(bathTokenAsset);

149:         (vars.limit, vars.lastBorrow) = _borrowLimit(

184:             vars.currentBathTokenAmount += _borrowLoop(

198:         _savePosition(

219:         _repay(asset, quote, posId);

220:         _redeem(asset, bathTokenAmount);

222:         _removePosition(posId);

238:         _updateMargin(posId, bathTokenAmount, amount);

267:         _borrow(_bathTokenQuote, _toBorrow);

395:             _exitMarket(_bathTokenAsset);

450:             ? _marketBuy(_asset, _quote, _fillLimit)

451:             : _marketSell(_asset, _quote, _fillLimit);

```
- [contracts/utilities/poolsUtility/Position.sol#L82](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L82)
- [contracts/utilities/poolsUtility/Position.sol#L147](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L147)
- [contracts/utilities/poolsUtility/Position.sol#L149](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L149)
- [contracts/utilities/poolsUtility/Position.sol#L184](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L184)
- [contracts/utilities/poolsUtility/Position.sol#L198](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L198)
- [contracts/utilities/poolsUtility/Position.sol#L219](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L219)
- [contracts/utilities/poolsUtility/Position.sol#L220](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L220)
- [contracts/utilities/poolsUtility/Position.sol#L222](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L222)
- [contracts/utilities/poolsUtility/Position.sol#L238](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L238)
- [contracts/utilities/poolsUtility/Position.sol#L267](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L267)
- [contracts/utilities/poolsUtility/Position.sol#L395](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L395)
- [contracts/utilities/poolsUtility/Position.sol#L450](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L450)
- [contracts/utilities/poolsUtility/Position.sol#L451](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L451)

```solidity
File: contracts/utilities/FeeWrapper.sol

52:             return _rubicall(params);

54:             return _rubicallPayable(params);

64:         _chargeFee(_params.feeParams, _params.target);

80:         uint256 _msgValue = _chargeFeePayable(_params.feeParams);

```
- [contracts/utilities/FeeWrapper.sol#L52](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L52)
- [contracts/utilities/FeeWrapper.sol#L54](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L54)
- [contracts/utilities/FeeWrapper.sol#L64](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L64)
- [contracts/utilities/FeeWrapper.sol#L80](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L80)


&nbsp;
## [G-08] Expressions for constant values such as a call to `keccak256` should use `immutable` rather than `constant`.

When left as `constant`, the value is re-calculated each time it is used instead of being converted to a constant at compile time. This costs an extra ~100 gas for each access.

Using `immutable` only incurs the gas costs for the computation at deploy time.

See [here](https://github.com/ethereum/solidity/issues/9232) for a detailed description of the issue.

Instances: 1
```solidity
File: contracts/RubiconMarket.sol

232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```
- [contracts/RubiconMarket.sol#L232](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232)


&nbsp;
## [G-09] Refactor modifiers to call a local function

Modifiers code is copied in all instances where it's used, increasing bytecode size. By doing a refractor to the internal function, one can reduce bytecode size significantly at the cost of one JUMP.

Instances: 15
```solidity
File: contracts/RubiconMarket.sol

30:     modifier auth() {

245:     modifier can_buy(uint256 id) virtual {

251:     modifier can_cancel(uint256 id) virtual {

260:     modifier can_offer() virtual {

264:     modifier synchronized() {

597:     modifier can_offer() override {

603:     modifier can_buy(uint256 id) override {

610:     modifier can_cancel(uint256 id) virtual override {

643:     modifier note() {

719:     modifier can_cancel(uint256 id) override {

```
- [contracts/RubiconMarket.sol#L30](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L30)
- [contracts/RubiconMarket.sol#L245](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L245)
- [contracts/RubiconMarket.sol#L251](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L251)
- [contracts/RubiconMarket.sol#L260](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260)
- [contracts/RubiconMarket.sol#L264](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264)
- [contracts/RubiconMarket.sol#L597](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L597)
- [contracts/RubiconMarket.sol#L603](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L603)
- [contracts/RubiconMarket.sol#L610](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L610)
- [contracts/RubiconMarket.sol#L643](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L643)
- [contracts/RubiconMarket.sol#L719](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L719)

```solidity
File: contracts/BathHouseV2.sol

26:     modifier onlyAdmin() {

```
- [contracts/BathHouseV2.sol#L26](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L26)

```solidity
File: contracts/periphery/BathBuddy.sol

87:     modifier onlyOwner() {

94:     modifier onlyBuddy() {

104:     modifier onlyBathHouse() {

247:     modifier updateReward(address account, address token) {

```
- [contracts/periphery/BathBuddy.sol#L87](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L87)
- [contracts/periphery/BathBuddy.sol#L94](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L94)
- [contracts/periphery/BathBuddy.sol#L104](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L104)
- [contracts/periphery/BathBuddy.sol#L247](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L247)


&nbsp;
## [G-10] Use `unchecked` for operations that cannot overflow/underflow

By bypassing Solidity's built in overflow/underflow checks using `unchecked`, we can save gas. This is especially beneficial for the index variable within for loops (saves 120 gas per iteration).

Instances: 11
```solidity
File: contracts/RubiconMarket.sol

334:        offers[id].pay_amt = sub(_offer.pay_amt, quantity);

335:        offers[id].buy_amt = sub(_offer.buy_amt, spend);

569:         last_offer_id++;

899:         for (uint i = 0; i < payAmts.length; i++) {

911:         for (uint i = 0; i < ids.length; i++) {

924:         for (uint i = 0; i < ids.length; i++) {

```
- [contracts/RubiconMarket.sol#L334](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L334)
- [contracts/RubiconMarket.sol#L335](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L335)
- [contracts/RubiconMarket.sol#L569](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L569)
- [contracts/RubiconMarket.sol#L899](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L899)
- [contracts/RubiconMarket.sol#L911](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L911)
- [contracts/RubiconMarket.sol#L924](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L924)

```solidity
File: contracts/BathHouseV2.sol

122:         for (uint256 i = 0; i < buddies.length; ++i) {

```
- [contracts/BathHouseV2.sol#L122](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L122)

```solidity
File: contracts/V2Migrator.sol

31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {

```
- [contracts/V2Migrator.sol#L31](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L31)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

158:         for (uint256 i = 0; i < vars.limit; ++i) {

407:         lastPositionId++;

```
- [contracts/utilities/poolsUtility/Position.sol#L158](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L158)
- [contracts/utilities/poolsUtility/Position.sol#L407](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L407)

```solidity
File: contracts/utilities/FeeWrapper.sol

40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {

```
- [contracts/utilities/FeeWrapper.sol#L40](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L40)


&nbsp;
## [G-11] Change `public` functions to `external`

Functions marked as `public` that are not called internally should be set to `external` to save gas and improve code quality. External call cost is less expensive than of public functions.

Instances: 10
```solidity
File: contracts/RubiconMarket.sol

290:     ) public view returns (uint256, ERC20, uint256, ERC20) {

574:     function getFeeBPS() public view returns (uint256) {

624:     function getTime() public view returns (uint64) {

700:     function initialize(address _feeTo) public {

738:     ) public override returns (bytes32) {

1001:     ) public view returns (uint256) {

1010:     function getFirstUnsortedOffer() public view returns (uint256) {

1016:     function getNextUnsortedOffer(uint256 id) public view returns (uint256) {

1153:     ) public view returns (uint256 fill_amt) {

```
- [contracts/RubiconMarket.sol#L290](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L290)
- [contracts/RubiconMarket.sol#L574](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L574)
- [contracts/RubiconMarket.sol#L624](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624)
- [contracts/RubiconMarket.sol#L700](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L700)
- [contracts/RubiconMarket.sol#L738](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L738)
- [contracts/RubiconMarket.sol#L1001](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1001)
- [contracts/RubiconMarket.sol#L1010](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1010)
- [contracts/RubiconMarket.sol#L1016](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1016)
- [contracts/RubiconMarket.sol#L1153](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1153)

```solidity
File: contracts/BathHouseV2.sol

47:     ) public view returns (address) {

```
- [contracts/BathHouseV2.sol#L47](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L47)


&nbsp;
## [G-12] Use a more recent version of Solidity

Use a Solidity version of at least 0.8.2 to get simple compiler automatic inlining.

Use a Solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads.

Use a Solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()`/`require()` strings.

Use a Solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value.

Use a Solidity version of at least 0.8.12 to get string.concat() to be used instead of abi.encodePacked(<str>,<str>).

Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions.

Instances: 2
```solidity
File: contracts/RubiconMarket.sol

2: pragma solidity ^0.8.9;

```
- [contracts/RubiconMarket.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2)

```solidity
File: contracts/periphery/BathBuddy.sol

2: pragma solidity ^0.8.0;

```
- [contracts/periphery/BathBuddy.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2)


&nbsp;
## [G-13] Naming a return variable and still calling the `return` keyword wastes gas

Instances: 10
```solidity
File: contracts/RubiconMarket.sol

58:     function min(uint256 x, uint256 y) internal pure returns (uint256 z) {

62:     function max(uint256 x, uint256 y) internal pure returns (uint256 z) {

66:     function imin(int256 x, int256 y) internal pure returns (int256 z) {

70:     function imax(int256 x, int256 y) internal pure returns (int256 z) {

276:     function isActive(uint256 id) public view returns (bool active) {

280:     function getOwner(uint256 id) public view returns (address owner) {

284:     function getRecipient(uint256 id) public view returns (address owner) {

496:     ) external virtual returns (bytes32 id) {

620:     function isClosed() public pure returns (bool closed) {

873:     ) public override can_cancel(id) returns (bool success) {

```
- [contracts/RubiconMarket.sol#L58](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L58)
- [contracts/RubiconMarket.sol#L62](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L62)
- [contracts/RubiconMarket.sol#L66](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L66)
- [contracts/RubiconMarket.sol#L70](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L70)
- [contracts/RubiconMarket.sol#L276](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L276)
- [contracts/RubiconMarket.sol#L280](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L280)
- [contracts/RubiconMarket.sol#L284](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L284)
- [contracts/RubiconMarket.sol#L496](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L496)
- [contracts/RubiconMarket.sol#L620](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L620)
- [contracts/RubiconMarket.sol#L873](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L873)


&nbsp;
## [G-14] Usage of `uint` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Consider using a larger size then downcasting where needed.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html

Instances: 6
```solidity
File: contracts/RubiconMarket.sol

124:         uint128 pay_amt,

125:         uint128 buy_amt

159:         uint128 take_amt,

160:         uint128 give_amt

180:         uint128 pay_amt,

181:         uint128 buy_amt

```
- [contracts/RubiconMarket.sol#L124](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L124)
- [contracts/RubiconMarket.sol#L125](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L125)
- [contracts/RubiconMarket.sol#L159](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L159)
- [contracts/RubiconMarket.sol#L160](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L160)
- [contracts/RubiconMarket.sol#L180](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L180)
- [contracts/RubiconMarket.sol#L181](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L181)


&nbsp;
## [G-15] Use of `DSMath`, `SafeMath` is redundant
With Solidity version 0.8.0 and above, overflow/underflow checks are performed automatically, and so using a library such as OpenZeppelin's `SafeMath` or `DSMath` in RubiconMarket.sol is unnecessary and wastes gas.

Instances: 3
```solidity
File: contracts/RubiconMarket.sol

45: contract DSMath {

```
- [contracts/RubiconMarket.sol#L45](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L45)

```solidity
File: contracts/periphery/BathBuddy.sol

39:     using SafeMath for uint256;

```
- [contracts/periphery/BathBuddy.sol#L39](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L39)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

15: contract Position is Ownable, DSMath {

```
- [contracts/utilities/poolsUtility/Position.sol#L15](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L15)

&nbsp;
## [G-16] Use named return values

Using named return values instead of explicitly calling `return` saves ~13 execution gas per call and >1000 deployment gas per instance.

Instances: 37
```solidity
File: contracts/RubiconMarket.sol

35:     function isAuthorized(address src) internal view returns (bool) {

272:     function makerFee() public view returns (uint256) {

290:     ) public view returns (uint256, ERC20, uint256, ERC20) {

317:     ) public virtual can_buy(id) synchronized returns (bool) {

568:     function _next_id() internal returns (uint256) {

574:     function getFeeBPS() public view returns (uint256) {

624:     function getTime() public view returns (uint64) {

738:     ) public override returns (bytes32) {

766:     ) external can_offer returns (uint256) {

787:     ) external can_offer returns (uint256) {

810:     ) external can_offer returns (uint256) {

833:     ) public can_offer returns (uint256) {

858:     ) public override can_buy(id) returns (bool) {

937:     function del_rank(uint256 id) external returns (bool) {

958:     ) external auth note returns (bool) {

977:     ) public view returns (uint256) {

985:     function getWorseOffer(uint256 id) public view returns (uint256) {

1001:     ) public view returns (uint256) {

1010:     function getFirstUnsortedOffer() public view returns (uint256) {

1016:     function getNextUnsortedOffer(uint256 id) public view returns (uint256) {

1020:     function isOfferSorted(uint256 id) public view returns (bool) {

1183:     function _buys(uint256 id, uint256 amount) internal returns (bool) {

1208:     function _find(uint256 id) internal view returns (uint256) {

1225:     function _findpos(uint256 id, uint256 pos) internal view returns (uint256) {

1264:     ) internal view returns (bool) {

1407:     ) internal returns (bool) {

1440:     ) internal returns (bool) {

1466:     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {

1471:     function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {

1476:     function setFeeTo(address newFeeTo) external auth returns (bool) {

```
- [contracts/RubiconMarket.sol#L35](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35)
- [contracts/RubiconMarket.sol#L272](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L272)
- [contracts/RubiconMarket.sol#L290](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L290)
- [contracts/RubiconMarket.sol#L317](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L317)
- [contracts/RubiconMarket.sol#L568](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568)
- [contracts/RubiconMarket.sol#L574](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L574)
- [contracts/RubiconMarket.sol#L624](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624)
- [contracts/RubiconMarket.sol#L738](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L738)
- [contracts/RubiconMarket.sol#L766](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L766)
- [contracts/RubiconMarket.sol#L787](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L787)
- [contracts/RubiconMarket.sol#L810](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L810)
- [contracts/RubiconMarket.sol#L833](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L833)
- [contracts/RubiconMarket.sol#L858](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L858)
- [contracts/RubiconMarket.sol#L937](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L937)
- [contracts/RubiconMarket.sol#L958](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L958)
- [contracts/RubiconMarket.sol#L977](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L977)
- [contracts/RubiconMarket.sol#L985](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L985)
- [contracts/RubiconMarket.sol#L1001](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1001)
- [contracts/RubiconMarket.sol#L1010](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1010)
- [contracts/RubiconMarket.sol#L1016](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1016)
- [contracts/RubiconMarket.sol#L1020](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1020)
- [contracts/RubiconMarket.sol#L1183](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1183)
- [contracts/RubiconMarket.sol#L1208](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1208)
- [contracts/RubiconMarket.sol#L1225](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1225)
- [contracts/RubiconMarket.sol#L1264](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1264)
- [contracts/RubiconMarket.sol#L1407](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1407)
- [contracts/RubiconMarket.sol#L1440](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1440)
- [contracts/RubiconMarket.sol#L1466](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466)
- [contracts/RubiconMarket.sol#L1471](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471)
- [contracts/RubiconMarket.sol#L1476](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1476)

```solidity
File: contracts/BathHouseV2.sol

47:     ) public view returns (address) {

```
- [contracts/BathHouseV2.sol#L47](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L47)

```solidity
File: contracts/periphery/BathBuddy.sol

114:     ) public view returns (uint256) {

121:     function rewardPerToken(address token) public view returns (uint256) {

142:     ) public view override returns (uint256) {

```
- [contracts/periphery/BathBuddy.sol#L114](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L114)
- [contracts/periphery/BathBuddy.sol#L121](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L121)
- [contracts/periphery/BathBuddy.sol#L142](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L142)

```solidity
File: contracts/utilities/FeeWrapper.sol

50:     ) external payable returns (bytes memory) {

62:     ) internal returns (bytes memory) {

78:     ) internal returns (bytes memory) {

```
- [contracts/utilities/FeeWrapper.sol#L50](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L50)
- [contracts/utilities/FeeWrapper.sol#L62](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L62)
- [contracts/utilities/FeeWrapper.sol#L78](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L78)


&nbsp;
## [G-17] State variables can be packed more efficiently

If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables can also be cheaper.

For more information about variable packing, see [here](https://blog.techfund.jp/p/solidity-variable-packing-saving-deployment-costs/#:~:text=Solidity%20variable%20packing%20is%20a,bytes%20equal%20to%20256%20bits.).

Specifically, in `SimpleMarket` the `address` variable `feeTo` and the `bool` variable `locked` can be packed into a single storage slot, whereas in the current implementation they occupy one each.

Instances: 1
```solidity
File: contracts/RubiconMarket.sol

218: contract SimpleMarket is EventfulMarket, DSMath {
```
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L218

&nbsp;
## [G-18] Remove useless function

The function `SimpleMarket::bump` is useless; it does not change state or return anything. Remove it to reduce bytecode size and save deployment gas.

Instances: 1
```solidity
File: contracts/RubiconMarket.sol

297:     function bump(bytes32 id_) external can_buy(uint256(id_)) {
```
- [contracts/RubiconMarket.sol#L297](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297)