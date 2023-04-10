## Gas Optimizations

|        | Issue                                                                                                                                               |
| ------ | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| GAS-1  | `<X> += <Y>`/`<X> -= <Y>` COSTS MORE GAS THAN `<X> = <X> + <Y>`/`<X> = <X> - <Y>` FOR STATE VARIABLES                                               |
| GAS-2  | BEFORE SOME FUNCTIONS, WE SHOULD CHECK SOME VARIABLES FOR POSSIBLE GAS SAVE                                                                         |
| GAS-3  | SETTING THE CONSTRUCTOR TO PAYABLE                                                                                                                  |
| GAS-4  | DIVISION OR MULTIPLY BY TWO SHOULD USE BIT SHIFTING                                                                                                 |
| GAS-5  | DUPLICATED REQUIRE()/REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION (INSTANCES)                                                     |
| GAS-6  | DOS WITH BLOCK GAS LIMIT                                                                                                                            |
| GAS-7  | USE FUNCTION INSTEAD OF MODIFIERS                                                                                                                   |
| GAS-8  | INSTEAD OF CALCULATING A STATEVAR WITH KECCAK256() EVERY TIME THE CONTRACT IS MADE PRE CALCULATE THEM BEFORE AND ONLY GIVE THE RESULT TO A CONSTANT |
| GAS-9  | INTERNAL FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE REMOVED TO SAVE DEPLOYMENT GAS                                                              |
| GAS-10 | INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS                                                                                      |
| GAS-11 | CONSTANT VALUES SUCH AS A CALL TO KECCAK256(), SHOULD USE IMMUTABLE RATHER THAN CONSTANT                                                            |
| GAS-12 | KECCAK256() SHOULD ONLY NEED TO BE CALLED ON A SPECIFIC STRING LITERAL ONCE                                                                         |
| GAS-13 | MODIFIERS ARE REDUNDANT IF USED ONLY ONCE OR NOT USED AT ALL                                                                                        |
| GAS-14 | MULTIPLE ADDRESS/ID MAPPINGS CAN BE COMBINED INTO A SINGLE MAPPING OF AN ADDRESS/ID TO A STRUCT, WHERE APPROPRIATE                                  |
| GAS-15 | FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE                                                                    |
| GAS-16 | OPTIMIZE NAMES TO SAVE GAS                                                                                                                          |
| GAS-17 | THE INCREMENT IN FOR LOOP POSTCONDITION CAN BE MADE UNCHECKED                                                                                       |
| GAS-18 | PROPER DATA TYPES                                                                                                                                   |
| GAS-19 | PUBLIC FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE DECLARED EXTERNAL INSTEAD                                                                     |
| GAS-20 | USE A MORE RECENT VERSION OF SOLIDITY                                                                                                               |
| GAS-21 | REORDER STRUCTURE LAYOUT                                                                                                                            |
| GAS-22 | `REQUIRE()` OR `REVERT()` STATEMENTS THAT CHECK INPUT ARGUMENTS SHOULD BE AT THE TOP OF THE FUNCTION                                                |
| GAS-23 | ADD `UNCHECKED {}` FOR SUBTRACTIONS WHERE THE OPERANDS CANNOT UNDERFLOW BECAUSE OF A PREVIOUS `REQUIRE()`, `REVERT` OR `IF` STATEMENT               |
| GAS-24 | SPLITTING REQUIRE() STATEMENTS THAT USE && SAVES GAS                                                                                                |
| GAS-25 | STATE VARIABLES ONLY SET IN THE CONSTRUCTOR SHOULD BE DECLARED IMMUTABLE                                                                            |
| GAS-26 | USING STORAGE INSTEAD OF MEMORY FOR STRUCTS/ARRAYS SAVES GAS                                                                                        |
| GAS-27 | STRUCTS CAN BE PACKED INTO FEWER STORAGE SLOTS                                                                                                      |
| GAS-28 | TERNARY OPERATION IS CHEAPER THAN IF-ELSE STATEMENT                                                                                                 |
| GAS-29 | USAGE OF `UINT`/`INT` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD                                                                              |
| GAS-30 | UNNECESSARY LIBRARIES                                                                                                                               |
| GAS-31 | USE BYTES32 INSTEAD OF STRING                                                                                                                       |
| GAS-32 | CAN MAKE THE VARIABLE OUTSIDE THE LOOP TO SAVE GAS                                                                                                  |

