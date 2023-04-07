## QA
---
### Layout Order [1]

- The best-practices for layout within a contract is the following order: state variables, events, modifiers, constructor and functions.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
// modifiers coming after functions
87:    modifier onlyOwner() {
94:    modifier onlyBuddy() {
104:    modifier onlyBathHouse() {
247:    modifier updateReward(address account, address token) {

// events coming after functions
261:    event RewardAdded(uint256 reward);
262:    event Staked(address indexed user, uint256 amount);
263:    event Withdrawn(address indexed user, uint256 amount);
264:    event RewardPaid(address indexed user, uint256 reward);
265:    event RewardsDurationUpdated(uint256 newDuration);
266:    event Recovered(address token, uint256 amount);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
// modifier coming after function
30:    modifier auth() {

// state variables coming after functions
74:    uint256 constant WAD = 10 ** 18;
75:    uint256 constant RAY = 10 ** 27;
```

---

### Function Visibility [2]

- Order of Functions: Ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. Functions should be grouped according to their visibility and ordered: constructor, receive function (if exists), fallback function (if exists), public, external, internal, private. Within a grouping, place the view and pure functions last.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
// internal function coming before few externals
125:    function openPosition(
```

---

### natSpec missing [3]

- Some functions are missing @params and @returns. Other functions have a documentation but do not follow the NatSpec rules: Specification Format.” These are written with a triple slash (///) or a double asterisk block(/** ... */) directly above function declarations or statements to generate documentation in JSON format for developers and end-users. It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). These comments contain different types of tags:

@title: A title that should describe the contract/interface
@author: The name of the author (for contract, interface)
@notice: Explain to an end user what this does (for contract, interface, function, public state variable, event)
@dev: Explain to a developer any extra details (for contract, interface, function, state variable, event)
@param: Documents a parameter (just like in doxygen) and must be followed by parameter name (for function, event)
@return: Documents the return variables of a contract’s function (function, public state variable)
@inheritdoc: Copies all missing tags from the base function and must be followed by the contract name (for function, public state variable)
@custom…: Custom tag, semantics is application-defined (for everywhere)

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol

```solidity
28:    function calculateFee(

48:    function rubicall(

60:    function _rubicall(

76:    function _rubicallPayable(

93:    function _chargeFee(FeeParams memory _feeParams, address _target) internal {

108:    function _chargeFeePayable(
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
50:    event PositionOpened(uint256 positionId, Position position);

51:    event PositionClosed(uint256 positionId);

52:    event MarginIncreased(uint256 positionId, uint256 amount);

54:    constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {

65:    function borrowBalance(address bathToken) public returns (uint256 balance) {

72:    function borrowBalanceOfPos(

86:    function borrowRate(address bathToken) public view returns (uint256 rate) {

93:    function buyAllAmountWithLeverage(

107:    function sellAllAmountWithLeverage(

// @return missing
125:    function openPosition(

226:    function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242:    function withdraw(address token, uint256 amount) external onlyOwner {

251:    function _borrowLoop(

272:    function _borrow(address _cToken, uint256 _amount) internal {

280:    function _repay(address _asset, address _quote, uint256 _posId) internal {

// @return missing
306:    function _maxBorrow(

322:    function _calculateDebt(

336:    function _enterMarkets(address _bathToken) internal {

343:    function _exitMarket(address _bathToken) internal {

350:    function _supply(

382:    function _redeem(address _asset, uint256 _bathTokenAmount) internal {

401:    function _savePosition(

421:    function _removePosition(uint256 _positionId) internal {

426:    function _updateMargin(

454:    function _marketBuy(

475:    function _marketSell(

586:    function _leverageCheck(uint256 _leverage, bool _long) internal pure {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
9:      interface IBathBuddy {

13:    function getReward(IERC20 token, address recipient) external;

17:    function earned(

71:    function spawnBuddy(

87:    modifier onlyOwner() {

104:    modifier onlyBathHouse() {

112:    function lastTimeRewardApplicable(

121:    function rewardPerToken(address token) public view returns (uint256) {

139:    function earned(

157:    function getRewardForDuration(

168:    function getReward(

191:    function notifyRewardAmount(

232:    function setRewardsDuration(

247:    modifier updateReward(address account, address token) {

261:    event RewardAdded(uint256 reward);

262:    event Staked(address indexed user, uint256 amount);

263:    event Withdrawn(address indexed user, uint256 amount);

264:    event RewardPaid(address indexed user, uint256 reward);

265:    event RewardsDurationUpdated(uint256 newDuration);

266:    event Recovered(address token, uint256 amount);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol

```solidity
8:      interface IBathToken is IERC20 {

9:    function withdraw(

13:    function underlyingToken() external view returns(IERC20);

17:     contract V2Migrator {

21:    event Migrated(

30:    constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

38:    function migrate(IBathToken bathTokenV1) external {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
25:    function setOwner(address owner_) external auth {

30:    modifier auth() {

35:    function isAuthorized(address src) internal view returns (bool) {

46:    function add(uint256 x, uint256 y) internal pure returns (uint256 z) {

50:    function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {

54:    function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {

58:    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {

62:    function max(uint256 x, uint256 y) internal pure returns (uint256 z) {

66:    function imin(int256 x, int256 y) internal pure returns (int256 z) {

70:    function imax(int256 x, int256 y) internal pure returns (int256 z) {

77:    function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {

81:    function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {

85:    function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

89:    function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

152:    event emitTake(

174:    event emitCancel(

209:    event emitDelete(

234:    struct OfferInfo {

260:    modifier can_offer() virtual {

264:    modifier synchronized() {

272:    function makerFee() public view returns (uint256) {

276:    function isActive(uint256 id) public view returns (bool active) {

280:    function getOwner(uint256 id) public view returns (address owner) {

284:    function getRecipient(uint256 id) public view returns (address owner) {

288:    function getOffer(

297:    function bump(bytes32 id_) external can_buy(uint256(id_)) {

314:    function buy(

452:    function cancel(

487:    function kill(bytes32 id) external virtual {

491:    function make(

511:    function offer(

564:    function take(bytes32 id, uint128 maxTakeAmount) external virtual {

568:    function _next_id() internal returns (uint256) {

574:    function getFeeBPS() public view returns (uint256) {

578:    function calcAmountAfterFee(

610:    modifier can_cancel(uint256 id) virtual override {

620:    function isClosed() public pure returns (bool closed) {

624:    function getTime() public view returns (uint64) {

628:    function stop() external auth {

633:    contract DSNote {

634:    event LogNote(

643:    modifier note() {

660:    contract MatchingEvents {

662:    event LogMinSell(address pay_gem, uint256 min_amount);

664:    event LogUnsortedOffer(uint256 id);

665:    event LogSortedOffer(uint256 id);

667:    event LogDelete(address keeper, uint256 id);

668:    event LogMatch(uint256 id, uint256 amount);

686:    struct sortInfo {

700:    function initialize(address _feeTo) public {

719:    modifier can_cancel(uint256 id) override {

733:    function make(

752:    function take(bytes32 id, uint128 maxTakeAmount) public override {

756:    function kill(bytes32 id) external override {

761:    function offer(

780:    function offer(

802:    function offer(

824:    function offer(

855:    function buy(

871:    function cancel(

887:    function batchOffer(

910:    function batchCancel(uint[] calldata ids) external {

917:    function batchRequote(

937:    function del_rank(uint256 id) external returns (bool) {

955:    function setMinSell(

965:    function getMinSell(

974:    function getBestOffer(

985:    function getWorseOffer(uint256 id) public view returns (uint256) {

993:    function getBetterOffer(uint256 id) external view returns (uint256) {

998:    function getOfferCount(

1010:    function getFirstUnsortedOffer() public view returns (uint256) {

1016:    function getNextUnsortedOffer(uint256 id) public view returns (uint256) {

1020:    function isOfferSorted(uint256 id) public view returns (bool) {

1028:    function sellAllAmount(

1069:    function buyAllAmount(

1115:    function getBuyAmountWithFee(

1124:    function getBuyAmount(

1149:    function getPayAmountWithFee(

1157:    function getPayAmount(

1183:    function _buys(uint256 id, uint256 amount) internal returns (bool) {

1208:    function _find(uint256 id) internal view returns (uint256) {

1225:    function _findpos(uint256 id, uint256 pos) internal view returns (uint256) {

1261:    function _isPricedLtOrEq(

1273:    function _matcho(

1362:    function _sort(

1405:    function _unsort(

1438:    function _hide(

1466:    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {

1471:    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {

1476:    function setFeeTo(address newFeeTo) external auth returns (bool) {

1482:    function getFeeTo() external view returns (address) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

```solidity
12:   contract BathHouseV2 {

26:    modifier onlyAdmin() {

32:    function initialize(address _comptroller, address _pAdmin) external {

// @param and @return missing
45:    function getBathTokenFromAsset(

51:    function whoIsBuddy(

60:    function createBathToken(

115:    function claimRewards(

131:    function getReward(address buddy, address rewardsToken) external {

137:    function _bathify(
```

---

### State variable and function names [4]

- Variables should be named according to their specifications
- private and internal `variables` should preppend with `underline`
- private and internal `functions` should preppend with `underline`
- constant state variables should be UPPER_CASE

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
//  private and internal `functions` should preppend with `underline`
125:    function openPosition(
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
// private and internal `functions` should preppend with `underline`
35:    function isAuthorized(address src) internal view returns (bool) {
46:    function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
50:    function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {
54:    function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
58:    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
62:    function max(uint256 x, uint256 y) internal pure returns (uint256 z) {
66:    function imin(int256 x, int256 y) internal pure returns (int256 z) {
70:    function imax(int256 x, int256 y) internal pure returns (int256 z) {
77:    function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
81:    function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
85:    function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {
89:    function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

// private and internal `variables` should preppend with `underline`
227:    uint256 internal feeBPS;
230:    address internal feeTo;

// remove the pre-fixed underline from non private/internal variables
691:    mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
692:    mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
693:    mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook
694:    mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
695:    mapping(uint256 => uint256) public _near; //next unsorted offer id
696:    uint256 public _head; //first unsorted offer id
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

```solidity
18:    bool private initialized;
20:    mapping(address => address) private tokenToBathToken;
21:    mapping(address => address) private bathTokenToBuddy;
```

---

### Version [5]

- Pragma versions should be standardized and avoid floating pragma `( ^ )`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
// lose the ^ for safer code. Also this version is not similar to other files using 0.8.17
2:  pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
// lose the ^ for safer code. Also this version is not similar to other files using 0.8.17
2:  pragma solidity ^0.8.9;
```


---

### Change to camelCase [6]

-  Several state variables, functions, modifiers and params are using snake_case but Solidity is more of a camelCase pattern

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
// change to `camelCase`
66:    function imin(int256 x, int256 y) internal pure returns (int256 z) {
70:    function imax(int256 x, int256 y) internal pure returns (int256 z) {
77:    function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
81:    function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
85:    function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {
89:    function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

// events are conventionally PascalCase, ex: EmitOffer
118:    event emitOffer(
152:    event emitTake(
174:    event emitCancel(
198:    event emitFee(
209:    event emitDelete(

// change to `camelCase`
219:    uint256 public last_offer_id;
245:    modifier can_buy(uint256 id) virtual {
251:    modifier can_cancel(uint256 id) virtual {
260:    modifier can_offer() virtual {
492:        ERC20 pay_gem,
493:        ERC20 buy_gem,
494:        uint128 pay_amt,
495:        uint128 buy_amt
512:        uint256 pay_amt,
513:        ERC20 pay_gem,
514:        uint256 buy_amt,
515:        ERC20 buy_gem,
568:    function _next_id() internal returns (uint256) {
597:    modifier can_offer() override {
603:    modifier can_buy(uint256 id) override {
610:    modifier can_cancel(uint256 id) virtual override {
734:        ERC20 pay_gem,
735:        ERC20 buy_gem,
736:        uint128 pay_amt,
737:        uint128 buy_amt
762:        uint256 pay_amt, //maker (ask) sell how much
763:        ERC20 pay_gem, //maker (ask) sell which token
764:        uint256 buy_amt, //maker (ask) buy how much
781:        uint256 pay_amt, //maker (ask) sell how much
782:        ERC20 pay_gem, //maker (ask) sell which token
783:        uint256 buy_amt, //maker (ask) buy how much
784:        ERC20 buy_gem, //maker (ask) buy which token
803:        uint256 pay_amt, //maker (ask) sell how much
804:        ERC20 pay_gem, //maker (ask) sell which token
805:        uint256 buy_amt, //maker (ask) buy how much
806:        ERC20 buy_gem, //maker (ask) buy which token
825:        uint256 pay_amt, //maker (ask) sell how much
826:        ERC20 pay_gem, //maker (ask) sell which token
827:        uint256 buy_amt, //maker (ask) buy how much
828:        ERC20 buy_gem, //maker (ask) buy which token
956:        ERC20 pay_gem, //token to assign minimum sell amount to
966:        ERC20 pay_gem //token for which minimum sell amount is queried
975:        ERC20 sell_gem,
976:        ERC20 buy_gem
999:        ERC20 sell_gem,
1000:        ERC20 buy_gem
1029:        ERC20 pay_gem,
1030:        uint256 pay_amt,
1031:        ERC20 buy_gem,
1032:        uint256 min_fill_amount
1033:    ) external returns (uint256 fill_amt) {
1070:        ERC20 buy_gem,
1071:        uint256 buy_amt,
1072:        ERC20 pay_gem,
1073:        uint256 max_fill_amount
1074:    ) external returns (uint256 fill_amt) {
1116:        ERC20 buy_gem,
1117:        ERC20 pay_gem,
1118:        uint256 pay_amt
1119:    ) external view returns (uint256 fill_amt) {
1125:        ERC20 buy_gem,
1126:        ERC20 pay_gem,
1127:        uint256 pay_amt
1128:    ) public view returns (uint256 fill_amt) {
1150:        ERC20 pay_gem,
1151:        ERC20 buy_gem,
1152:        uint256 buy_amt
1153:    ) public view returns (uint256 fill_amt) {
1158:        ERC20 pay_gem,
1159:        ERC20 buy_gem,
1160:        uint256 buy_amt
1161:    ) public view returns (uint256 fill_amt) {

// change function name to _findPos (camelCase)
1225:    function _findpos(uint256 id, uint256 pos) internal view returns (uint256) {

// change to `camelCase`
1274:        uint256 t_pay_amt, //taker sell how much
1275:        ERC20 t_pay_gem, //taker sell which token
1276:        uint256 t_buy_amt, //taker buy how much
1277:        ERC20 t_buy_gem, //taker buy which token
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol

```solidity
// change to `camelCase`
48:    function rubicall(
60:    function _rubicall(
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
// in variable names set first letters to lower case
682:    bool public AqueductDistributionLive;
684:    address public AqueductAddress;
```

### Observations [7]

<!-- go through all the codebase again to remove useless commented code -->

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
// remove these useless commented code

4:  // Uncomment this line to use console.log
5:// import "hardhat/console.sol";
16:    /// event LogSetAuthority(address indexed authority); /// TODO: this event is not used in the contract, remove?
99:    /// TODO: double check it is sound logic to kill this event
100:    /// event LogTrade(
107:    /// event LogMake(
128:    /// TODO: double check it is sound logic to kill this event
129:    /// event LogBump(
140:    /// event LogTake(
163:    /// event LogKill(
184:    /// TODO: double check it is sound logic to kill this event
185:    /// event LogInt(string lol, uint256 input);
187:    /// event FeeTake(
207:    /// event OfferDeleted(bytes32 indexed id);
299:        /// TODO: double check it is sound logic to kill this event
300:        /// emit LogBump(
386:        /// emit LogTake(
409:        /// emit FeeTake(
428:        /// TODO: double check it is sound logic to kill this event
429:        /// emit LogTrade(
464:        /// emit LogKill(
542:        /// emit LogMake(
661:    /// event LogBuyEnabled(bool isEnabled); /// TODO: this event is not used in the contract, remove?
663:    /// event LogMatchingEnabled(bool isEnabled); /// TODO: this event is not used in the contract, remove?
666:    /// event LogInsert(address keeper, uint256 id); /// TODO: this event is not used in the contract, remove?
850:        // }
851:        // return super.offer(pay_amt, pay_gem, buy_amt, buy_gem);
1346:    // function _offeru(
1380:            //offers[id] is not the highest offer
1381:            //requirement below is satisfied by statements above
1382:            //require(_isPricedLtOrEq(id, pos));
1387:            //offers[id] is the highest offer
1395:            //require(!_isPricedLtOrEq(id, prev_id));
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25-L28

```solidity
    // check if owner_ is not equal to address(0) and if it is the desired new owner by adding a whitelist so the new owner is not set the wrong address 
    function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
    }
```


