### Table Of Contents
- [FINDINGS](#findings)
- [Using immutable on variables that are only set in the constructor and never after `(Gas saved: 8.4k)`](#using-immutable-on-variables-that-are-only-set-in-the-constructor-and-never-after-gas-saved-84k)
  - [The variables shown below can be marked as immutable](#the-variables-shown-below-can-be-marked-as-immutable)
- [Use constants for variables that are never changed`(Saves: 4K gas)`](#use-constants-for-variables-that-are-never-changedsaves-4k-gas)
  - [buyEnabled and matchingEnabled can be declared as constants(Saves 2 SLOTS: 4k Gas)](#buyenabled-and-matchingenabled-can-be-declared-as-constantssaves-2-slots-4k-gas)
- [The modifier synchronized simply acts as non reentrant modifier.`(~45k gas saved)`](#the-modifier-synchronized-simply-acts-as-non-reentrant-modifier45k-gas-saved)
  - [Massive 15k per tx gas savings - use Openzeppelin or solmate for Reentrancy guard](#massive-15k-per-tx-gas-savings---use-openzeppelin-or-solmate-for-reentrancy-guard)
- [Tighly pack storage variables/optimize the order of variable declaration`(Gas saved: 2K )`](#tighly-pack-storage-variablesoptimize-the-order-of-variable-declarationgas-saved-2k-)
  - [Pack bool locked with address internal feeTo(Saves 1 SLOT: 2k gas)](#pack-bool-locked-with-address-internal-feetosaves-1-slot-2k-gas)
- [Pack structs by putting data types in ascending size`(Gas saved: 2K)`](#pack-structs-by-putting-data-types-in-ascending-sizegas-saved-2k)
  - [Position.sol.Position{}: Pack bool isActive with address asset(Saves 1 SLOT: 2k gas)](#positionsolposition-pack-bool-isactive-with-address-assetsaves-1-slot-2k-gas)
- [The result of a function call should be cached rather than re-calling the function](#the-result-of-a-function-call-should-be-cached-rather-than-re-calling-the-function)
  - [RubiconMarket.sol.buy(): Results of makerFee() should be cached](#rubiconmarketsolbuy-results-of-makerfee-should-be-cached)
  - [RubiconMarket.sol.calcAmountAfterFee(): Results of makerFee() should be cached](#rubiconmarketsolcalcamountafterfee-results-of-makerfee-should-be-cached)
  - [RubiconMarket.sol.can\_cancel(): Result of getOwner(id) should be cached](#rubiconmarketsolcan_cancel-result-of-getownerid-should-be-cached)
  - [BathBuddy.sol.rewardPerToken(): Results of IERC20(myBathTokenBuddy).totalSupply() should be cached(happy path)](#bathbuddysolrewardpertoken-results-of-ierc20mybathtokenbuddytotalsupply-should-be-cachedhappy-path)
- [IF's/require() statements that check input arguments should be at the top of the function](#ifsrequire-statements-that-check-input-arguments-should-be-at-the-top-of-the-function)
  - [Reorder the statements to check the function parameter before loading the storage variable(mapping)](#reorder-the-statements-to-check-the-function-parameter-before-loading-the-storage-variablemapping)
  - [Check for functional parameters that don't involve casting first](#check-for-functional-parameters-that-dont-involve-casting-first)
  - [Cheaper to check for functional parameters rather than storage variable](#cheaper-to-check-for-functional-parameters-rather-than-storage-variable)
  - [Reorder the statements to check for functional parameter validity before reading from storage](#reorder-the-statements-to-check-for-functional-parameter-validity-before-reading-from-storage)
  - [Cheaper to validate function parameters rather than validate storage variables](#cheaper-to-validate-function-parameters-rather-than-validate-storage-variables)
  - [The require statement can be called earlier before assigning the other two variables](#the-require-statement-can-be-called-earlier-before-assigning-the-other-two-variables)
- [Internal/Private functions only called once can be inlined to save gas(Gas saved: ~480 gas)](#internalprivate-functions-only-called-once-can-be-inlined-to-save-gasgas-saved-480-gas)
- [Emitting storage values instead of the memory one.](#emitting-storage-values-instead-of-the-memory-one)
  - [BathBuddy.sol.setRewardsDuration(): should emit \_rewardsDuration](#bathbuddysolsetrewardsduration-should-emit-_rewardsduration)
- [Multiple accesses of a mapping/array should use a local variable cache](#multiple-accesses-of-a-mappingarray-should-use-a-local-variable-cache)
  - [BathBuddy.sol.lastTimeRewardApplicable(): periodFinish\[token\] should be cached in local storage(happy path)](#bathbuddysollasttimerewardapplicable-periodfinishtoken-should-be-cached-in-local-storagehappy-path)
  - [BathBuddy.sol.notifyRewardAmount(): periodFinish\[address(rewardsToken)\] should be cached in local storage(Sad path)](#bathbuddysolnotifyrewardamount-periodfinishaddressrewardstoken-should-be-cached-in-local-storagesad-path)
  - [BathBuddy.sol.notifyRewardAmount(): rewardsDuration\[address(rewardsToken)\] should be cached in local storage](#bathbuddysolnotifyrewardamount-rewardsdurationaddressrewardstoken-should-be-cached-in-local-storage)
  - [RubiconMarket.sol.sellAllAmount(): offers\[offerId\] should be cached in local storage](#rubiconmarketsolsellallamount-offersofferid-should-be-cached-in-local-storage)
  - [RubiconMarket.sol.\_find(): offers\[id\] should be cached in local storage](#rubiconmarketsol_find-offersid-should-be-cached-in-local-storage)
  - [RubiconMarket.sol.\_isPricedLtOrEq(): offers\[low\] and offers\[high\] should be cached in local storage](#rubiconmarketsol_ispricedltoreq-offerslow-and-offershigh-should-be-cached-in-local-storage)
  - [RubiconMarket.sol.\_matcho(): offers\[best\_maker\_id\] should be cached in local storage](#rubiconmarketsol_matcho-offersbest_maker_id-should-be-cached-in-local-storage)
  - [RubiconMarket.sol.\_sort(): offers\[id\] should be cached](#rubiconmarketsol_sort-offersid-should-be-cached)
  - [RubiconMarket.sol.\_unsort(): offers\[id\] should be cached](#rubiconmarketsol_unsort-offersid-should-be-cached)
- [Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate](#multiple-address-mappings-can-be-combined-into-a-single-mapping-of-an-address-to-a-struct-where-appropriate)
- [Duplicated require()/revert() checks should be refactored to a modifier or function](#duplicated-requirerevert-checks-should-be-refactored-to-a-modifier-or-function)
- [Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead](#usage-of-uintsints-smaller-than-32-bytes-256-bits-incurs-overhead)
- [Use the cached value here instead of reading from storage again](#use-the-cached-value-here-instead-of-reading-from-storage-again)
  - [RubiconMarket.sol.\_hide(): uid should be used(saves 1 SLOAD)](#rubiconmarketsol_hide-uid-should-be-usedsaves-1-sload)
- [Splitting require() statements that use \&\& saves gas - (saves 8 gas per \&\&)](#splitting-require-statements-that-use--saves-gas---saves-8-gas-per-)
- [A modifier used only once and not being inherited should be inlined to save gas](#a-modifier-used-only-once-and-not-being-inherited-should-be-inlined-to-save-gas)
- [Functions guaranteed to revert when called by normal users can be marked `payable`](#functions-guaranteed-to-revert-when-called-by-normal-users-can-be-marked-payable)
- [Setting the constructor to payable](#setting-the-constructor-to-payable)
    - [Description:](#description)
- [Cheaper to read a constant directly rather than cache it(Gas saved: 5)](#cheaper-to-read-a-constant-directly-rather-than-cache-itgas-saved-5)
- [Do not calculate constants variables](#do-not-calculate-constants-variables)
- [Cast only once instead of repeatedly casting values to same type](#cast-only-once-instead-of-repeatedly-casting-values-to-same-type)
  - [Position.sol.openPosition(): IERC20(asset) should be cast only once and result cached](#positionsolopenposition-ierc20asset-should-be-cast-only-once-and-result-cached)
  - [Position.sol.\_repay(): IERC20(\_quote) should be cast only once](#positionsol_repay-ierc20_quote-should-be-cast-only-once)
  - [Position.sol.\_marketBuy(): ERC20(\_asset) and ERC20(\_quote) should be cast once and cached](#positionsol_marketbuy-erc20_asset-and-erc20_quote-should-be-cast-once-and-cached)
  - [Position.sol.\_marketSell(): ERC20(\_asset) and ERC20(\_quote) should be cast only once and cached](#positionsol_marketsell-erc20_asset-and-erc20_quote-should-be-cast-only-once-and-cached)
  - [FeeWrapper.sol.\_chargeFee(): IERC20(\_feeToken) should be cast only once and use the cached value](#feewrappersol_chargefee-ierc20_feetoken-should-be-cast-only-once-and-use-the-cached-value)
  - [V2Migrator.sol.migrate(): bathTokenV2 should be cast to IERC20 once](#v2migratorsolmigrate-bathtokenv2-should-be-cast-to-ierc20-once)
- [SafeMath no longer required](#safemath-no-longer-required)

## FINDINGS
NB: Some functions have been truncated where necessary to just show affected parts of the code
Through out the report some places might be denoted with audit tags to show the actual place affected.

## Using immutable on variables that are only set in the constructor and never after `(Gas saved: 8.4k)`
Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving SLOAD.
Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around 20 000 gas per variable) and replace the expensive storage-reading operations (around 2100 gas per reading) to a less expensive value reading (3 gas)

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L44-L47
### The variables shown below can be marked as immutable
**Total instances : 4**
**(Gas saved per instance:  2.1k**
`Total saved: 2.1k * 4 = 8.4k)`

```solidity
File: /contracts/utilities/poolsUtility/Position.sol
44:    Comptroller public comptroller;
45:    PriceOracle public oracle;
46:    RubiconMarket public rubiconMarket;
47:    BathHouseV2 public bathHouseV2;
```

```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..b698e54 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -41,10 +41,10 @@ contract Position is Ownable, DSMath {
     // position id => Position struct
     mapping(uint256 => Position) public positions;

-    Comptroller public comptroller;
-    PriceOracle public oracle;
-    RubiconMarket public rubiconMarket;
-    BathHouseV2 public bathHouseV2;
+    Comptroller public immutable comptroller;
+    PriceOracle public immutable  oracle;
+    RubiconMarket public immutable rubiconMarket;
+    BathHouseV2 public immutable bathHouseV2;

```

## Use constants for variables that are never changed`(Saves: 4K gas)`
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L675-L676
### buyEnabled and matchingEnabled can be declared as constants(Saves 2 SLOTS: 4k Gas)
`Gas saved per SLOT: 2K`
`Total SlOTS saved: 2`
`Gas saved: 2k * 2 = 4k gas`

```solidity
File: /contracts/RubiconMarket.sol
675:    bool public buyEnabled = true; //buy enabled TODO: review this decision!
676:    bool public matchingEnabled = true; //true: enable matching,
```

The above two variables are being set during declaration. The only other time they are being allocated a value is when calling the function, `initialize`

```solidity
700:    function initialize(address _feeTo) public {
701:        require(!initialized, "contract is already initialized");

713:        initialized = true;
714:        matchingEnabled = true;
715:        buyEnabled = true;
716:    }
```

Note from the above function , the value being assigned is similar to what was assigned during declaration, which makes the second assignment redundant. We could omit the second assignment and declare our variables as constants with the value `true` which would also save us 2 storage slots 

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..12df3ae 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -672,8 +672,8 @@ contract MatchingEvents {
 /// @notice This contract is based on the original open-source work done by OasisDEX under the Apache License 2.0
 /// @dev This contract inherits the key trading functionality from SimpleMarket
 contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
-    bool public buyEnabled = true; //buy enabled TODO: review this decision!
-    bool public matchingEnabled = true; //true: enable matching,
+    bool public constant buyEnabled = true; //buy enabled TODO: review this decision!
+    bool public constant matchingEnabled = true; //true: enable matching,

     /// @dev Below is variable to allow for a proxy-friendly constructor
     bool public initialized;
@@ -711,8 +711,7 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
         feeBPS = 1;

         initialized = true;
-        matchingEnabled = true;
-        buyEnabled = true;
+
```

## The modifier synchronized simply acts as non reentrant modifier.`(~45k gas saved)`
In the instance shown below, we have a modifier `synchronized` but looking at the implementation, all it's doing is serve as non reentrant modifier.

As we are already importing the reentrancy guard from OZ in other files https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L5 we could make use of the same here. 

Other than for Quality assurance purpose , the change would net 15k gas savings on most transactions. 
### Massive 15k per tx gas savings - use Openzeppelin or solmate for Reentrancy guard
The modifier `synchronized` uses `true` and `false` unlike the versions found on OZ and solmate(Linked at the end).
Using `true` and `false` will trigger gas-refunds, which after London are 1/5 of what they used to be, meaning using `1` and `2` (keeping the slot non-zero), will cost 5k per change (5k + 5k) vs 20k + 5k, saving you 15k gas per function which uses the modifier

The modifier synchronized has been used on 3 functions as such we get a gas saving of

**Gas per function: 15k**
**Total instance: 3**
`Gas saved: 15k * 3 = 45K`

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L264-L269
```solidity
File: /contracts/RubiconMarket.sol
264:    modifier synchronized() {
265:        require(!locked);
266:        locked = true;
267:        _;
268:        locked = false;
269:    }
```

See solmate implementation(https://github.com/transmissions11/solmate/blob/main/src/utils/ReentrancyGuard.sol)
Open zeppelin implementation https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/ReentrancyGuard.sol



## Tighly pack storage variables/optimize the order of variable declaration`(Gas saved: 2K )`
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if the values combined are <= 32 bytes). If the variables packed together are retrieved together in functions we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)`.
Here, the storage variables can be tightly packed from:

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L219-L230
### Pack bool locked with address internal feeTo(Saves 1 SLOT: 2k gas)
```solidity
File: /contracts/RubiconMarket.sol
219:    uint256 public last_offer_id;

222:    mapping(uint256 => OfferInfo) public offers;

224:    bool locked;

227:    uint256 internal feeBPS;

230:    address internal feeTo;
```

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..825ea53 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -221,7 +221,6 @@ contract SimpleMarket is EventfulMarket, DSMath {
     /// @dev The mapping that makes up the core orderbook of the exchange
     mapping(uint256 => OfferInfo) public offers;

-    bool locked;

     /// @dev This parameter is in basis points
     uint256 internal feeBPS;
@@ -229,6 +228,8 @@ contract SimpleMarket is EventfulMarket, DSMath {
     /// @dev This parameter provides the address to which fees are sent
     address internal feeTo;

+    bool locked;
+
     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```

## Pack structs by putting data types in ascending size`(Gas saved: 2K)`

As the solidity EVM works with 32 bytes, variables less than 32 bytes should be packed inside a struct so that they can be stored in the same slot, this saves gas when writing to storage ~20000 gas

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L33-L40
### Position.sol.Position{}: Pack bool isActive with address asset(Saves 1 SLOT: 2k gas)
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
33:    struct Position {
34:        address asset; // supplied as collateral
35:        address quote; // borrowed token
36:        uint256 borrowedAmount; // amount of borrowed quote
37:        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
38:        uint256 blockNum; // block number on which position was opened
39:        bool isActive; // false by default, when active - true
40:    }
```

```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..acca7f4 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -31,12 +31,12 @@ contract Position is Ownable, DSMath {
     }

     struct Position {
+        bool isActive; // false by default, when active - true
         address asset; // supplied as collateral
         address quote; // borrowed token
         uint256 borrowedAmount; // amount of borrowed quote
         uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
         uint256 blockNum; // block number on which position was opened
-        bool isActive; // false by default, when active - true
     }
     // position id => Position struct
     mapping(uint256 => Position) public positions;
```

## The result of a function call should be cached rather than re-calling the function

Calling external functions can be expensive, rather than keep calling the same function on one function implementation,consider caching the results of the function and using the cached value instead.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L314-L346
### RubiconMarket.sol.buy(): Results of makerFee() should be cached
```solidity
File: /contracts/RubiconMarket.sol
314:    function buy(

345:        if (makerFee() > 0) {
346:            uint256 mFee = mul(spend, makerFee()) / 100_000;
```

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..ee999c4 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -342,8 +342,9 @@ contract SimpleMarket is EventfulMarket, DSMath {
         );

         // taker pay maker 0_0
-        if (makerFee() > 0) {
-            uint256 mFee = mul(spend, makerFee()) / 100_000;
+        uint256 _makerFee = makerFee();
+        if (_makerFee > 0) {
+            uint256 mFee = mul(spend, _makerFee) / 100_000;

```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L578-L589
### RubiconMarket.sol.calcAmountAfterFee(): Results of makerFee() should be cached
```solidity
File: /contracts/RubiconMarket.sol
578:    function calcAmountAfterFee(
579:        uint256 amount
580:    ) public view returns (uint256 _amount) {

585:        if (makerFee() > 0) {//@audit: Initial call
586:            _amount -= mul(amount, makerFee()) / 100_000;//@audit: 2nd call
587:        }
588:    }
```

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..261f928 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -582,8 +582,10 @@ contract SimpleMarket is EventfulMarket, DSMath {
         _amount = amount;
         _amount -= mul(amount, feeBPS) / 100_000;

-        if (makerFee() > 0) {
-            _amount -= mul(amount, makerFee()) / 100_000;
+        uint256 _makerFee = makerFee();
+
+        if (_makerFee > 0) {
+            _amount -= mul(amount, _makerFee) / 100_000;
         }
     }
 }
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L612-L616
### RubiconMarket.sol.can_cancel(): Result of getOwner(id) should be cached
```solidity
File: /contracts/RubiconMarket.sol
612:        require(
613:            (msg.sender == getOwner(id)) ||
614:                isClosed() ||
615:                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
616:        );
```

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..36ff647 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -609,10 +609,11 @@ contract ExpiringMarket is DSAuth, SimpleMarket {
     /// @dev After close, anyone can cancel an offer.
     modifier can_cancel(uint256 id) virtual override {
         require(isActive(id));
+        address _owner = getOwner(id);
         require(
-            (msg.sender == getOwner(id)) ||
+            (msg.sender == _owner) ||
                 isClosed() ||
-                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
+                (msg.sender == getRecipient(id) && _owner == address(0))
         );
         _;
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L721-L727
```solidity
File: /contracts/RubiconMarket.sol
721:        require(
722:            isClosed() ||
723:                msg.sender == getOwner(id) ||
724:                id == dustId ||
725:                (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
726:            "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
727:        );
```

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..0d02a66 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -718,11 +718,12 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
     // // After close, anyone can cancel an offer
     modifier can_cancel(uint256 id) override {
         require(isActive(id), "Offer was deleted or taken, or never existed.");
+        address _owner = getOwner(id);
         require(
             isClosed() ||
-                msg.sender == getOwner(id) ||
+                msg.sender == _owner ||
                 id == dustId ||
-                (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
+                (msg.sender == getRecipient(id) && _owner == address(0)),
             "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
         );
         _;
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L121-L135
### BathBuddy.sol.rewardPerToken(): Results of IERC20(myBathTokenBuddy).totalSupply() should be cached(happy path)
```solidity
File: /contracts/periphery/BathBuddy.sol
121:    function rewardPerToken(address token) public view returns (uint256) {
122:        require(friendshipStarted, "I have not started a bathToken friendship");


124:        if (IERC20(myBathTokenBuddy).totalSupply() == 0) {//@audit: 1st access
125:            return rewardsPerTokensStored[token];
126:        }
127:        return
128:            rewardsPerTokensStored[token].add(
129:                lastTimeRewardApplicable(token)
130:                    .sub(lastUpdateTime[token])
131:                    .mul(rewardRates[token])
132:                    .mul(1e18)
133:                    .div(IERC20(myBathTokenBuddy).totalSupply())//@audit:2nd access
134:            );
135:    }
```

```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..52536fd 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -121,7 +121,9 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
     function rewardPerToken(address token) public view returns (uint256) {
         require(friendshipStarted, "I have not started a bathToken friendship");

-        if (IERC20(myBathTokenBuddy).totalSupply() == 0) {
+        uint256 _bathTokenSupply = IERC20(myBathTokenBuddy).totalSupply();
+
+        if (_bathTokenSupply == 0) {
             return rewardsPerTokensStored[token];
         }
         return
@@ -130,7 +132,7 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
                     .sub(lastUpdateTime[token])
                     .mul(rewardRates[token])
                     .mul(1e18)
-                    .div(IERC20(myBathTokenBuddy).totalSupply())
+                    .div(_bathTokenSupply)
             );
     }
```


##  IF's/require() statements that check input arguments should be at the top of the function

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting alot of gas in a function that may ultimately revert in the unhappy case.

**Note: I've written some notes before every diff with some explanations if needed**

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L314-L322
### Reorder the statements to check the function parameter before loading the storage variable(mapping) 
```solidity
File: /contracts/RubiconMarket.sol
314:    function buy(
315:        uint256 id,
316:        uint256 quantity
317:    ) public virtual can_buy(id) synchronized returns (bool) {
318:        OfferInfo memory _offer = offers[id];
319:        uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;

321:        require(uint128(spend) == spend, "spend is not an int");
322:        require(uint128(quantity) == quantity, "quantity is not an int");
```
We are checking for a function parameter `quantity`  which would be cheaper to load compared to the operation ` OfferInfo memory _offer = offers[id];`. As we might revert on the check `require(uint128(quantity) == quantity, "quantity is not an int")` it would be better to revert without wasting gas 

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..9711b2e 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -315,11 +315,12 @@ contract SimpleMarket is EventfulMarket, DSMath {
         uint256 id,
         uint256 quantity
     ) public virtual can_buy(id) synchronized returns (bool) {
+        require(uint128(quantity) == quantity, "quantity is not an int");
+
         OfferInfo memory _offer = offers[id];
         uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;

         require(uint128(spend) == spend, "spend is not an int");
-        require(uint128(quantity) == quantity, "quantity is not an int");
```


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L511-L525
### Check for functional parameters that don't involve casting first
```solidity
File: /contracts/RubiconMarket.sol
511:    function offer(
512:        uint256 pay_amt,
513:        ERC20 pay_gem,
514:        uint256 buy_amt,
515:        ERC20 buy_gem,
516:        address owner,
517:        address recipient
518:    ) public virtual can_offer synchronized returns (uint256 id) {
519:        require(uint128(pay_amt) == pay_amt);
520:        require(uint128(buy_amt) == buy_amt);
521:        require(pay_amt > 0);
522:        require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
523:        require(buy_amt > 0);
524:        require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6
525:        require(pay_gem != buy_gem);
```

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..dae90d1 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -516,11 +516,11 @@ contract SimpleMarket is EventfulMarket, DSMath {
         address owner,
         address recipient
     ) public virtual can_offer synchronized returns (uint256 id) {
+        require(pay_amt > 0);
+        require(buy_amt > 0);
         require(uint128(pay_amt) == pay_amt);
         require(uint128(buy_amt) == buy_amt);
-        require(pay_amt > 0);
         require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
-        require(buy_amt > 0);
         require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6
         require(pay_gem != buy_gem);
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L700-L702
### Cheaper to check for functional parameters rather than storage variable
```solidity
File: /contracts/RubiconMarket.sol
700:    function initialize(address _feeTo) public {
701:        require(!initialized, "contract is already initialized");
702:        require(_feeTo != address(0));
```
The second require check is cheaper than the first. In case we end up reverting on the second we'd have wasted gas doing the first check. It's better to revert cheapely so reorder the require statements as below
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..4ed9b8c 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -698,9 +698,10 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {

     /// @dev Proxy-safe initialization of storage
     function initialize(address _feeTo) public {
-        require(!initialized, "contract is already initialized");
         require(_feeTo != address(0));

+        require(!initialized, "contract is already initialized");
```


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1438-L1444
### Reorder the statements to check for functional parameter validity before reading from storage
```solidity
File: /contracts/RubiconMarket.sol
1438:    function _hide(
1439:        uint256 id //id of maker offer to remove from unsorted list
1440:    ) internal returns (bool) {
1441:        uint256 uid = _head; //id of an offer in unsorted offers list
1442:        uint256 pre = uid; //id of previous offer in unsorted offers list

1444:        require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list
```
As we have a check for a function parameter, we should have it as the first operation in our function so that incase of a revert we don't end up wasting gas doing unnecessary expensive operations

```diff
     function _hide(
         uint256 id //id of maker offer to remove from unsorted list
     ) internal returns (bool) {
+
+        require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list
+
         uint256 uid = _head; //id of an offer in unsorted offers list
         uint256 pre = uid; //id of previous offer in unsorted offers list

-        require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L60-L79
### Cheaper to validate function parameters rather than validate storage variables
```solidity
File: /contracts/BathHouseV2.sol
60:    function createBathToken(
61:        address underlying,
62:        InterestRateModel interestRateModel,
63:        uint256 initialExchangeRateMantissa,
64:        address implementation,
65:        bytes memory becomeImplementationData
66:    ) external onlyAdmin {
67:        // underlying can be used only for one bathToken
68:        require(
69:            tokenToBathToken[underlying] == address(0),
70:            "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
71:        );
72:        require(
73:            underlying != address(0),
74:            "createBathToken: UNDERLYING == ADDRESS 0"
75:        );
76:        require(
77:            implementation != address(0),
78:            "createBathToken: IMPLEMENTATION == ADDRESS 0"
79:        );
```
We have a check that reads a storage value and other two checks for functional params, As we could revert on any of this require statements, it would be cheaper to check for functional params first as they are cheaper to read thus less gas consumed. Reorder the require statement as follows to have cheap checks first
```diff
diff --git a/contracts/BathHouseV2.sol b/contracts/BathHouseV2.sol
index 5ff6d4c..a9f594f 100644
--- a/contracts/BathHouseV2.sol
+++ b/contracts/BathHouseV2.sol
@@ -64,11 +64,7 @@ contract BathHouseV2 {
         address implementation,
         bytes memory becomeImplementationData
     ) external onlyAdmin {
-        // underlying can be used only for one bathToken
-        require(
-            tokenToBathToken[underlying] == address(0),
-            "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
-        );
+
         require(
             underlying != address(0),
             "createBathToken: UNDERLYING == ADDRESS 0"
@@ -77,6 +73,12 @@ contract BathHouseV2 {
             implementation != address(0),
             "createBathToken: IMPLEMENTATION == ADDRESS 0"
         );
+        // underlying can be used only for one bathToken
+        require(
+            tokenToBathToken[underlying] == address(0),
+            "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
+        );
+

```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L108-L115
### The require statement can be called earlier before assigning the other two variables
```solidity
File: /contracts/utilities/FeeWrapper.sol
108:    function _chargeFeePayable(
109:        FeeParams memory _feeParams
110:    ) internal returns (uint256 _msgValue) {
111:        // _feeToken is ETH
112:        uint256 _totalAmount = _feeParams.totalAmount;
113:        uint256 _feeAmount = _feeParams.feeAmount;
114:        address _feeTo = _feeParams.feeTo;
115:        require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");
```
Move the require statement above the two assignment operations.This way, if the require statement does not pass, we wouldn't end up wasting gas doing the assignments. The goal is to revert early and as cheaply as we can.
```diff
diff --git a/contracts/utilities/FeeWrapper.sol b/contracts/utilities/FeeWrapper.sol
index 88d6cfe..66dbbce 100644
--- a/contracts/utilities/FeeWrapper.sol
+++ b/contracts/utilities/FeeWrapper.sol
@@ -110,9 +110,10 @@ contract FeeWrapper {
     ) internal returns (uint256 _msgValue) {
         // _feeToken is ETH
         uint256 _totalAmount = _feeParams.totalAmount;
+        require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");
+
         uint256 _feeAmount = _feeParams.feeAmount;
         address _feeTo = _feeParams.feeTo;
-        require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");

```


## Internal/Private functions only called once can be inlined to save gas(Gas saved: ~480 gas)

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.
**Total instacens: 24**
**Gas per instance:20**
`Gas saved: 24 * 20 = 480`

Affected code:

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L35-L41
```solidity
File: /contracts/RubiconMarket.sol
35:    function isAuthorized(address src) internal view returns (bool) {
36:        if (src == owner) {
37:            return true;
38:        } else {
39:            return false;
40:        }
41:    }
```

The above function is only called on [Line 31](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L31) which is a modifier. We could just inline it 


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L568
```solidity
File: /contracts/RubiconMarket.sol
568:    function _next_id() internal returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1183
```solidity
File: /contracts/RubiconMarket.sol
1183:    function _buys(uint256 id, uint256 amount) internal returns (bool) {

1225:    function _findpos(uint256 id, uint256 pos) internal view returns (uint256) {

1273:    function _matcho(
1274:        uint256 t_pay_amt, //taker sell how much
1275:        ERC20 t_pay_gem, //taker sell which token
1276:        uint256 t_buy_amt, //taker buy how much
1277:        ERC20 t_buy_gem, //taker buy which token
1278:        uint256 pos, //position id
1279:        bool rounding, //match "close enough" orders?
1280:        address owner,
1281:        address recipient
1282:    ) internal returns (uint256 id) {


1362:    function _sort(
1363:        uint256 id, //maker (ask) id
1364:        uint256 pos //position to insert into
1365:    ) internal {
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L137-L143
```solidity
File: /contracts/BathHouseV2.sol
137:    function _bathify(
138:        address _underlying
139:    )
140:        internal
141:        view
142:        returns (string memory _name, string memory _symbol, uint8 _decimals)
143:    {
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L60
```solidity
File: /contracts/utilities/FeeWrapper.sol

60:    function _rubicall(
61:        CallParams memory _params
62:    ) internal returns (bytes memory) {

76:    function _rubicallPayable(
77:        CallParams memory _params
78:    ) internal returns (bytes memory) {


93:    function _chargeFee(FeeParams memory _feeParams, address _target) internal {


108:    function _chargeFeePayable(
109:        FeeParams memory _feeParams
110:    ) internal returns (uint256 _msgValue) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L251-L258
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
251:    function _borrowLoop(
252:        address _asset,
253:        address _quote,
254:        address _bathTokenAsset,
255:        address _bathTokenQuote,
256:        uint256 _amount,
257:        uint256 _toBorrow
258:    ) internal returns (uint256 _bathTokenAmount) {

272:    function _borrow(address _cToken, uint256 _amount) internal {

280:    function _repay(address _asset, address _quote, uint256 _posId) internal {

322:    function _calculateDebt(
323:        address _bathToken,
324:        uint256 _startBlock,
325:        uint256 _borrowedAmount
326:    ) internal view returns (uint256 _debt) {

336:    function _enterMarkets(address _bathToken) internal {

343:    function _exitMarket(address _bathToken) internal {

382:    function _redeem(address _asset, uint256 _bathTokenAmount) internal {

401:    function _savePosition(
402:        address _asset,
403:        address _quote,
404:        uint256 _borrowedAmount,
405:        uint256 _currentBathTokenAmount
406:    ) internal {

421:    function _removePosition(uint256 _positionId) internal {

426:    function _updateMargin(
427:        uint256 _positionId,
428:        uint256 _bathTokenAmount,
429:        uint256 _marginAmount
430:    ) internal {

454:    function _marketBuy(
455:        address _asset,
456:        address _quote,
457:        uint256 _maxFill
458:    ) internal {

475:    function _marketSell(
476:        address _asset,
477:        address _quote,
478:        uint256 _minFill
479:    ) internal {

526:    function _borrowLimit(
527:        address _bathToken,
528:        address _asset,
529:        uint256 _assetAmount,
530:        uint256 _leverage
531:    ) internal returns (uint256 _limit, uint256 _lastBorrow) {
```


## Emitting storage values instead of the memory one.
Here, the values emitted shouldn’t be read from storage. The existing memory values should be used instead:

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28
```solidity
File: /contracts/RubiconMarket.sol
25:    function setOwner(address owner_) external auth {
26:        owner = owner_;
27:        emit LogSetOwner(owner);
28:    }
```

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..31cc77d 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -24,7 +24,7 @@ contract DSAuth is DSAuthEvents {

     function setOwner(address owner_) external auth {
         owner = owner_;
-        emit LogSetOwner(owner);
+        emit LogSetOwner(owner_);
     }

```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L232-L242
### BathBuddy.sol.setRewardsDuration(): should emit \_rewardsDuration 
```solidity
File: /contracts/periphery/BathBuddy.sol
232:    function setRewardsDuration(
233:        uint256 _rewardsDuration,
234:        address token
235:    ) external onlyOwner {

240:        rewardsDuration[token] = _rewardsDuration;
241:        emit RewardsDurationUpdated(rewardsDuration[token]);
242:    }
```

```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..37570a7 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -238,7 +238,7 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
             "Previous rewards period must be complete before changing the duration for the new period"
         );
         rewardsDuration[token] = _rewardsDuration;
-        emit RewardsDurationUpdated(rewardsDuration[token]);
+        emit RewardsDurationUpdated(_rewardsDuration);
     }
```

## Multiple accesses of a mapping/array should use a local variable cache

Caching a mapping's value in a local storage or calldata variable when the value is accessed multiple times saves ~42 gas per access due to not having to perform the same offset calculation every time.
**Help the Optimizer by saving a storage variable's reference instead of repeatedly fetching it**

To help the optimizer,declare a storage type variable and use it instead of repeatedly fetching the reference in a map or an array. 
As an example, instead of repeatedly calling ```someMap[someIndex]```, save its reference like this: ```SomeStruct storage someStruct = someMap[someIndex]``` and use it.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L112-L119
### BathBuddy.sol.lastTimeRewardApplicable(): periodFinish[token] should be cached in local storage(happy path)
```solidity
File: /contracts/periphery/BathBuddy.sol
112:    function lastTimeRewardApplicable(

115:        return
116:            block.timestamp < periodFinish[token]//@audit: 1st access
117:                ? block.timestamp
118:                : periodFinish[token];//@audit: 2nd access
119:    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L191-L209
### BathBuddy.sol.notifyRewardAmount(): periodFinish\[address(rewardsToken)] should be cached in local storage(Sad path)
```solidity
File: /contracts/periphery/BathBuddy.sol
191:    function notifyRewardAmount(

195:        if (block.timestamp >= periodFinish[address(rewardsToken)]) {
196:            rewardRates[address(rewardsToken)] = reward.div(
197:                rewardsDuration[address(rewardsToken)]
198:            );
199:        } else {
200:            uint256 remaining = periodFinish[address(rewardsToken)].sub(
201:                block.timestamp
202:            );
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L217-L226
### BathBuddy.sol.notifyRewardAmount(): rewardsDuration\[address(rewardsToken)] should be cached in local storage
```solidity
File: /contracts/periphery/BathBuddy.sol
217:        require(
218:            rewardRates[address(rewardsToken)] <=
219:                balance.div(rewardsDuration[address(rewardsToken)]),
220:            "Provided reward too high"
221:        );

224:        periodFinish[address(rewardsToken)] = block.timestamp.add(
225:            rewardsDuration[address(rewardsToken)]
226:        );
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1028-L1067
### RubiconMarket.sol.sellAllAmount(): offers\[offerId] should be cached in local storage
**Regardless of the path followed we would benefit from caching**
```solidity
File: /contracts/RubiconMarket.sol
1028:    function sellAllAmount(

1043:            if (
1044:                mul(pay_amt, 1 ether) <
1045:                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
1046:            ) {
1047:                break; //We consider that all amount is sold
1048:            }
1049:            if (pay_amt >= offers[offerId].buy_amt) {

1051:                fill_amt = add(fill_amt, offers[offerId].pay_amt);            
1052:                pay_amt = sub(pay_amt, offers[offerId].buy_amt);                 
1053:                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); 
                
1054:            } else {
1055:                // if lower
1056:                uint256 baux = rmul(
1057:                    mul(pay_amt, 10 ** 9),
1058:                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
1059:                ) / 10 ** 9;
```

Similar instance to the one above on same file
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1069-L1112

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1124-L1146

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1157-L1179

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1208-L1212
### RubiconMarket.sol.\_find(): offers\[id] should be cached in local storage
```solidity
File: /contracts/RubiconMarket.sol
1208:    function _find(uint256 id) internal view returns (uint256) {

1211:        address buy_gem = address(offers[id].buy_gem);//@audit: 1st access
1212:        address pay_gem = address(offers[id].pay_gem);//@audit: 2nd access
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1261-L1268
### RubiconMarket.sol.\_isPricedLtOrEq(): offers\[low] and offers\[high] should be cached in local storage
```solidity
File: /contracts/RubiconMarket.sol
1261:    function _isPricedLtOrEq(

1265:        return
1266:            mul(offers[low].buy_amt, offers[high].pay_amt) >=
1267:            mul(offers[high].buy_amt, offers[low].pay_amt);
1268:    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1289-L1292
### RubiconMarket.sol.\_matcho(): offers\[best_maker_id] should be cached in local storage
```solidity
File: /contracts/RubiconMarket.sol
1289:        while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {
1290:            best_maker_id = _best[address(t_buy_gem)][address(t_pay_gem)];
1291:            m_buy_amt = offers[best_maker_id].buy_amt;
1292:            m_pay_amt = offers[best_maker_id].pay_amt;
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1368-L1369
### RubiconMarket.sol.\_sort(): offers[id] should be cached
```solidity
File: /contracts/RubiconMarket.sol
1368:        ERC20 buy_gem = offers[id].buy_gem;
1369:        ERC20 pay_gem = offers[id].pay_gem;
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1408-L1409
### RubiconMarket.sol.\_unsort(): offers[id] should be cached
```solidity
File: /contracts/RubiconMarket.sol
1408:        address buy_gem = address(offers[id].buy_gem);
1409:        address pay_gem = address(offers[id].pay_gem);
```

## Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L20-L21
```solidity
File: /contracts/BathHouseV2.sol
20:    mapping(address => address) private tokenToBathToken;
21:    mapping(address => address) private bathTokenToBuddy;
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L52-L55
```solidity
File: /contracts/periphery/BathBuddy.sol
52:    mapping(address => uint256) public periodFinish; // Token specific
53:    mapping(address => uint256) public rewardRates; // Token specific reward rates
54:    mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
55:    mapping(address => uint256) public lastUpdateTime; //Token specific
```

## Duplicated require()/revert() checks should be refactored to a modifier or function
This saves deployement gas

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L834
```solidity
File: /contracts/RubiconMarket.sol
834:        require(!locked, "Reentrancy attempt");
```
The above is repeated in the following

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L859
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L874
```solidity
File: /contracts/RubiconMarket.sol
859:        require(!locked, "Reentrancy attempt");

874:        require(!locked, "Reentrancy attempt");
```


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L938
```solidity
File: /contracts/RubiconMarket.sol
938:        require(!locked);

1034:       require(!locked);

1075:       require(!locked);
```


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L122
```solidity
File: /contracts/periphery/BathBuddy.sol
122:        require(friendshipStarted, "I have not started a bathToken friendship");

143:        require(friendshipStarted, "I have not started a bathToken friendship");
```

## Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L491-L496

```solidity
File: /contracts/RubiconMarket.sol
//@audit:  uint128 pay_amt, uint128 buy_amt
491:    function make(
492:        ERC20 pay_gem,
493:        ERC20 buy_gem,
494:        uint128 pay_amt,
495:        uint128 buy_amt
496:    ) external virtual returns (bytes32 id) {

//@audit: uint128 maxtakeAmount
564:    function take(bytes32 id, uint128 maxTakeAmount) external virtual {

//@audit: uint128 pay_amt,uint128 buy_amt
733:    function make(
734:        ERC20 pay_gem,
735:        ERC20 buy_gem,
736:        uint128 pay_amt,
737:        uint128 buy_amt
738:    ) public override returns (bytes32) {

//@audit: uint128 maxTakeAmount
752:    function take(bytes32 id, uint128 maxTakeAmount) public override {
```


## Use the cached value here instead of reading from storage again
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1438-L1451
### RubiconMarket.sol.\_hide(): uid should be used(saves 1 SLOAD)
```solidity
File: /contracts/RubiconMarket.sol
1438:    function _hide(

1441:        uint256 uid = _head; //id of an offer in unsorted offers list

1446:        if (_head == id) {//@audit: _head already cached, use the cached value(uid)
```

```diff
@@ -1443,7 +1443,7 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {

         require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list

-        if (_head == id) {
+        if (uid == id) {
             //check if offer is first offer in unsorted offers list
             _head = _near[id]; //set head to new first unsorted offer
             _near[id] = 0; //delete order from unsorted order list
```

## Splitting require() statements that use && saves gas - (saves 8 gas per &&)

Instead of using the && operator in a single require statement to check multiple conditions,using multiple require statements with 1 condition per require statement will save 8 GAS per &&
The gas difference would only be realized if the revert condition is realized(met).


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L612-L616
```solidity
File: /contracts/RubiconMarket.sol
612:        require(
613:            (msg.sender == getOwner(id)) ||
614:                isClosed() ||
615:                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
616:        );
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L721-L727
```solidity
File: /contracts/RubiconMarket.sol
721:        require(
722:            isClosed() ||
723:                msg.sender == getOwner(id) ||
724:                id == dustId ||
725:                (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
726:            "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
727:        );
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L893-L898
```solidity
File: /contracts/RubiconMarket.sol
893:        require(
894:            payAmts.length == payGems.length &&
895:                payAmts.length == buyAmts.length &&
896:                payAmts.length == buyGems.length,
897:            "Array lengths do not match"
898:        );
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L940-L944
```solidity
File: /contracts/RubiconMarket.sol
940:        require(
941:            !isActive(id) &&
942:                _rank[id].delb != 0 &&
943:                _rank[id].delb < block.number - 10
944:        );
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1412-L1415
```solidity
File: /contracts/RubiconMarket.sol
1412:        require(
1413:            _rank[id].delb == 0 && //assert id is in the sorted list
1414:                isOfferSorted(id)
1415:       );
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L591-L594
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
591:            ? require(
592:                _leverage > _wad && _leverage <= _leverageMax,
593:                "_leverageCheck{Long}: INVLAID LEVERAGE"
594:            )
595:            : require(
596:                _leverage >= _wad && _leverage <= _leverageMax,
597:                "_leverageCheck{Short}: INVLAID LEVERAGE"
598:            );
```

## A modifier used only once and not being inherited should be inlined to save gas

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L643-L657
```solidity
File: /contracts/RubiconMarket.sol
643:    modifier note() {
644:        bytes32 foo;
645:        bytes32 bar;
646:        uint256 wad;

648:        assembly {
649:            foo := calldataload(4)
650:            bar := calldataload(36)
651:            wad := callvalue()
652:        }

654:        emit LogNote(msg.sig, msg.sender, foo, bar, wad, msg.data);
        
656:        _;
657:    }
```

The above modifer is only used once on [Line 958](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L958)

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L26-L29
```solidity
File: /contracts/BathHouseV2.sol
26:    modifier onlyAdmin() {
27:        require(msg.sender == admin, "onlyAdmin: !admin");
28:        _;
29:    }
```
The above modifier is only used on [Line 66](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L66)

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L104-L107
```solidity
File: /contracts/periphery/BathBuddy.sol
104:    modifier onlyBathHouse() {
105:        require(msg.sender == bathHouse, "You are not my beloved bath house!");
106:        _;
107:    }
```
The above modifier is only used on [Line 177](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L177)


## Functions guaranteed to revert when called by normal users can be marked `payable`

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.The extra opcodes avoided costs an average of about **21 gas per call** to the function, in addition to the extra deployment cost

**Total Instances: 9**
**Gas saved: `9 * 21 = 118`**


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L93-L98

```solidity
File: /contracts/utilities/poolsUtility/Position.sol
93:    function buyAllAmountWithLeverage(
94:        address quote,
95:        address asset,
96:        uint256 quotePayAmount,
97:        uint256 leverage
98:    ) external onlyOwner {

107:    function sellAllAmountWithLeverage(
108:        address asset,
109:        address quote,
110:        uint256 assetPayAmount,
111:        uint256 leverage
112:    ) external onlyOwner {

210:    function closePosition(uint256 posId) external onlyOwner {

226:    function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242:    function withdraw(address token, uint256 amount) external onlyOwner {
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L168-L178
```solidity
File: /contracts/periphery/BathBuddy.sol
168:    function getReward(
169:        IERC20 rewardsToken,
170:        address holderRecipient
171:    )
172:        external
173:        override
174:        nonReentrant
175:        whenNotPaused
176:        updateReward(holderRecipient, address(rewardsToken))
177:        onlyBathHouse
178:    {

191:    function notifyRewardAmount(
192:        uint256 reward,
193:        IERC20 rewardsToken
194:    ) external onlyOwner updateReward(address(0), address(rewardsToken)) {

232:    function setRewardsDuration(
233:        uint256 _rewardsDuration,
234:        address token
235:    ) external onlyOwner {
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L60-L66
```solidity
File: /contracts/BathHouseV2.sol
60:    function createBathToken(
61:        address underlying,
62:        InterestRateModel interestRateModel,
63:        uint256 initialExchangeRateMantissa,
64:        address implementation,
65:        bytes memory becomeImplementationData
66:    ) external onlyAdmin {
```

## Setting the constructor to payable

#### Description:

You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of `if(msg.value != 0) revert();`  and saves 13 gas on deployment with no security risks.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L54
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
54:    constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30
```solidity
File: /contracts/V2Migrator.sol
30:    constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
```

see [Openzeppelin contract creation](https://blog.openzeppelin.com/deconstructing-a-solidity-contract-part-ii-creation-vs-runtime-6b9d60ecb44c/)

## Cheaper to read a constant directly rather than cache it(Gas saved: 5)
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L586-L599
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
586:    function _leverageCheck(uint256 _leverage, bool _long) internal pure {
587:        uint256 _wad = WAD;
588:        uint256 _leverageMax = WAD.mul(3);

590:        _long // long can't be with 1x leverage
591:            ? require(
592:                _leverage > _wad && _leverage <= _leverageMax,
593:                "_leverageCheck{Long}: INVLAID LEVERAGE"
594:            )
595:            : require(
596:                _leverage >= _wad && _leverage <= _leverageMax,
597:                "_leverageCheck{Short}: INVLAID LEVERAGE"
598:            );
599:    }
```

```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..1b1b689 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -584,16 +584,15 @@ contract Position is Ownable, DSMath {

     /// @notice check if specified leverage fits into the current leverage boundaries
     function _leverageCheck(uint256 _leverage, bool _long) internal pure {
-        uint256 _wad = WAD;
         uint256 _leverageMax = WAD.mul(3);

         _long // long can't be with 1x leverage
             ? require(
-                _leverage > _wad && _leverage <= _leverageMax,
+                _leverage > WAD && _leverage <= _leverageMax,
                 "_leverageCheck{Long}: INVLAID LEVERAGE"
             )
             : require(
-                _leverage >= _wad && _leverage <= _leverageMax,
+                _leverage >= WAD && _leverage <= _leverageMax,
                 "_leverageCheck{Short}: INVLAID LEVERAGE"
             );
     }
```


## Do not calculate constants variables

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L232
```solidity
File: /contracts/RubiconMarket.sol
232:    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..3d873af 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -229,7 +229,7 @@ contract SimpleMarket is EventfulMarket, DSMath {
     /// @dev This parameter provides the address to which fees are sent
     address internal feeTo;

-    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
+    bytes32 internal constant MAKER_FEE_SLOT = 0x55a583bf144a5516c8de6ffe85b1859d93234d5e765ed3e895106330766044b8;

```


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74-L75
```solidity
File: /contracts/RubiconMarket.sol
74:    uint256 constant WAD = 10 ** 18;
75:    uint256 constant RAY = 10 ** 27;
```

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..176b4ca 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -71,8 +71,8 @@ contract DSMath {
         return x >= y ? x : y;
     }

-    uint256 constant WAD = 10 ** 18;
-    uint256 constant RAY = 10 ** 27;
+    uint256 constant WAD = 1e18;
+    uint256 constant RAY = 1e27;

```

## Cast only once instead of repeatedly casting values to same type

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L125-L160
### Position.sol.openPosition(): IERC20(asset) should be cast only once and result cached
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
    function openPosition(

        vars.initAssetBalance = IERC20(asset).balanceOf(address(this));

        IERC20(asset).safeTransferFrom(msg.sender, address(this), initMargin);

        for (uint256 i = 0; i < vars.limit; ++i) {
            /// @dev save borrowed balance of an asset
            uint256 assetBalance = IERC20(asset).balanceOf(address(this));
```

```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..27aff29 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -137,10 +137,11 @@ contract Position is Ownable, DSMath {
         /// @dev save initial borrow balance before borrowing more
         vars.borrowedAmount = borrowBalance(bathTokenQuote);
         /// @dev save initial balance of asset to calculate then amount to borrow
-        vars.initAssetBalance = IERC20(asset).balanceOf(address(this));
+        IERC20 asset_ = IERC20(asset);
+        vars.initAssetBalance = asset_.balanceOf(address(this));

         // transfer initial margin amount
-        IERC20(asset).safeTransferFrom(msg.sender, address(this), initMargin);
+        asset_.safeTransferFrom(msg.sender, address(this), initMargin);

         // enter bathTokenAsset market in order to
         // supply collateral and borrow quote
@@ -157,7 +158,7 @@ contract Position is Ownable, DSMath {
         // supply/borrow/swap => until the iterations limit is reached
         for (uint256 i = 0; i < vars.limit; ++i) {
             /// @dev save borrowed balance of an asset
-            uint256 assetBalance = IERC20(asset).balanceOf(address(this));
+            uint256 assetBalance = asset_.balanceOf(address(this));
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L280-L302
### Position.sol.\_repay(): IERC20(\_quote) should be cast only once
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
280:   function _repay(address _asset, address _quote, uint256 _posId) internal {

286:        uint256 _quoteBalance = IERC20(_quote).balanceOf(address(this));

297:        IERC20(_quote).approve(_bathTokenQuote, _amountToRepay);
```

```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..027d79a 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -283,7 +283,8 @@ contract Position is Ownable, DSMath {

         // sell asset for quote
         _rubiconSwap(_asset, _quote, _amountToRepay, false);
-        uint256 _quoteBalance = IERC20(_quote).balanceOf(address(this));
+        IERC20 quote_ = IERC20(_quote);
+        uint256 _quoteBalance = quote_.balanceOf(address(this));

         require(
             _amountToRepay <= _quoteBalance,
@@ -294,7 +295,7 @@ contract Position is Ownable, DSMath {
             _amountToRepay = _borrowBalance;
         }

-        IERC20(_quote).approve(_bathTokenQuote, _amountToRepay);
+        quote_.approve(_bathTokenQuote, _amountToRepay);
         require(
             CErc20Interface(_bathTokenQuote).repayBorrow(_amountToRepay) == 0,
             "_repay: ERROR"
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L454-L473
### Position.sol.\_marketBuy(): ERC20(\_asset) and ERC20(\_quote) should be cast once and cached
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
454:    function _marketBuy(
455:        address _asset,
456:        address _quote,
457:        uint256 _maxFill
458:    ) internal {
459:        uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
460:        uint256 _buyAmount = rubiconMarket.getBuyAmount(
461:            ERC20(_asset),
462:            ERC20(_quote),
463:            _maxFill.sub(_fee)
464:        );
465:        IERC20(_quote).approve(address(rubiconMarket), _maxFill);

467:        rubiconMarket.buyAllAmount(
468:            ERC20(_asset),
469:            _buyAmount,
470:            ERC20(_quote),
471:            _maxFill
472:        );
473:    }
```


```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..5f962c9 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -457,17 +457,19 @@ contract Position is Ownable, DSMath {
         uint256 _maxFill
     ) internal {
         uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
+        ERC20 asset_ = ERC20(_asset);
+        ERC20 quote_ = ERC20(_quote);
         uint256 _buyAmount = rubiconMarket.getBuyAmount(
-            ERC20(_asset),
-            ERC20(_quote),
+            asset_,
+            quote_,
             _maxFill.sub(_fee)
         );
-        IERC20(_quote).approve(address(rubiconMarket), _maxFill);
+        quote_.approve(address(rubiconMarket), _maxFill);

         rubiconMarket.buyAllAmount(
-            ERC20(_asset),
+            asset_,
             _buyAmount,
-            ERC20(_quote),
+            quote_,
             _maxFill
         );
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L475-L511
### Position.sol.\_marketSell(): ERC20(\_asset) and ERC20(\_quote) should be cast only once and cached
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
475:    function _marketSell(

482:        uint256 _payAmount = rubiconMarket.getPayAmount(
483:            ERC20(_asset),
484:            ERC20(_quote),
485:            _minFill.add(_fee)
486:        );
487:        uint256 _assetBalance = IERC20(_asset).balanceOf(address(this));

492:        if (_assetBalance < _payAmount) {
493:            IERC20(_asset).transferFrom(

500:        IERC20(_asset).approve(
501:            address(rubiconMarket),
502:            IERC20(_asset).balanceOf(address(this))
503:        );

505:        rubiconMarket.sellAllAmount(
506:            ERC20(_asset),
507:            _payAmount,
508:            ERC20(_quote),
509:            _minFill
510:        );
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L93-L106
### FeeWrapper.sol.\_chargeFee(): IERC20(\_feeToken) should be cast only once and use the cached value
```solidity
File: /contracts/utilities/FeeWrapper.sol
93:  function _chargeFee(FeeParams memory _feeParams, address _target) internal {

100:        IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);
101:        // transfer fee to the 3rd party protocol
102:        IERC20(_feeToken).transfer(_feeTo, _feeAmount);

105:        IERC20(_feeToken).approve(_target, (_totalAmount - _feeAmount));
106:    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L59-L66
### V2Migrator.sol.migrate(): bathTokenV2 should be cast to IERC20 once 
```solidity
File: /contracts/V2Migrator.sol
59:        IERC20(bathTokenV2).transfer(
60:            msg.sender,
61:            IERC20(bathTokenV2).balanceOf(address(this))
62:        );
63:        require(
64:            IERC20(bathTokenV2).balanceOf(address(this)) == 0,
65:            "migrate: BATH TOKENS V2 STUCK IN THE CONTRACT"
66:        );
```

## SafeMath no longer required

Since Solidity 0.8, the overflow/underflow check is implemented on the language level - it adds the validation to the bytecode during compilation.

You don't need the SafeMath library for Solidity 0.8+. Using it  in this version will just perform the same validation twice (one on the language level and one in the library) which simply adds overhead.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L6
```solidity
File: /contracts/periphery/BathBuddy.sol
6:import "@openzeppelin/contracts/utils/math/SafeMath.sol";
```

```solidity
39:    using SafeMath for uint256;
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L6
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
6:import "@openzeppelin/contracts/utils/math/SafeMath.sol";
```

The safeMath pattern has been used extensively throughout the inscope contracts. Using the above as an example