### [GAS-1] `<X> += <Y>`/`<X> -= <Y>` COSTS MORE GAS THAN `<X> = <X> + <Y>`/`<X> = <X> - <Y>` FOR STATE VARIABLES

#### Description:

Using the addition operator instead of plus-equals saves gas

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

583:         _amount -= mul(amount, feeBPS) / 100_000;

586:             _amount -= mul(amount, makerFee()) / 100_000;

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

184:             vars.currentBathTokenAmount += _borrowLoop(

431:         positions[_positionId].bathTokenAmount += _bathTokenAmount;

560:             _assetAmount += _loopBorrowed;

```

### [GAS-2] BEFORE SOME FUNCTIONS, WE SHOULD CHECK SOME VARIABLES FOR POSSIBLE GAS SAVE

#### Description:

Before transfer, we should check for amount being 0 so the function doesnt run when its not gonna do anything.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

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
File: contracts/utilities/FeeWrapper.sol

100:         IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);

102:         IERC20(_feeToken).transfer(_feeTo, _feeAmount);

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

493:             IERC20(_asset).transferFrom(

```

### [GAS-3] SETTING THE CONSTRUCTOR TO PAYABLE

#### Description:

Saves ~13 gas per instance

#### **Proof Of Concept**

```solidity
File: contracts/V2Migrator.sol

30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

54:     constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {

```

### [GAS-4] DIVISION OR MULTIPLY BY TWO SHOULD USE BIT SHIFTING

#### Description:

`<x> / 2` is the same as `<x> >> 1`. While the compiler uses the `SHR` opcode to accomplish both, the version that uses division incurs an overhead of 20 gas due to `JUMP`s to and from a compiler utility function that introduces checks which can be avoided by using `unchecked {}` around the division by two.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

78:         z = add(mul(x, y), WAD / 2) / WAD;

82:         z = add(mul(x, y), RAY / 2) / RAY;

86:         z = add(mul(x, WAD), y / 2) / y;

90:         z = add(mul(x, RAY), y / 2) / y;

```

### [GAS-5] DUPLICATED REQUIRE()/REVERT() CHECKS SHOULD BE REFACTORED TO A MODIFIER OR FUNCTION (INSTANCES)

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

246:         require(isActive(id));

252:         require(isActive(id));

265:         require(!locked);

488:         require(cancel(uint256(id)));

524:         require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6

524:         require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6

565:         require(buy(uint256(id), maxTakeAmount));

598:         require(!isClosed());

604:         require(isActive(id));

605:         require(!isClosed());

611:         require(isActive(id));

753:         require(buy(uint256(id), maxTakeAmount));

757:         require(cancel(uint256(id)));

834:         require(!locked, "Reentrancy attempt");

835:         require(_dust[address(pay_gem)] <= pay_amt);

859:         require(!locked, "Reentrancy attempt");

874:         require(!locked, "Reentrancy attempt");

938:         require(!locked);

1034:         require(!locked);

1075:         require(!locked);

1080:             require(offerId != 0, "offerId == 0");

1136:                 require(offerId != 0); //Fails if there are not enough offers to complete

1169:                 require(offerId != 0); //Fails if there are not enough offers to complete

1209:         require(id > 0);

1226:         require(id > 0);

1366:         require(isActive(id));

```

```solidity
File: contracts/periphery/BathBuddy.sol

122:         require(friendshipStarted, "I have not started a bathToken friendship");

143:         require(friendshipStarted, "I have not started a bathToken friendship");

```

### [GAS-6] DOS WITH BLOCK GAS LIMIT

#### Description:

When smart contracts are deployed or functions inside them are called, the execution of these actions always requires a certain amount of gas, based of how much computation is needed to complete them. The Ethereum network specifies a block gas limit and the sum of all transactions included in a block can not exceed the threshold.

Programming patterns that are harmless in centralized applications can lead to Denial of Service conditions in smart contracts when the cost of executing a function exceeds the block gas limit. Modifying an array of unknown size, that increases in size over time, can lead to such a Denial of Service condition.

Reference: https://swcregistry.io/docs/SWC-128

This loops could drain all user gas and revert.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

122:    for (uint256 i = 0; i < buddies.length; ++i) {

```

```solidity
File: contracts/RubiconMarket.sol

899:    for (uint i = 0; i < payAmts.length; i++) {

911:    for (uint i = 0; i < ids.length; i++) {

924:    for (uint i = 0; i < ids.length; i++) {


```

```solidity
File: contracts/V2Migrator.sol

31:     for (uint256 i = 0; i < bathTokensV1.length; ++i) {

```

```solidity
File: contracts/utilities/FeeWrapper.sol

40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {

```

#### Recommended Mitigation Steps:

Caution is advised when you expect to have large arrays that grow over time. Actions that require looping across the entire data structure should be avoided.

If you absolutely must loop over an array of unknown size, then you should plan for it to potentially take multiple blocks, and therefore require multiple transactions.

### [GAS-7] USE FUNCTION INSTEAD OF MODIFIERS

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

26:     modifier onlyAdmin() {

```

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

```solidity
File: contracts/periphery/BathBuddy.sol

87:     modifier onlyOwner() {

94:     modifier onlyBuddy() {

104:     modifier onlyBathHouse() {

247:     modifier updateReward(address account, address token) {

```

### [GAS-8] INSTEAD OF CALCULATING A STATEVAR WITH KECCAK256() EVERY TIME THE CONTRACT IS MADE PRE CALCULATE THEM BEFORE AND ONLY GIVE THE RESULT TO A CONSTANT

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```

### [GAS-9] INTERNAL FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE REMOVED TO SAVE DEPLOYMENT GAS

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

62:     function max(uint256 x, uint256 y) internal pure returns (uint256 z) {

66:     function imin(int256 x, int256 y) internal pure returns (int256 z) {

70:     function imax(int256 x, int256 y) internal pure returns (int256 z) {

77:     function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {

1183:     function _buys(uint256 id, uint256 amount) internal returns (bool) {

```

### [GAS-10] INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS

#### Description:

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

35:     function isAuthorized(address src) internal view returns (bool) {

568:     function _next_id() internal returns (uint256) {

1225:     function _findpos(uint256 id, uint256 pos) internal view returns (uint256) {

```

```solidity
File: contracts/utilities/FeeWrapper.sol

93:     function _chargeFee(FeeParams memory _feeParams, address _target) internal {

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

272:     function _borrow(address _cToken, uint256 _amount) internal {

280:     function _repay(address _asset, address _quote, uint256 _posId) internal {

336:     function _enterMarkets(address _bathToken) internal {

343:     function _exitMarket(address _bathToken) internal {

382:     function _redeem(address _asset, uint256 _bathTokenAmount) internal {

421:     function _removePosition(uint256 _positionId) internal {

```

### [GAS-11] CONSTANT VALUES SUCH AS A CALL TO KECCAK256(), SHOULD USE IMMUTABLE RATHER THAN CONSTANT

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```

### [GAS-12] KECCAK256() SHOULD ONLY NEED TO BE CALLED ON A SPECIFIC STRING LITERAL ONCE

#### Description:

It should be saved to an immutable variable, and the variable used instead. If the hash is being used as a part of a function selector, the cast to bytes4 should also only be done once.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```

### [GAS-13] MODIFIERS ARE REDUNDANT IF USED ONLY ONCE OR NOT USED AT ALL

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

26:     modifier onlyAdmin() {

```

```solidity
File: contracts/RubiconMarket.sol

643:     modifier note() {

```

```solidity
File: contracts/periphery/BathBuddy.sol

94:     modifier onlyBuddy() {

104:     modifier onlyBathHouse() {

```

### [GAS-14] MULTIPLE ADDRESS/ID MAPPINGS CAN BE COMBINED INTO A SINGLE MAPPING OF AN ADDRESS/ID TO A STRUCT, WHERE APPROPRIATE

#### Description:

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

If both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

222:     mapping(uint256 => OfferInfo) public offers;

691:     mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids

692:     mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair

693:     mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook

```

```solidity
File: contracts/periphery/BathBuddy.sol

52:     mapping(address => uint256) public periodFinish; // Token specific

53:     mapping(address => uint256) public rewardRates; // Token specific reward rates

54:     mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific

55:     mapping(address => uint256) public lastUpdateTime; //Token specific

56:     mapping(address => uint256) public rewardsPerTokensStored; // Token specific

61:     mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC

```

#### Recommended Mitigation Steps:

Make mapping a struct instead

### [GAS-15] FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

#### Description:

If a function modifier such as onlyOwner/onlyX is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

66:     ) external onlyAdmin {

```

```solidity
File: contracts/periphery/BathBuddy.sol

87:     modifier onlyOwner() {

94:     modifier onlyBuddy() {

104:     modifier onlyBathHouse() {

177:         onlyBathHouse

194:     ) external onlyOwner updateReward(address(0), address(rewardsToken)) {

235:     ) external onlyOwner {

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

98:     ) external onlyOwner {

112:     ) external onlyOwner {

210:     function closePosition(uint256 posId) external onlyOwner {

226:     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242:     function withdraw(address token, uint256 amount) external onlyOwner {

```

### [GAS-16] OPTIMIZE NAMES TO SAVE GAS

#### Description:

`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. In this report are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92).

[Solidity optimize name github](https://github.com/enzosv/solidity-optimize-name)

[Source](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

12: contract BathHouseV2 {

```

```solidity
File: contracts/RubiconMarket.sol

15: contract DSAuthEvents {

22: contract DSAuth is DSAuthEvents {

45: contract DSMath {

96: contract EventfulMarket {

218: contract SimpleMarket is EventfulMarket, DSMath {

593: contract ExpiringMarket is DSAuth, SimpleMarket {

633: contract DSNote {

660: contract MatchingEvents {

674: contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {

701:         require(!initialized, "contract is already initialized");

```

```solidity
File: contracts/V2Migrator.sol

8: interface IBathToken is IERC20 {

17: contract V2Migrator {

```

```solidity
File: contracts/periphery/BathBuddy.sol

9: interface IBathBuddy {

38: contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {

```

```solidity
File: contracts/utilities/FeeWrapper.sol

8: contract FeeWrapper {

15:         address target; // target contract to call

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

15: contract Position is Ownable, DSMath {

```

### [GAS-17] THE INCREMENT IN FOR LOOP POSTCONDITION CAN BE MADE UNCHECKED

#### Description:

This is only relevant if you are using the default solidity checked arithmetic.

The for loop postcondition, i.e., `i++` involves checked arithmetic, which is not required. This is because the value of i is always strictly less than `length <= 2**256 - 1`. Therefore, the theoretical maximum value of i to enter the for-loop body is `2**256 - 2`. This means that the `i++` in the for loop can never overflow. Regardless, the overflow checks are performed by the compiler.

Unfortunately, the Solidity optimizer is not smart enough to detect this and remove the checks. One can manually do this.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

569:         last_offer_id++;

899:         for (uint i = 0; i < payAmts.length; i++) {

911:         for (uint i = 0; i < ids.length; i++) {

924:         for (uint i = 0; i < ids.length; i++) {

1400:         _span[address(pay_gem)][address(buy_gem)]++;

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

407:         lastPositionId++;

572:                 _limit++;

576:                 _limit++;

580:                 _limit++;

```

### [GAS-18] PROPER DATA TYPES

#### Description:

In Solidity, some data types are more expensive than others. It’s important to be aware of the most efficient type that can be used. Here are a few rules about data types.

Type uint should be used in place of type string whenever possible.

Type uint256 takes less gas to store than uint8

Type bytes should be used over byte[]

If the length of bytes can be limited, use the lowest amount possible from bytes1 to bytes32.

Type bytes32 is cheaper to use than type string and bytes.

If data can fit into 32 bytes, then you should use bytes32 datatype rather than bytes or strings as it is cheaper in solidity.

[Source](https://betterprogramming.pub/how-to-write-smart-contracts-that-optimize-gas-spent-on-ethereum-30b5e9c5db85)

Fixed size variables are always cheaper than dynamic ones.

[Source](https://medium.com/coinmonks/gas-optimization-in-solidity-part-i-variables-9d5775e43dde)

Most of the time it will be better to use a mapping instead of an array because of its cheaper operations.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

82:         (string memory name, string memory symbol, uint8 decimals) = _bathify(

142:         returns (string memory _name, string memory _symbol, uint8 _decimals)

```

```solidity
File: contracts/RubiconMarket.sol

124:         uint128 pay_amt,

125:         uint128 buy_amt

159:         uint128 take_amt,

160:         uint128 give_amt

180:         uint128 pay_amt,

181:         uint128 buy_amt

240:         uint64 timestamp;

321:         require(uint128(spend) == spend, "spend is not an int");

322:         require(uint128(quantity) == quantity, "quantity is not an int");

405:             uint128(quantity),

406:             uint128(spend)

480:             uint128(_offer.pay_amt),

481:             uint128(_offer.buy_amt)

494:         uint128 pay_amt,

495:         uint128 buy_amt

519:         require(uint128(pay_amt) == pay_amt);

520:         require(uint128(buy_amt) == buy_amt);

534:         info.timestamp = uint64(block.timestamp);

559:             uint128(pay_amt),

560:             uint128(buy_amt)

564:     function take(bytes32 id, uint128 maxTakeAmount) external virtual {

624:     function getTime() public view returns (uint64) {

625:         return uint64(block.timestamp);

635:         bytes4 indexed sig,

640:         bytes fax

736:         uint128 pay_amt,

737:         uint128 buy_amt

1093:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1103:                 take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need

```

```solidity
File: contracts/utilities/FeeWrapper.sol

13:         bytes4 selector; // function selector

```

### [GAS-19] PUBLIC FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE DECLARED EXTERNAL INSTEAD

#### Description:

The following functions could be set external to save gas and improve code quality. External call cost is less expensive than of public functions.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

574:     function getFeeBPS() public view returns (uint256) {

624:     function getTime() public view returns (uint64) {

700:     function initialize(address _feeTo) public {

1010:     function getFirstUnsortedOffer() public view returns (uint256) {

1016:     function getNextUnsortedOffer(uint256 id) public view returns (uint256) {

```

### [GAS-20] USE A MORE RECENT VERSION OF SOLIDITY

#### Description:

Using at least `0.8.10` will save gas due to skipped extcodesize check if there is a return value. Currently the contracts are compiled using different versions `^0.8.17`, `^0.8.19`, `^0.8.0`.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

2: pragma solidity 0.8.17;

```

```solidity
File: contracts/RubiconMarket.sol

2: pragma solidity ^0.8.9;

```

```solidity
File: contracts/V2Migrator.sol

2: pragma solidity 0.8.17;

```

```solidity
File: contracts/periphery/BathBuddy.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: contracts/utilities/FeeWrapper.sol

2: pragma solidity 0.8.17;

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

2: pragma solidity 0.8.17;

```

### [GAS-21] REORDER STRUCTURE LAYOUT

#### Description:

Structures could be optimized moving the position of certain values in order to save a lot slots.

For example Enums are represented by integers; the possibility listed first by 0, the next by 1, and so forth. An enum type just acts like uintN, where N is the smallest legal value large enough to accomodate all the possibilities.

[Source](https://ethdebug.github.io/solidity-data-representation)

[Source](https://docs.soliditylang.org/en/v0.8.17/internals/layout_in_storage.html#storage-inplace-encoding)

#### **Proof Of Concept**

```solidity
File: contracts/utilities/FeeWrapper.sol

12:     struct CallParams {
            bytes4 selector; // function selector
            bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
            address target; // target contract to call
            FeeParams feeParams;
}

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

33:     struct Position {
            address asset; // supplied as collateral
            address quote; // borrowed token
            uint256 borrowedAmount; // amount of borrowed quote
            uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
            uint256 blockNum; // block number on which position was opened
            bool isActive; // false by default, when active - true
    }

```

### [GAS-22] `REQUIRE()` OR `REVERT()` STATEMENTS THAT CHECK INPUT ARGUMENTS SHOULD BE AT THE TOP OF THE FUNCTION

#### Description:

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (2100 gas\*) in a function that may ultimately revert in the unhappy case.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

322:         require(uint128(quantity) == quantity, "quantity is not an int");

```

### [GAS-23] ADD `UNCHECKED {}` FOR SUBTRACTIONS WHERE THE OPERANDS CANNOT UNDERFLOW BECAUSE OF A PREVIOUS `REQUIRE()`, `REVERT` OR `IF` STATEMENT

#### Description:

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block [see resource](https://github.com/ethereum/solidity/issues/10695)

`require(a <= b); x = b - a => require(a <= b); unchecked { x = b - a }`

`if(a <= b); x = b - a => if(a <= b); unchecked { x = b - a }`

This will stop the check for overflow and underflow so it will save gas

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

581:         require(amount > 0);
            _amount = amount;
            _amount -= mul(amount, feeBPS) / 100_000;

            if (makerFee() > 0) {
                _amount -= mul(amount, makerFee()) / 100_000;
            }

```

### [GAS-24] SPLITTING REQUIRE() STATEMENTS THAT USE && SAVES GAS

#### Description:

Instead of using operator `&&` on a single `require`. Using a two `require` can save more gas.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

253:       require(
            (msg.sender == getOwner(id)) ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
        );



612:    require(
            (msg.sender == getOwner(id)) ||
                isClosed() ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
        );

725:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0)),

893:     equire(
            payAmts.length == payGems.length &&
                payAmts.length == buyAmts.length &&
                payAmts.length == buyGems.length,
            "Array lengths do not match"
        );

940:    require(
            !isActive(id) &&
                _rank[id].delb != 0 &&
                _rank[id].delb < block.number - 10
        );

1412:   require(
            _rank[id].delb == 0 && //assert id is in the sorted list
                isOfferSorted(id)
        );

```

```solidity
File: contracts/periphery/BathBuddy.sol

95:     require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

591:        ? require(
                _leverage > _wad && _leverage <= _leverageMax,
                "_leverageCheck{Long}: INVLAID LEVERAGE"
            )
            : require(
                _leverage >= _wad && _leverage <= _leverageMax,
                "_leverageCheck{Short}: INVLAID LEVERAGE"
            );

```

### [GAS-25] STATE VARIABLES ONLY SET IN THE CONSTRUCTOR SHOULD BE DECLARED IMMUTABLE

#### Description:

Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving SLOAD. Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around 20 000 gas per variable) and replace the expensive storage-reading operations (around 2100 gas per reading) to a less expensive value reading (3 gas)

#### **Proof Of Concept**

```solidity
File: contracts/utilities/poolsUtility/Position.sol

        rubiconMarket = RubiconMarket(_rubiconMarket);
        comptroller = bathHouseV2.comptroller();

```

### [GAS-26] USING STORAGE INSTEAD OF MEMORY FOR STRUCTS/ARRAYS SAVES GAS

#### Description:

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional `MLOAD` rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read.

The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol


116:         address[] memory buddies,

117:         address[] memory rewardsTokens

```

```solidity
File: contracts/utilities/FeeWrapper.sol

29:         uint256[] memory tokenAmounts,

35:         returns (uint256[] memory amountsWithFee, uint256[] memory fees)

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

337:         address[] memory _bathTokens = new address[](1);

339:         uint256[] memory _errs = comptroller.enterMarkets(_bathTokens);

```

### [GAS-27] STRUCTS CAN BE PACKED INTO FEWER STORAGE SLOTS

#### Description:

Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings.

#### **Proof Of Concept**

```solidity
File: contracts/utilities/FeeWrapper.sol

12:        struct CallParams {
                bytes4 selector; // function selector
                bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
                address target; // target contract to call
                FeeParams feeParams;
    }

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

23:     struct PosVars {

33:         struct Position {
                address asset; // supplied as collateral
                address quote; // borrowed token
                uint256 borrowedAmount; // amount of borrowed quote
                uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
                uint256 blockNum; // block number on which position was opened
                bool isActive; // false by default, when active - true
    }

```

### [GAS-28] TERNARY OPERATION IS CHEAPER THAN IF-ELSE STATEMENT

#### Description:

There are instances where a ternary operation can be used instead of if-else statement. In these cases, using ternary operation will save modest amounts of gas.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

36:     if (src == owner) {
            return true;
        } else {
            return false;
        }

876:        if (isOfferSorted(id)) {
                require(_unsort(id));
            } else {
                require(_hide(id), "can't hide");
            }

1186:       if (isOfferSorted(id)) {
                //offers[id] must be removed from sorted list because all of it is bought
                _unsort(id);
            } else {
                _hide(id);
            }

```

```solidity
File: contracts/utilities/FeeWrapper.sol

51:    if (msg.value == 0) {
            return _rubicall(params);
        } else {
            return _rubicallPayable(params);
        }

```

### [GAS-29] USAGE OF `UINT`/`INT` SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD

#### Description:

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the uint8, as well as the associated stack operations of doing so. Use a larger size then downcast where needed.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

82:         (string memory name, string memory symbol, uint8 decimals) = _bathify(

142:         returns (string memory _name, string memory _symbol, uint8 _decimals)

```

```solidity
File: contracts/RubiconMarket.sol

240:         uint64 timestamp;

534:         info.timestamp = uint64(block.timestamp);

624:     function getTime() public view returns (uint64) {

625:         return uint64(block.timestamp);

```

### [GAS-30] UNNECESSARY LIBRARIES

#### Description:

Libraries are often only imported for a small number of uses, meaning that they can contain a significant amount of code that is redundant to your contract. If you can safely and effectively implement the functionality imported from a library within your contract, it is optimal to do so.
[Source](https://betterprogramming.pub/how-to-write-smart-contracts-that-optimize-gas-spent-on-ethereum-30b5e9c5db85)

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

import "./compound-v2-fork/InterestRateModel.sol";
import "./compound-v2-fork/CErc20Delegator.sol";
import "./compound-v2-fork/Comptroller.sol";
import "./compound-v2-fork/Unitroller.sol";
import "./periphery/BathBuddy.sol";

```

```solidity
File: contracts/V2Migrator.sol

import "./compound-v2-fork/CTokenInterfaces.sol";

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

import "../../compound-v2-fork/Comptroller.sol";
import "../../compound-v2-fork/PriceOracle.sol";
import "../../BathHouseV2.sol";
import "../../RubiconMarket.sol";

```

```solidity
File: contracts/utilities/FeeWrapper.sol

import "./RubiconRouter.sol";

```

### [GAS-31] USE BYTES32 INSTEAD OF STRING

#### Description:

Use bytes32 instead of string to save gas whenever possible. String is a dynamic data structure and therefore is more gas consuming then bytes32.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

82:         (string memory name, string memory symbol, uint8 decimals) = _bathify(

82:         (string memory name, string memory symbol, uint8 decimals) = _bathify(

142:         returns (string memory _name, string memory _symbol, uint8 _decimals)

142:         returns (string memory _name, string memory _symbol, uint8 _decimals)

146:         _name = string.concat("bath", ERC20(_underlying).symbol());

147:         _symbol = string.concat(_name, "v2");

```

### [GAS-32] CAN MAKE THE VARIABLE OUTSIDE THE LOOP TO SAVE GAS

#### Description:

Make it outside and only use it inside.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

899:         for (uint i = 0; i < payAmts.length; i++) {

911:         for (uint i = 0; i < ids.length; i++) {

924:         for (uint i = 0; i < ids.length; i++) {

```
