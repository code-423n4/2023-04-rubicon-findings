## GAS OPTIMIZATIONS

##

## [G-1] State variables only set in the constructor should be declared immutable

Avoids a Gsset (20000 gas) in the constructor, and replaces the first access in each transaction (Gcoldsload - 2100 gas) and each access thereafter (Gwarmacces - 100 gas) with a PUSH32 (3 gas).

While strings are not value types, and therefore cannot be immutable/constant if not hard-coded outside of the constructor, the same behavior can be achieved by making the current contract abstract with virtual functions for the string accessors, and having a child contract override the functions with the hard-coded implementation-specific values.

```solidity
FILE : 2023-04-rubicon/contracts/V2Migrator.sol

/// @audit v1ToV2Pools (constructor)
33: v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
```
[V2Migrator.sol#L33](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L33)

```solidity
FILE : 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

/// @audit oracle (constructor),rubiconMarket (constructor),bathHouseV2 (constructor),comptroller (constructor)
55: oracle = PriceOracle(_oracle);
56: rubiconMarket = RubiconMarket(_rubiconMarket);
57: bathHouseV2 = BathHouseV2(_bathHouseV2);
58: comptroller = bathHouseV2.comptroller();

```
[Position.sol#L55-L58](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L55-L58)


##

## [G-2] State variables can be packed into fewer storage slots

If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables can also be cheaper

```solidity

FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

219: uint256 public last_offer_id; // 32 bytes  SLOT-1
222: mapping(uint256 => OfferInfo) public offers; // 32 bytes  SLOT-2
224: bool locked; // 1 bytes  SLOT-3
227: uint256 internal feeBPS; // 32 bytes  SLOT-4
230: address internal feeTo; // 20 bytes   SLOT-5

> Currently total of 5 slots occupied 

```
[RubiconMarket.sol#L219-L230](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L219-L230)

### After Mitigations 

```solidity

FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

  219: uint256 public last_offer_id; // 32 bytes  SLOT-1
  222: mapping(uint256 => OfferInfo) public offers; // 32 bytes  SLOT-2
- 224: bool locked; // 1 bytes  
  227: uint256 internal feeBPS; // 32 bytes  SLOT-3
  230: address internal feeTo; // 20 bytes   SLOT-4
+ 224: bool locked; // 1 bytes  SLOT-4

```
### After mitigation total slots 4 . Saved 1 slot and saves Gsset (20000 gas)

##

## [G-3] Structs can be packed into fewer storage slots

Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct.

```solidity
FILE : 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

/// @audit Variable ordering with 5 slots instead of the current 6 :
///        address(20):asset,address(20):quote,bool(1):isActive,uint256(32):borrowedAmount,uint256(32):bathTokenAmount, uint256(32):blockNum
  33:         struct Position {
  34:         address asset; 
  35:         address quote;
+ 39:         bool isActive; 
  36:         uint256 borrowedAmount; 
  37:         uint256 bathTokenAmount; 
  38:         uint256 blockNum; 
- 39:         bool isActive; 
  40:         }
```
[Position.sol#L33-L40](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L33-L40)

```solidity
FILE : 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

/// @audit Variable ordering with 3 slots instead of the current 4 :
///   bytes4(4):selector, address(20):target ,bytes(32):target, struct(dynamic) : feeParams 
  12: struct CallParams {
  13:      bytes4 selector; 
  14:      bytes args; 
  15:      address target; 
  16:      FeeParams feeParams;
  17:    } 

```
[FeeWrapper.sol#L12-L17](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L12-L17)

## [G-4] Use a more recent version of solidity

One of the main ways that Solidity reduces gas costs is through the use of more efficient bytecode. The Solidity compiler generates optimized bytecode that uses fewer instructions and consumes less gas than previous versions. This optimization can result in significant gas savings, particularly for contracts with complex logic or large data structures

- Inline assembly
- Storage layout optimization
- Better gas estimation
-Low-level data access

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

3: pragma solidity ^0.8.9;
```

```solidity
FILE : 2023-04-rubicon/contracts/periphery/BathBuddy.sol

2: pragma solidity ^0.8.0;
```

``` solidity
FILE : 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

2: pragma solidity 0.8.17;
```
##

## [G-5] Optimize names to save gas

public/external function names and public member variable names can be optimized to save gas. See this [link](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

> public/external function names

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

/// @audit setOwner()
15: contract DSAuthEvents {

/// @audit makerFee(),isActive(),getOwner(),getRecipient(),getOffer(),bump(),getFeeBPS(),calcAmountAfterFee(),
218: contract SimpleMarket is EventfulMarket, DSMath {

/// @audit isClosed(),getTime(),stop(),
593: contract ExpiringMarket is DSAuth, SimpleMarket {

/// @audit initialize(),offer(),batchOffer(),batchCancel(),batchRequote(),del_rank(),setMinSell(),getMinSell(),getBestOffer(),getWorseOffer(),getBetterOffer(),getOfferCount(),getFirstUnsortedOffer(),getNextUnsortedOffer(),isOfferSorted(),sellAllAmount(),buyAllAmount(),getBuyAmountWithFee(),getPayAmountWithFee(),getPayAmount(),setMakerFee(),setFeeTo(),getFeeTo
674: contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
```
```solidity
FILE : 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

/// @audit calculateFee(),rubicall(),
8: contract FeeWrapper {
```
```solidity
FILE : 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

/// @audit borrowBalance(),borrowBalanceOfPos(),borrowRate(),buyAllAmountWithLeverage(),sellAllAmountWithLeverage(),closePosition(),increaseMargin(),withdraw(),
15: contract Position is Ownable, DSMath {
```
```solidity
FILE : 2023-04-rubicon/contracts/periphery/BathBuddy.sol

/// @audit spawnBuddy(),lastTimeRewardApplicable(),getRewardForDuration(),notifyRewardAmount(),setRewardsDuration(),
38: contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
```
```solidity
FILE : 2023-04-rubicon/contracts/V2Migrator.sol

/// @audit migrate()
17: contract V2Migrator {
```
```solidity
FILE : 2023-04-rubicon/contracts/BathHouseV2.sol

/// @audit initialize(),getBathTokenFromAsset(),whoIsBuddy(),createBathToken(),claimRewards(),getReward(),
12: contract BathHouseV2 {
```

##

## [G-6] Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to [not having to recalculate the key’s keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

```solidity
FILE : 2023-04-rubicon/contracts/BathHouseV2.sol

20: mapping(address => address) private tokenToBathToken;
21: mapping(address => address) private bathTokenToBuddy;
```
[BathHouseV2.sol#L20-L21](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L20-L21)

```solidity
FILE: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

52: mapping(address => uint256) public periodFinish; // Token specific
53: mapping(address => uint256) public rewardRates; // Token specific reward rates
54: mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
55: mapping(address => uint256) public lastUpdateTime; //Token specific
56: mapping(address => uint256) public rewardsPerTokensStored; // Token specific
59: mapping(address => mapping(address => uint256))
        public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
61: mapping(address => mapping(address => uint256)) public tokenRewards;
```
[BathBuddy.sol#L52-L61](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L52-L61)

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

691: mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
692: mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
693: mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair 
     in sorted orderbook
694: mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
695: mapping(uint256 => uint256) public _near; //next unsorted offer id
```
[RubiconMarket.sol#L691-L695](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L691-L695)

##

## [G-7] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

25:   function setOwner(address owner_) external auth {
1466: function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
1471: function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
1476: function setFeeTo(address newFeeTo) external auth returns (bool) {

```
[RubiconMarket.sol#L25](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25),[RubiconMarket.sol#L1466](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466)

(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L955-L958)

```solidity
FILE: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

   function buyAllAmountWithLeverage(
        address quote,
        address asset,
        uint256 quotePayAmount,
        uint256 leverage
    ) external onlyOwner {

function sellAllAmountWithLeverage(
        address asset,
        address quote,
        uint256 assetPayAmount,
        uint256 leverage
    ) external onlyOwner {

210: function closePosition(uint256 posId) external onlyOwner {
226: function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {
242: function withdraw(address token, uint256 amount) external onlyOwner {

```
[Position.sol#L93-L98](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L93-L98),[Position.sol#L107-L112](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L107-L112),[Position.sol#L210](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L210),[Position.sol#L226](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L226),[Position.sol#L242](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L242)

```solidity
FILE: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

function getReward(
        IERC20 rewardsToken,
        address holderRecipient
    )
        external
        override
        nonReentrant
        whenNotPaused
        updateReward(holderRecipient, address(rewardsToken))
        onlyBathHouse

function notifyRewardAmount(
        uint256 reward,
        IERC20 rewardsToken
    ) external onlyOwner updateReward(address(0), address(rewardsToken)) {

function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner {

```
[BathBuddy.sol#L168-L177](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L168-L177)
##

## [G-8] Do not calculate constants

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

232: bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```
[RubiconMarket.sol#L232](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L232)

##

## [G-9] internal/Private functions or Modifiers only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```solidity 
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

modifier auth() {
        require(isAuthorized(msg.sender), "ds-auth-unauthorized");
        _;
    }

function isAuthorized(address src) internal view returns (bool) {
        if (src == owner) {
            return true;
        } else {
            return false;
        }
    }

```
[RubiconMarket.sol#L30-L42](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L30-L42)

```solidity
FILE: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

modifier onlyBathHouse() {
        require(msg.sender == bathHouse, "You are not my beloved bath house!");
        _;
    }

```
[BathBuddy.sol#L104-L107](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L104-L107)

```solidity
FILE : FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

35: function isAuthorized(address src) internal view returns (bool) {
46: function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
50: function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {
54: function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
58: function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
62: function max(uint256 x, uint256 y) internal pure returns (uint256 z) {
66: function imin(int256 x, int256 y) internal pure returns (int256 z) {
70: function imax(int256 x, int256 y) internal pure returns (int256 z) {
77: function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
81: function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
85: function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {
89: function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

```

##

## [G-10] NOT USING THE NAMED RETURN VARIABLES WHEN A FUNCTION RETURNS, WASTES DEPLOYMENT GAS

It is true that not using the named return variables when a function returns wastes deployment gas.

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

276: function isActive(uint256 id) public view returns (bool active) {
280: function getOwner(uint256 id) public view returns (address owner) {
284: function getRecipient(uint256 id) public view returns (address owner) {
```
[RubiconMarket.sol#L276](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L276),[RubiconMarket.sol#L280](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L280),[RubiconMarket.sol#L284](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L284)

[RubiconMarket.sol#L491-L496](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L491-L496)

[RubiconMarket.sol#L620-L622](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620-L622)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L870-L873)
()
()
()
()
()
()

##

## [G-11] Setting the constructor to payable

You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of msg.value == 0 and saves 13 gas on deployment with no security risks

```solidity
FILE: 2023-04-rubicon/contracts/V2Migrator.sol

30: constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
```
[V2Migrator.sol#L30](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30)

```solidity
FILE : 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

54: constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
```
[Position.sol#L54](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L54)

##

## [G-12] Use assembly to write address storage values

```solidity
FILE : 2023-04-rubicon/contracts/BathHouseV2.sol

34: comptroller = Comptroller(_comptroller);
35: admin = msg.sender;
36: proxyAdmin = _pAdmin;
```
[BathHouseV2.sol#L34-L36](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L34-L36)

```solidity
FILE: 2023-04-rubicon/contracts/V2Migrator.sol

33:v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
```
[V2Migrator.sol#L33](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L33)

```solidity
FILE : 2023-04-rubicon/contracts/periphery/BathBuddy.sol

77: owner = _owner;
78: myBathTokenBuddy = newBud;
79: bathHouse = _bathHouse;
```
[BathBuddy.sol#L77-L79](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L77-L79)

```solidity
FILE : 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

55: oracle = PriceOracle(_oracle);
56: rubiconMarket = RubiconMarket(_rubiconMarket);
57: bathHouseV2 = BathHouseV2(_bathHouseV2);
58: comptroller = bathHouseV2.comptroller();
```
[Position.sol#L55-L58](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L55-L58)

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

26: owner = owner_;
```
[RubiconMarket.sol#L26](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L26)

##

## [G-13] Use nested if and, avoid multiple check combinations

Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

349: if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
```
[RubiconMarket.sol#L349](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L349)

##

## [G-14] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contracts gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

(https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html)

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

494: uint128 pay_amt,
495: uint128 buy_amt

564: function take(bytes32 id, uint128 maxTakeAmount) external virtual {

736: uint128 pay_amt,
737: uint128 buy_amt

```
[RubiconMarket.sol#L494-L495](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L494-L495),[RubiconMarket.sol#L564](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L564),

##

## [G-15] Splitting require() statements that use && saves gas

See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper by 3 gas

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

893:   require(
894:            payAmts.length == payGems.length &&
895:                payAmts.length == buyAmts.length &&
896:                payAmts.length == buyGems.length,
897:            "Array lengths do not match"
898:        );


 940: require(
            !isActive(id) &&
                _rank[id].delb != 0 &&
                _rank[id].delb < block.number - 10
        );

```
[RubiconMarket.sol#L893-L898](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L893-L898),[RubiconMarket.sol#L940-L944](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L940-L944)

```solidity
FILE: rubicon/contracts/periphery/BathBuddy.sol

require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );



```
[BathBuddy.sol#L95-L100](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L95-L100)

##

## [G-16] Add unchecked {} for subtractions where the operands cannot underflow

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

The block.number is not going to less than 10

943: _rank[id].delb < block.number - 10
```
[RubiconMarket.sol#L943](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L943)

##

## [G-17] Repeated same value assignments for buyEnabled, matchingEnabled varibales in initialize() function 

The buyEnabled, matchingEnabled values already set true when declaring state variables. Inside initialize() function again the buyEnabled, matchingEnabled values set to true. This is waste of work and consumes large volume of gas. The values should be set true only if the buyEnabled, matchingEnabled variables value is false otherwise we can simply skip this assignments 

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol
675: bool public buyEnabled = true; //buy enabled TODO: review this decision!
676: bool public matchingEnabled = true; //true: enable matching,

714: matchingEnabled = true;
715: buyEnabled = true;

```
[RubiconMarket.sol#L675-L676](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L675-L676)

## [G-18] For events use 3 indexed rule to save gas 

Need to declare 3 indexed fields for event parameters. If the event parameter is less than 3 should declare all event parameters indexed 

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

97:  event LogItemUpdate(uint256 id);
662: event LogMinSell(address pay_gem, uint256 min_amount);
664: event LogUnsortedOffer(uint256 id);
665: event LogSortedOffer(uint256 id);
667: event LogDelete(address keeper, uint256 id);
668: event LogMatch(uint256 id, uint256 amount);
```
[RubiconMarket.sol#L97](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L97)

```solidity 
FILE: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

50: event PositionOpened(uint256 positionId, Position position);
51: event PositionClosed(uint256 positionId);
52: event MarginIncreased(uint256 positionId, uint256 amount);

```
[Position.sol#L50-L52](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L50-L52)

```solidity
FILE: rubicon/contracts/periphery/BathBuddy.sol

event RewardAdded(uint256 reward);
event Staked(address indexed user, uint256 amount);
event Withdrawn(address indexed user, uint256 amount);
event RewardPaid(address indexed user, uint256 reward);
event RewardsDurationUpdated(uint256 newDuration);
event Recovered(address token, uint256 amount);

##
[BathBuddy.sol#L261-L266](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L261-L266)

## [G-19] Modifiers not called by contract should be removed to save deployment cost 

```solidity
FILE: rubicon/contracts/periphery/BathBuddy.sol

modifier onlyBuddy() {
        require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );
        _;
    }

```
[BathBuddy.sol#L94-L102](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L94-L102)

##

## [G-20] 



public functions not called by contract
avoid contract existence check
internal functions only called once can be inlined to save gas 
Don’t compare boolean expressions to boolean literals
Ternary unnecessary

GAS-1	Use assembly to check for address(0)	14
GAS-2	Using bools for storage incurs overhead	7
GAS-3	Cache array length outside of loop	6
GAS-4	State variables should be cached in stack variables rather than re-reading them from storage	2
GAS-5	Use calldata instead of memory for function arguments that do not get mutated	7
GAS-6	Use Custom Errors	36
GAS-7	Don't initialize variables with default value	8
GAS-8	Long revert strings	7
GAS-9	Pre-increments and pre-decrements are cheaper than post-increments and post-decrements	8
GAS-10	Use shift Right/Left instead of division/multiplication if possible	4
GAS-11	Use storage instead of memory for structs/arrays	12
GAS-12	Increments can be unchecked in for-loops	7
GAS-13	Use != 0 instead of > 0 for unsigned integer comparison	21
GAS-14	internal functions not called by the contract should be removed	4