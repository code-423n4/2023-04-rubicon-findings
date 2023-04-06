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

FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

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

FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

```solidity
3: pragma solidity ^0.8.9;
```

FILE : 2023-04-rubicon/contracts/periphery/BathBuddy.sol

```solidity
2: pragma solidity ^0.8.0;
```

FILE : 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

``` solidity
2: pragma solidity 0.8.17;
```
##

## [G-5] Optimize names to save gas

public/external function names and public member variable names can be optimized to save gas. See this [link](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

> public/external function names

```solidity
/// @audit setOwner()
15: contract DSAuthEvents {

/// @audit makerFee(),isActive(),getOwner(),getRecipient(),getOffer(),bump(),getFeeBPS(),calcAmountAfterFee(),
218: contract SimpleMarket is EventfulMarket, DSMath {

/// @audit isClosed(),getTime(),stop(),
593: contract ExpiringMarket is DSAuth, SimpleMarket {

/// @audit initialize(),offer(),batchOffer(),batchCancel(),batchRequote(),del_rank(),setMinSell(),getMinSell(),getBestOffer(),getWorseOffer(),getBetterOffer(),getOfferCount(),getFirstUnsortedOffer(),getNextUnsortedOffer(),isOfferSorted(),sellAllAmount(),buyAllAmount(),getBuyAmountWithFee(),getPayAmountWithFee(),getPayAmount(),setMakerFee(),setFeeTo(),getFeeTo
674: contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
```

FILE : 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

```solidity
/// @audit calculateFee(),rubicall(),
8: contract FeeWrapper {
```
FILE : 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

```solidity

/// @audit borrowBalance(),borrowBalanceOfPos(),borrowRate(),buyAllAmountWithLeverage(),sellAllAmountWithLeverage(),closePosition(),increaseMargin(),withdraw(),
15: contract Position is Ownable, DSMath {
```

FILE : 2023-04-rubicon/contracts/periphery/BathBuddy.sol

```solidity
/// @audit spawnBuddy(),lastTimeRewardApplicable(),getRewardForDuration(),notifyRewardAmount(),setRewardsDuration(),
38: contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
```

FILE : 2023-04-rubicon/contracts/V2Migrator.sol

```solidity
/// @audit migrate()
17: contract V2Migrator {
```

FILE : 2023-04-rubicon/contracts/BathHouseV2.sol

```solidity
/// @audit initialize(),getBathTokenFromAsset(),whoIsBuddy(),createBathToken(),claimRewards(),getReward(),
12: contract BathHouseV2 {
```

##

## [G-6] Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to [not having to recalculate the key’s keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

FILE : 2023-04-rubicon/contracts/BathHouseV2.sol

```solidity
20: mapping(address => address) private tokenToBathToken;
21: mapping(address => address) private bathTokenToBuddy;
```
[BathHouseV2.sol#L20-L21](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L20-L21)

FILE: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

```solidity
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

FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

```solidity
691: mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
692: mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
693: mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair 
     in sorted orderbook
694: mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
695: mapping(uint256 => uint256) public _near; //next unsorted offer id
```
[RubiconMarket.sol#L691-L695](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L691-L695)







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