# Summary
A majority of the optimizations were benchmarked via the protocol's tests, i.e. using the following config: `solc version 0.8.17`, `optimizer on`, and `5 runs`. Optimizations that were not benchmarked are explained via EVM gas costs and opcodes.

Below are the overall average gas savings for the following tested functions, with all the optimizations applied (not including [Issue 1](#do-not-use-safemath) and [Issue 2](#use-a-more-gas-efficient-synchronized-modifier)):
| Function |    Before   |    After   | Avg Gas Savings |
| ------ | -------- | -------- | ------- |
| BathBuddy.notifyRewardAmount |  113455  |  68064  |  45391 | 
| BathBuddy.setRewardsDuration |  49947  |  47792  |  2155 |  
| BathHouseV2.claimRewards |  265601  |  240252  | 25349 | 
| ExpiringMarket.cancel |  118396  |  86489 | 31907 | 
| Position.buyAllAmountWithLeverage |  694718  |  638441  | 56277 | 
| Position.closePosition |  500436  |  490680  | 9756 | 
| Position.increaseMargin |  201193  |  195290  | 5903 | 
| RubiconMarket.buy |  142006  |  139691  | 2315 | 
| RubiconMarket.offer |  259582  |  251681  | 7901 | 
| RubiconMarket.sellAllAmount |  145248  |  142506  | 2742 | 
| RubiconRouter.buyAllAmountForETH |  217184  |  201348  | 15836 | 
| RubiconRouter.buyAllAmountWithETH |  225975  |  220047  | 5928 | 
| RubiconRouter.cancelForETH |  169250  |  152143  | 17107 |
| RubiconRouter.offerForETH |  332029  |  331577  | 452 |
| RubiconRouter.offerWithETH |  350416  |  350246  | 170 |

**Total gas saved across all listed functions: 229189**

*Notes*: 

- The `Avg` gas changes unpredictably for `buyAllAmountWithLeverage` and therefore the `Min` gas for this function is used for benchmarking.
- The [Gas report](#gasreport-output-with-all-optimizations-applied) output, after all optimizations have been applied, can be found at the end of the report.
- The final diffs for each contract, with all the optimizations applied, can be found [here](https://gist.github.com/0xJCN/69437a989c5a2a2ef4ae45a7ff13cac7).
- Some code snippets may be truncated to save space. Code snippets may also be accompanied by `@audit` tags in comments to aid in explaining the issue.

## Gas Optimizations
| Number |Issue|Instances|
|-|:-|:-:|
| [G-01](#do-not-use-safemath) | Do not use SafeMath | - | 
| [G-02](#use-a-more-gas-efficient-synchronized-modifier) | Use a more gas efficient `synchronized` modifier | - | 
| [G-03](#multiple-address-mappings-can-be-combined-into-a-single-mapping-of-an-address-to-a-struct-where-appropriate) | Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate | 2 | 
| [G-04](#state-variables-only-set-in-the-constructor-should-be-declared-immutable) | State variables only set in the constructor should be declared immutable | 4 | 
| [G-05](#state-variables-can-be-cached-instead-of-re-reading-them-from-storage) | State variables can be cached instead of re-reading them from storage | 32 |
| [G-06](#avoid-emitting-storage-values) | Avoid emitting storage values | 1 |
| [G-07](#structs-can-be-packed-to-use-fewer-storage-slots) | Structs can be packed to use fewer storage slots | 2 |
| [G-08](#state-variables-can-be-packed-to-use-fewer-storage-slots) | State variables can be packed to use fewer storage slots | 1 |
| [G-09](#rearrange-storage-variables-to-pack-values-more-efficiently) | Rearrange storage variables to pack values more efficiently | 1 |
| [G-10](#return-values-from-external-calls-can-be-cached-to-avoid-unecessary-call) | Return values from external calls can be cached to avoid unecessary call | 1 |
| [G-11](#multiple-accesses-of-a-mappingarray-should-use-a-storage-pointer) | Multiple accesses of a mapping/array should use a storage pointer | 43 | 
| [G-12](#use-assembly-for-value-transfer-and-success-check) | Use assembly for value transfer and success check | 1 | 

## Do not use SafeMath
Since version 0.8.0, the compiler will throw an error for under/overflows. It does so by including extra opcodes to perform the necessary checks. Therefore, using `SafeMath` and the in-house `DSMath` contract is unecessary and is wasting gas by doing extra computation (under/overflow checks). Save gas by not using `SafeMath` and the `DSMath` contract.

The following files have this issue: RubiconMarket.sol, which uses the [DSMath](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L44) contract, [BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol), and [Position.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol). 

## Use a more gas efficient `synchronized` modifier
Each time a function that uses the `synchronized` modifier is called, said function will undergo a gas cost of `22.9k` for setting the `locked` slot to 1 from 0 (20k gas) and then back to 1 from 0 (2.9k gas). We are able to use a `1` and `2` instead of `false` and `true` to make a more efficient modifier. Using `1` and `2`, the functions will undergo a gas cost of `5.8k`: `1` -> `2` (2.9k gas) & `2` -> `1` (2.9k gas). See [this](https://hackmd.io/@fvictorio/gas-costs-after-berlin) article for more information.

Functions which have the `synchronized` modifier: [SimpleMarket.buy](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L314), [SimpleMarket.cancel](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L452), and [SimpleMarket.offer](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511).

Estimated Gas Saved: `17100`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269

```solidity
264:    modifier synchronized() {
265:        require(!locked);
266:        locked = true;
267:        _;
268:        locked = false;
269:    }
```

## Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate
We can combine multiple mappings below into structs. We can then pack the structs by modifying the `uint` type for the values. This will result in cheaper storage reads since multiple mappings are accessed in functions and those values are now occupying the same storage slot, meaning the slot will become warm after the first SLOAD. In addition, when setting the struct values we will avoid incurring multiple `Gsset (20000 gas)` since multiple struct values are now occupying the same slot.

*The first 5 mappings relate to token information and can therefore be combined into a struct. The last two mappings contain token information that is specific to a user, therefore those mappings can be combined into another struct.*

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L52-L61

*Gas Savings for `notifyRewardAmount`, obtained via protocol's tests: Avg 43096 gas* 
|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  113455 |    1    |
| After  |  -  |  -  |  70359  |    1    |

*Gas Savings for `setRewardsDuration`, obtained via protocol's tests: Avg 2120 gas*
|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  49947 |    1    |
| After  |  -  |  -  |  47827 |    1    |

*Gas Savings for `claimRewards`, obtained via protocol's tests: Avg 23219 gas*
|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  265601 |    1    |
| After  |  -  |  -  |  242382 |    1    |

```solidity
File: contracts/periphery/BathBuddy.sol
52:    mapping(address => uint256) public periodFinish; // Token specific
53:    mapping(address => uint256) public rewardRates; // Token specific reward rates
54:    mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
55:    mapping(address => uint256) public lastUpdateTime; //Token specific
56:    mapping(address => uint256) public rewardsPerTokensStored; // Token specific
57:
58:    // Token then user always
59:    mapping(address => mapping(address => uint256))
60:        public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
61:    mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..6d49bde 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -48,17 +48,24 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
     address public bathHouse;
     bool public friendshipStarted;

+    struct TokenInfo {
+        uint80 periodFinish;
+        uint80 lastUpdateTime;
+        uint80 rewardsDuration;
+        uint128 rewardRates;
+        uint128 rewardsPerTokensStored;
+    }
+
+    struct UserInfo {
+        uint128 userRewardsPerTokenPaid;
+        uint128 tokenRewards;
+    }
+
     /// @dev set to block.timestamp + rewards duration to track an active rewards period after notifyRewardAmount()
-    mapping(address => uint256) public periodFinish; // Token specific
-    mapping(address => uint256) public rewardRates; // Token specific reward rates
-    mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
-    mapping(address => uint256) public lastUpdateTime; //Token specific
-    mapping(address => uint256) public rewardsPerTokensStored; // Token specific
+    mapping(address => TokenInfo) public tokenInfo;

     // Token then user always
-    mapping(address => mapping(address => uint256))
-        public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
-    mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
+    mapping(address => mapping(address => UserInfo)) public userInfo;

     /* ========== CONSTRUCTOR ========== */

@@ -112,23 +119,25 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
     function lastTimeRewardApplicable(
         address token
     ) public view returns (uint256) {
+        TokenInfo storage _tokenInfo = tokenInfo[token];
         return
-            block.timestamp < periodFinish[token]
+            block.timestamp < _tokenInfo.periodFinish
                 ? block.timestamp
-                : periodFinish[token];
+                : _tokenInfo.periodFinish;
     }

     function rewardPerToken(address token) public view returns (uint256) {
         require(friendshipStarted, "I have not started a bathToken friendship");
-
+
+        TokenInfo storage _tokenInfo = tokenInfo[token];
         if (IERC20(myBathTokenBuddy).totalSupply() == 0) {
-            return rewardsPerTokensStored[token];
+            return _tokenInfo.rewardsPerTokensStored;
         }
         return
-            rewardsPerTokensStored[token].add(
+            uint256(_tokenInfo.rewardsPerTokensStored).add(
                 lastTimeRewardApplicable(token)
-                    .sub(lastUpdateTime[token])
-                    .mul(rewardRates[token])
+                    .sub(_tokenInfo.lastUpdateTime)
+                    .mul(_tokenInfo.rewardRates)
                     .mul(1e18)
                     .div(IERC20(myBathTokenBuddy).totalSupply())
             );
@@ -141,23 +150,25 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         address token
     ) public view override returns (uint256) {
         require(friendshipStarted, "I have not started a bathToken friendship");
-
+
+        UserInfo storage _userInfo = userInfo[token][account];
         return
             IERC20(myBathTokenBuddy) // Care with this?
                 .balanceOf(account)
                 .mul(
                     rewardPerToken(token).sub(
-                        userRewardsPerTokenPaid[token][account]
+                        _userInfo.userRewardsPerTokenPaid
                     )
                 )
                 .div(1e18)
-                .add(tokenRewards[token][account]);
+                .add(_userInfo.tokenRewards);
     }

     function getRewardForDuration(
         address token
     ) external view returns (uint256) {
-        return rewardRates[token].mul(rewardsDuration[token]);
+        TokenInfo storage _tokenInfo = tokenInfo[token];
+        return uint256(_tokenInfo.rewardRates).mul(_tokenInfo.rewardsDuration);
     }

     /* ========== MUTATIVE FUNCTIONS ========== */
@@ -176,9 +187,10 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         updateReward(holderRecipient, address(rewardsToken))
         onlyBathHouse
     {
-        uint256 reward = tokenRewards[address(rewardsToken)][holderRecipient];
+        UserInfo storage _userInfo = userInfo[address(rewardsToken)][holderRecipient];
+        uint256 reward = _userInfo.tokenRewards;
         if (reward > 0) {
-            tokenRewards[address(rewardsToken)][holderRecipient] = 0;
+            _userInfo.tokenRewards = 0;
             rewardsToken.safeTransfer(holderRecipient, reward);
             emit RewardPaid(holderRecipient, reward);
         }
@@ -192,20 +204,21 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         uint256 reward,
         IERC20 rewardsToken
     ) external onlyOwner updateReward(address(0), address(rewardsToken)) {
-        if (block.timestamp >= periodFinish[address(rewardsToken)]) {
-            rewardRates[address(rewardsToken)] = reward.div(
-                rewardsDuration[address(rewardsToken)]
-            );
+        TokenInfo storage _tokenInfo = tokenInfo[address(rewardsToken)];
+        if (block.timestamp >= _tokenInfo.periodFinish) {
+            _tokenInfo.rewardRates = uint128(reward.div(
+                _tokenInfo.rewardsDuration
+            ));
         } else {
-            uint256 remaining = periodFinish[address(rewardsToken)].sub(
+            uint256 remaining = uint256(_tokenInfo.periodFinish).sub(
                 block.timestamp
             );
             uint256 leftover = remaining.mul(
-                rewardRates[address(rewardsToken)]
-            );
-            rewardRates[address(rewardsToken)] = reward.add(leftover).div(
-                rewardsDuration[address(rewardsToken)]
+                _tokenInfo.rewardRates
             );
+            _tokenInfo.rewardRates = uint128(reward.add(leftover).div(
+                _tokenInfo.rewardsDuration
+            ));
         }

         // Ensure the provided reward amount is not more than the balance in the contract.
@@ -215,15 +228,15 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         // Note********** ERC20s must be here*************
         uint256 balance = rewardsToken.balanceOf(address(this));
         require(
-            rewardRates[address(rewardsToken)] <=
-                balance.div(rewardsDuration[address(rewardsToken)]),
+            _tokenInfo.rewardRates <=
+                balance.div(_tokenInfo.rewardsDuration),
             "Provided reward too high"
         );

-        lastUpdateTime[address(rewardsToken)] = block.timestamp;
-        periodFinish[address(rewardsToken)] = block.timestamp.add(
-            rewardsDuration[address(rewardsToken)]
-        );
+        _tokenInfo.lastUpdateTime = uint80(block.timestamp);
+        _tokenInfo.periodFinish = uint80(block.timestamp.add(
+            _tokenInfo.rewardsDuration
+        ));
         emit RewardAdded(reward);
     }

@@ -233,25 +246,26 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         uint256 _rewardsDuration,
         address token
     ) external onlyOwner {
+        TokenInfo storage _tokenInfo = tokenInfo[token];
         require(
-            block.timestamp > periodFinish[token],
+            block.timestamp > _tokenInfo.periodFinish,
             "Previous rewards period must be complete before changing the duration for the new period"
         );
-        rewardsDuration[token] = _rewardsDuration;
-        emit RewardsDurationUpdated(rewardsDuration[token]);
+        _tokenInfo.rewardsDuration = uint80(_rewardsDuration);
+        emit RewardsDurationUpdated(_tokenInfo.rewardsDuration);
     }

     /* ========== MODIFIERS ========== */

     // Rewards set here
     modifier updateReward(address account, address token) {
-        rewardsPerTokensStored[token] = rewardPerToken(token);
-        lastUpdateTime[token] = lastTimeRewardApplicable(token);
+        TokenInfo storage _tokenInfo = tokenInfo[token];
+        _tokenInfo.rewardsPerTokensStored = uint128(rewardPerToken(token));
+        _tokenInfo.lastUpdateTime = uint80(lastTimeRewardApplicable(token));
         if (account != address(0)) {
-            tokenRewards[token][account] = earned(account, token);
-            userRewardsPerTokenPaid[token][account] = rewardsPerTokensStored[
-                token
-            ];
+            UserInfo storage _userInfo = userInfo[token][account];
+            _userInfo.tokenRewards = uint128(earned(account, token));
+            _userInfo.userRewardsPerTokenPaid = _tokenInfo.rewardsPerTokensStored;
         }
         _;
     }
```

## State variables only set in the constructor should be declared immutable
The solidity compiler will directly embed the values of immutable variables into your contract bytecode and therefore will save you from incurring a `Gsset (20000 gas)` when you set storage variables in the constructor, a `Gcoldsload (2100 gas)` when you access storage variables for the first time in a transaction, and a `Gwarmaccess (100 gas)` for each subsequent access to that storage slot.

Total Instances: `4`

Estimated Gas Saved: `4 * 2100 = 8400`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L44-L47
```solidity
File: contracts/utilities/poolsUtility/Position.sol
44:    Comptroller public comptroller;
45:    PriceOracle public oracle;
46:    RubiconMarket public rubiconMarket;
47:    BathHouseV2 public bathHouseV2;
```
```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..a79c2fe 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -41,10 +41,10 @@ contract Position is Ownable, DSMath {
     // position id => Position struct
     mapping(uint256 => Position) public positions;

-    Comptroller public comptroller;
-    PriceOracle public oracle;
-    RubiconMarket public rubiconMarket;
-    BathHouseV2 public bathHouseV2;
+    Comptroller immutable public comptroller;
+    PriceOracle immutable public oracle;
+    RubiconMarket immutable public rubiconMarket;
+    BathHouseV2 immutable public bathHouseV2;

     // events
     event PositionOpened(uint256 positionId, Position position);
```

## State variables can be cached instead of re-reading them from storage
Caching of a state variable replaces each `Gwarmaccess (100 gas)` with a much cheaper stack read.

Total Instances: `32`

Estimated Gas Saved: `32 * 100 = 3200`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568-L571

### Cache `last_offer_id++` to save 1 SLOAD
```solidity
File: contracts/RubiconMarket.sol
568:    function _next_id() internal returns (uint256) {
569:        last_offer_id++; // @audit: 1st sload
570:        return last_offer_id; // @audit: 2nd sload
571:    }
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..465614e 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -566,8 +566,9 @@ contract SimpleMarket is EventfulMarket, DSMath {
     }

     function _next_id() internal returns (uint256) {
-        last_offer_id++;
-        return last_offer_id;
+        uint256 _last_offer_id = last_offer_id + 1;
+        last_offer_id = _last_offer_id;
+        return _last_offer_id;
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L334-L436

### Cache `offers[id].pay_amt ` to save 1 SLOAD
```solidity
File: contracts/RubiconMarket.sol
334:        offers[id].pay_amt = sub(_offer.pay_amt, quantity); // @audit: 1st sload
...
436:        if (offers[id].pay_amt == 0) { // @audit: 2nd sload
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..018dd66 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -331,7 +331,8 @@ contract SimpleMarket is EventfulMarket, DSMath {
             return false;
         }

-        offers[id].pay_amt = sub(_offer.pay_amt, quantity);
+        uint256 _pay_amt = sub(_offer.pay_amt, quantity);
+        offers[id].pay_amt = _pay_amt;
         offers[id].buy_amt = sub(_offer.buy_amt, spend);

         /// @dev Fee logic added on taker trades
@@ -433,7 +434,7 @@ contract SimpleMarket is EventfulMarket, DSMath {
         ///     address(_offer.buy_gem)
         /// );

-        if (offers[id].pay_amt == 0) {
+        if (_pay_amt == 0) {
             delete offers[id];
             /// emit OfferDeleted(bytes32(id));
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1043-L1059

### Cache `offers[offerId].buy_amt` and `offers[offerId].pay_amt` to save 4 SLOADs
```solidity
File: contracts/RubiconMarket.sol
1043:            if (
1044:                mul(pay_amt, 1 ether) <
1045:                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt) // @audit: 1st sloads
1046:            ) {
1047:                break; //We consider that all amount is sold
1048:            }
1049:            if (pay_amt >= offers[offerId].buy_amt) { // @audit: 2nd sload
1050:                //If amount to sell is higher or equal than current offer amount to buy
1051:                fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator // @audit: 2nd sload
1052:                pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell // @audit: 3rd sload
1053:                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer // @audit: 3rd sload
1054:            } else {
1055:                // if lower
1056:                uint256 baux = rmul(
1057:                    mul(pay_amt, 10 ** 9),
1058:                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt) // 2nd sload for `offers[offerId].pay_amt` & 3rd sload for `offers[offerId].buy_amt`
1059:                ) / 10 ** 9;
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..913e9e3 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1040,22 +1040,24 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             require(offerId != 0, "0 offerId"); //Fails if there are not more offers

             // There is a chance that pay_amt is smaller than 1 wei of the other token
+            uint256 _buy_amt = offers[offerId].buy_amt;
+            uint256 _pay_amt = offers[offerId].pay_amt;
             if (
                 mul(pay_amt, 1 ether) <
-                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
+                wdiv(_buy_amt, _pay_amt)
             ) {
                 break; //We consider that all amount is sold
             }
-            if (pay_amt >= offers[offerId].buy_amt) {
+            if (pay_amt >= _buy_amt) {
                 //If amount to sell is higher or equal than current offer amount to buy
-                fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator
-                pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell
-                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
+                fill_amt = add(fill_amt, _pay_amt); //Add amount bought to acumulator
+                pay_amt = sub(pay_amt, _buy_amt); //Decrease amount to sell
+                take(bytes32(offerId), uint128(_pay_amt)); //We take the whole offer
             } else {
                 // if lower
                 uint256 baux = rmul(
                     mul(pay_amt, 10 ** 9),
-                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
+                    rdiv(_pay_amt, _buy_amt)
                 ) / 10 ** 9;
                 fill_amt = add(fill_amt, baux); //Add amount bought to acumulator
                 take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1083-L1102

### Cache `offers[offerId].pay_amt` and `offers[offerId].buy_amt)` to save 4 SLOADs
```solidity
File: contracts/RubiconMarket.sol
1083:            if (
1084:                mul(buy_amt, 1 ether) <
1085:                wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt) // @audit: 1st sloads
1086:            ) {
1087:                break; //We consider that all amount is sold
1088:            }
1089:            if (buy_amt >= offers[offerId].pay_amt) { // @audit: 2nd sload
1090:                //If amount to buy is higher or equal than current offer amount to sell
1091:                fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator // @audit: 2nd sload
1092:                buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy // @audit: 3rd sload
1093:                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer // @audit: 3rd sload
1094:            } else {
1095:                //if lower
1096:                fill_amt = add(
1097:                    fill_amt,
1098:                    rmul(
1099:                        mul(buy_amt, 10 ** 9),
1100:                        rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt) // @audit: 2nd sload for `offers[offerId].buy_amt` & 3rd sload for `offers[offerId].pay_amt`
1101:                    ) / 10 ** 9
1102:                ); //Add amount sold to acumulator
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..86cc980 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1080,24 +1080,26 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             require(offerId != 0, "offerId == 0");

             // There is a chance that buy_amt is smaller than 1 wei of the other token
+            uint256 _pay_amt = offers[offerId].pay_amt;
+            uint256 _buy_amt = offers[offerId].buy_amt;
             if (
                 mul(buy_amt, 1 ether) <
-                wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
+                wdiv(_pay_amt, _buy_amt)
             ) {
                 break; //We consider that all amount is sold
             }
-            if (buy_amt >= offers[offerId].pay_amt) {
+            if (buy_amt >= _pay_amt) {
                 //If amount to buy is higher or equal than current offer amount to sell
-                fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator
-                buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy
-                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
+                fill_amt = add(fill_amt, _buy_amt); //Add amount sold to acumulator
+                buy_amt = sub(buy_amt, _pay_amt); //Decrease amount to buy
+                take(bytes32(offerId), uint128(_pay_amt)); //We take the whole offer
             } else {
                 //if lower
                 fill_amt = add(
                     fill_amt,
                     rmul(
                         mul(buy_amt, 10 ** 9),
-                        rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
+                        rdiv(_buy_amt, _pay_amt)
                     ) / 10 ** 9
                 ); //Add amount sold to acumulator
                 take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1183-L1205

### Cache `offers[id].pay_amt` to save 1 SLOAD
```solidity
File: contracts/RubiconMarket.sol
1183:    function _buys(uint256 id, uint256 amount) internal returns (bool) {
1184:        require(buyEnabled);
1185:        if (amount == offers[id].pay_amt) { // @audit: 1st sload
1186:            if (isOfferSorted(id)) {
1187:                //offers[id] must be removed from sorted list because all of it is bought
1188:                _unsort(id);
1189:            } else {
1190:                _hide(id);
1191:            }
1192:        }
1193:
1194:        require(super.buy(id, amount));
1195:
1196:        // If offer has become dust during buy, we cancel it
1197:        if (
1198:            isActive(id) &&
1199:            offers[id].pay_amt < _dust[address(offers[id].pay_gem)] // @audit: 2nd sload
1200:        ) {
1201:            dustId = id; //enable current msg.sender to call cancel(id)
1202:            cancel(id);
1203:        }
1204:        return true;
1205:    }
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..49e87eb 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1182,7 +1182,8 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {

     function _buys(uint256 id, uint256 amount) internal returns (bool) {
         require(buyEnabled);
-        if (amount == offers[id].pay_amt) {
+        uint256 _pay_amt = offers[id].pay_amt;
+        if (amount == _pay_amt) {
             if (isOfferSorted(id)) {
                 //offers[id] must be removed from sorted list because all of it is bought
                 _unsort(id);
@@ -1196,7 +1197,7 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
         // If offer has become dust during buy, we cancel it
         if (
             isActive(id) &&
-            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]
+            _pay_amt < _dust[address(offers[id].pay_gem)]
         ) {
             dustId = id; //enable current msg.sender to call cancel(id)
             cancel(id);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1417-L1430

### Cache `_rank[id].next` and `_rank[id].next` to save 4 SLOADs
```solidity
File: contracts/RubiconMarket.sol
1417:        if (id != _best[pay_gem][buy_gem]) {
1418:            // offers[id] is not the highest offer
1419:            require(_rank[_rank[id].next].prev == id); // @audit: 1st sload
1420:            _rank[_rank[id].next].prev = _rank[id].prev; // @audit: 2nd sload & 1st sload
1421:        } else {
1422:            //offers[id] is the highest offer
1423:            _best[pay_gem][buy_gem] = _rank[id].prev; // @audit: 1st sload
1424:        }
1425:
1426:        if (_rank[id].prev != 0) {
1427:            //offers[id] is not the lowest offer
1428:            require(_rank[_rank[id].prev].next == id); // @audit: 2nd sload
1429:            _rank[_rank[id].prev].next = _rank[id].next; // @audit: 3rd sloads
1430:        }
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..2d43bea 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1413,20 +1414,22 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             _rank[id].delb == 0 && //assert id is in the sorted list
                 isOfferSorted(id)
         );
-
+
+        uint256 _next = _rank[id].next;
+        uint256 _prev = _rank[id].prev;
         if (id != _best[pay_gem][buy_gem]) {
             // offers[id] is not the highest offer
-            require(_rank[_rank[id].next].prev == id);
-            _rank[_rank[id].next].prev = _rank[id].prev;
+            require(_rank[_next].prev == id);
+            _rank[_next].prev = _prev;
         } else {
             //offers[id] is the highest offer
-            _best[pay_gem][buy_gem] = _rank[id].prev;
+            _best[pay_gem][buy_gem] = _prev;
         }

         if (_rank[id].prev != 0) {
             //offers[id] is not the lowest offer
-            require(_rank[_rank[id].prev].next == id);
-            _rank[_rank[id].prev].next = _rank[id].next;
+            require(_rank[_prev].next == id);
+            _rank[_prev].next = _next;
         }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1438-L1451

### Use already cached `_head` to save 1 SLOAD
```solidity
File: contracts/RubiconMarket.sol
1438:    function _hide(
1439:        uint256 id //id of maker offer to remove from unsorted list
1440:    ) internal returns (bool) {
1441:        uint256 uid = _head; //id of an offer in unsorted offers list // @audit: 1st sload
1442:        uint256 pre = uid; //id of previous offer in unsorted offers list
1443:
1444:        require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list
1445:
1446:        if (_head == id) { // @audit: 2nd sload
1447:            //check if offer is first offer in unsorted offers list
1448:            _head = _near[id]; //set head to new first unsorted offer
1449:            _near[id] = 0; //delete order from unsorted order list
1450:            return true;
1451:        }
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..be97b0a 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1443,7 +1443,7 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {

         require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list

-        if (_head == id) {
+        if (uid == id) {
             //check if offer is first offer in unsorted offers list
             _head = _near[id]; //set head to new first unsorted offer
             _near[id] = 0; //delete order from unsorted order list
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1289-L1290

### Cache `_best[address(t_buy_gem)][address(t_pay_gem)]` to save 1 SLOAD

*More SLOADs will actually be saved since storage slot access is occuring within a loop*
```solidity
File: contracts/RubiconMarket.sol
1289:        while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) { // @audit: 1st sload + on every iteration
1290:            best_maker_id = _best[address(t_buy_gem)][address(t_pay_gem)]; // @audit: 2nd sload + on every iteration
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..15335c9 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarketNew.sol
@@ -1280,14 +1280,13 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
         address owner,
         address recipient
     ) internal returns (uint256 id) {
-        uint256 best_maker_id; //highest maker id
+        uint256 best_maker_id = _best[address(t_buy_gem)][address(t_pay_gem)]; //highest maker id
         uint256 t_buy_amt_old; //taker buy how much saved
         uint256 m_buy_amt; //maker offer wants to buy this much token
         uint256 m_pay_amt; //maker offer wants to sell this much token

         // there is at least one offer stored for token pair
-        while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {
-            best_maker_id = _best[address(t_buy_gem)][address(t_pay_gem)];
+        while (best_maker_id > 0) {
             m_buy_amt = offers[best_maker_id].buy_amt;
             m_pay_amt = offers[best_maker_id].pay_amt;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1405-L1432

### Cache `_span[pay_gem][buy_gem]` to save 1 SLOAD
```solidity
File: contracts/RubiconMarket.sol
1410:        require(_span[pay_gem][buy_gem] > 0); // @audit: 1st sload
...
1432:        _span[pay_gem][buy_gem]--; // @audit: 2nd sload
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..7ffe9c5 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1407,7 +1407,8 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
     ) internal returns (bool) {
         address buy_gem = address(offers[id].buy_gem);
         address pay_gem = address(offers[id].pay_gem);
-        require(_span[pay_gem][buy_gem] > 0);
+        uint256 span = _span[pay_gem][buy_gem];
+        require(span > 0);

         require(
             _rank[id].delb == 0 && //assert id is in the sorted list
@@ -1428,8 +1429,8 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             require(_rank[_rank[id].prev].next == id);
             _rank[_rank[id].prev].next = _rank[id].next;
         }
-
-        _span[pay_gem][buy_gem]--;
+
+        _span[pay_gem][buy_gem] = span - 1;
         _rank[id].delb = block.number; //mark _rank[id] for deletion
         return true;
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L125-L132

### Cache `bathHouseV2` to save 1 SLOAD
```solidity
File: contracts/utilities/poolsUtility/Position.sol
125:    function openPosition(
126:        address asset,
127:        address quote,
128:        uint256 initMargin,
129:        uint256 leverage
130:    ) internal returns (bool OK) {
131:        address bathTokenAsset = bathHouseV2.getBathTokenFromAsset(asset); // @audit: 1st sload
132:        address bathTokenQuote = bathHouseV2.getBathTokenFromAsset(quote); // @audit: 2nd sload
```
```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..5752323 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -128,8 +128,9 @@ contract Position is Ownable, DSMath {
         uint256 initMargin,
         uint256 leverage
     ) internal returns (bool OK) {
-        address bathTokenAsset = bathHouseV2.getBathTokenFromAsset(asset);
-        address bathTokenQuote = bathHouseV2.getBathTokenFromAsset(quote);
+        BathHouseV2 _bathHouseV2 = bathHouseV2;
+        address bathTokenAsset = _bathHouseV2.getBathTokenFromAsset(asset);
+        address bathTokenQuote = _bathHouseV2.getBathTokenFromAsset(quote);

         /// @dev avoid stack too deep
         PosVars memory vars;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L401-L419

### Cache `lastPositionId++` to save 2 SLOADs
```solidity
File: contracts/utilities/poolsUtility/Position.sol
401:    function _savePosition(
402:        address _asset,
403:        address _quote,
404:        uint256 _borrowedAmount,
405:        uint256 _currentBathTokenAmount
406:    ) internal {
407:        lastPositionId++; // @audit: 1st sload
408:        Position memory pos = Position(
409:            _asset,
410:            _quote,
411:            _borrowedAmount,
412:            _currentBathTokenAmount,
413:            block.number,
414:            true
415:        );
416:        positions[lastPositionId] = pos; // @audit: 2nd sload
417:
418:        emit PositionOpened(lastPositionId, pos); // @audit: 3rd sload
419:    }
```
```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..5ae9de8 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -404,7 +404,7 @@ contract Position is Ownable, DSMath {
         uint256 _borrowedAmount,
         uint256 _currentBathTokenAmount
     ) internal {
-        lastPositionId++;
+        uint256 _newLastPositionId = ++lastPositionId;
         Position memory pos = Position(
             _asset,
             _quote,
@@ -413,9 +413,9 @@ contract Position is Ownable, DSMath {
             block.number,
             true
         );
-        positions[lastPositionId] = pos;
+        positions[_newLastPositionId] = pos;

-        emit PositionOpened(lastPositionId, pos);
+        emit PositionOpened(_newLastPositionId, pos);
     }

     function _removePosition(uint256 _positionId) internal {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L454-L473

### Cache `rubiconMarket` to save 3 SLOADs
```solidity
File: contracts/utilities/poolsUtility/Position.sol
454:    function _marketBuy(
455:        address _asset,
456:        address _quote,
457:        uint256 _maxFill
458:    ) internal {
459:        uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000); // @audit: 1st sload
460:        uint256 _buyAmount = rubiconMarket.getBuyAmount( // @audit: 2nd sload
461:            ERC20(_asset),
462:            ERC20(_quote),
463:            _maxFill.sub(_fee)
464:        );
465:        IERC20(_quote).approve(address(rubiconMarket), _maxFill); // @audit: 3rd sload
466:
467:        rubiconMarket.buyAllAmount( // @audit: 4th sload
468:            ERC20(_asset),
469:            _buyAmount,
470:            ERC20(_quote),
471:            _maxFill
472:        );
473:    }
```
```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..29bcff2 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -456,15 +456,16 @@ contract Position is Ownable, DSMath {
         address _quote,
         uint256 _maxFill
     ) internal {
-        uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
-        uint256 _buyAmount = rubiconMarket.getBuyAmount(
+        RubiconMarket _rubiconMarket = rubiconMarket;
+        uint256 _fee = _maxFill.mul(_rubiconMarket.getFeeBPS()).div(10000);
+        uint256 _buyAmount = _rubiconMarket.getBuyAmount(
             ERC20(_asset),
             ERC20(_quote),
             _maxFill.sub(_fee)
         );
-        IERC20(_quote).approve(address(rubiconMarket), _maxFill);
+        IERC20(_quote).approve(address(_rubiconMarket), _maxFill);

-        rubiconMarket.buyAllAmount(
+        _rubiconMarket.buyAllAmount(
             ERC20(_asset),
             _buyAmount,
             ERC20(_quote),
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L475-L511

### Cache `rubiconMarket` to save 3 SLOADs
```solidity
File: contracts/utilities/poolsUtility/Position.sol
475:    function _marketSell(
476:        address _asset,
477:        address _quote,
478:        uint256 _minFill
479:    ) internal {
480:        uint256 _feeBPS = rubiconMarket.getFeeBPS(); // @audit: 1st sload
481:        uint256 _fee = _minFill.mul(_feeBPS).div(10000);
482:        uint256 _payAmount = rubiconMarket.getPayAmount( // @audit: 2nd sload
483:            ERC20(_asset),
484:            ERC20(_quote),
485:            _minFill.add(_fee)
486:        );
487:        uint256 _assetBalance = IERC20(_asset).balanceOf(address(this));
488:
489:        /// @dev recalculate fee in _asset form
490:        _fee = _payAmount.mul(_feeBPS).div(10000);
491:
492:        if (_assetBalance < _payAmount) {
493:            IERC20(_asset).transferFrom(
494:                msg.sender,
495:                address(this),
496:                _payAmount.sub(_assetBalance).add(_fee)
497:            );
498:        }
499:
500:        IERC20(_asset).approve(
501:            address(rubiconMarket), // @audit: 3rd sload
502:            IERC20(_asset).balanceOf(address(this))
503:        );
504:
505:        rubiconMarket.sellAllAmount( // @audit: 4th sload
506:            ERC20(_asset),
507:            _payAmount,
508:            ERC20(_quote),
509:            _minFill
510:        );
511:    }
```
```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..da3dac8 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -477,9 +477,10 @@ contract Position is Ownable, DSMath {
         address _quote,
         uint256 _minFill
     ) internal {
-        uint256 _feeBPS = rubiconMarket.getFeeBPS();
+        RubiconMarket _rubiconMarket = rubiconMarket;
+        uint256 _feeBPS = _rubiconMarket.getFeeBPS();
         uint256 _fee = _minFill.mul(_feeBPS).div(10000);
-        uint256 _payAmount = rubiconMarket.getPayAmount(
+        uint256 _payAmount = _rubiconMarket.getPayAmount(
             ERC20(_asset),
             ERC20(_quote),
             _minFill.add(_fee)
@@ -498,11 +499,11 @@ contract Position is Ownable, DSMath {
         }

         IERC20(_asset).approve(
-            address(rubiconMarket),
+            address(_rubiconMarket),
             IERC20(_asset).balanceOf(address(this))
         );

-        rubiconMarket.sellAllAmount(
+        _rubiconMarket.sellAllAmount(
             ERC20(_asset),
             _payAmount,
             ERC20(_quote),
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L112-L119

### Cache `periodFinish[token]` to save 1 SLOAD
```solidity
File: contracts/periphery/BathBuddy.sol
112:    function lastTimeRewardApplicable(
113:        address token
114:    ) public view returns (uint256) {
115:        return
116:            block.timestamp < periodFinish[token] // @audit: 1st sload
117:                ? block.timestamp
118:                : periodFinish[token]; // @audit: 2nd sload
119:    }
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..758f41b 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -112,10 +112,11 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
     function lastTimeRewardApplicable(
         address token
     ) public view returns (uint256) {
+        uint256 _periodFinish = periodFinish[token];
         return
-            block.timestamp < periodFinish[token]
+            block.timestamp < _periodFinish
                 ? block.timestamp
-                : periodFinish[token];
+                : _periodFinish;
     }

     function rewardPerToken(address token) public view returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L191-L228

### Cache `rewardsDuration[address(rewardsToken)]` and `rewardRates[address(rewardsToken)]` to save 3 SLOADs
```solidity
File: contracts/periphery/BathBuddy.sol
191:    function notifyRewardAmount(
192:        uint256 reward,
193:        IERC20 rewardsToken
194:    ) external onlyOwner updateReward(address(0), address(rewardsToken)) {
195:        if (block.timestamp >= periodFinish[address(rewardsToken)]) {
196:            rewardRates[address(rewardsToken)] = reward.div(
197:                rewardsDuration[address(rewardsToken)] // @audit: 1st sload
198:            );
199:        } else {
200:            uint256 remaining = periodFinish[address(rewardsToken)].sub(
201:                block.timestamp
202:            );
203:            uint256 leftover = remaining.mul(
204:                rewardRates[address(rewardsToken)] // @audit: 1st sload
205:            );
206:            rewardRates[address(rewardsToken)] = reward.add(leftover).div(
207:                rewardsDuration[address(rewardsToken)] // @audit: 1st sload
208:            );
209:        }
210:
211:        // Ensure the provided reward amount is not more than the balance in the contract.
212:        // This keeps the reward rate in the right range, preventing overflows due to
213:        // very high values of rewardRate in the earned and rewardsPerToken functions;
214:        // Reward + leftover must be less than 2^256 / 10^18 to avoid overflow.
215:        // Note********** ERC20s must be here*************
216:        uint256 balance = rewardsToken.balanceOf(address(this));
217:        require(
218:            rewardRates[address(rewardsToken)] <= // @audit: 2nd sload
219:                balance.div(rewardsDuration[address(rewardsToken)]), // @audit: 2nd sload
220:            "Provided reward too high"
221:        );
222:
223:        lastUpdateTime[address(rewardsToken)] = block.timestamp;
224:        periodFinish[address(rewardsToken)] = block.timestamp.add(
225:            rewardsDuration[address(rewardsToken)] // @audit: 3rd sload
226:        );
227:        emit RewardAdded(reward);
228:    }
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..e613e11 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -192,19 +192,21 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         uint256 reward,
         IERC20 rewardsToken
     ) external onlyOwner updateReward(address(0), address(rewardsToken)) {
+        uint256 _rewardsDuration = rewardsDuration[address(rewardsToken)];
+        uint256 _rewardRates = rewardRates[address(rewardsToken)];
         if (block.timestamp >= periodFinish[address(rewardsToken)]) {
             rewardRates[address(rewardsToken)] = reward.div(
-                rewardsDuration[address(rewardsToken)]
+                _rewardsDuration
             );
         } else {
             uint256 remaining = periodFinish[address(rewardsToken)].sub(
                 block.timestamp
             );
             uint256 leftover = remaining.mul(
-                rewardRates[address(rewardsToken)]
+                _rewardRates
             );
             rewardRates[address(rewardsToken)] = reward.add(leftover).div(
-                rewardsDuration[address(rewardsToken)]
+                _rewardsDuration
             );
         }

@@ -215,14 +217,14 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         // Note********** ERC20s must be here*************
         uint256 balance = rewardsToken.balanceOf(address(this));
         require(
-            rewardRates[address(rewardsToken)] <=
-                balance.div(rewardsDuration[address(rewardsToken)]),
+            _rewardRates <=
+                balance.div(_rewardsDuration),
             "Provided reward too high"
         );

         lastUpdateTime[address(rewardsToken)] = block.timestamp;
         periodFinish[address(rewardsToken)] = block.timestamp.add(
-            rewardsDuration[address(rewardsToken)]
+            _rewardsDuration
         );
         emit RewardAdded(reward);
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L247-L257

### Cache `rewardPerToken(token)` to save 1 SLOAD
```solidity
File: contracts/periphery/BathBuddy.sol
247:    modifier updateReward(address account, address token) {
248:        rewardsPerTokensStored[token] = rewardPerToken(token);
249:        lastUpdateTime[token] = lastTimeRewardApplicable(token);
250:        if (account != address(0)) {
251:            tokenRewards[token][account] = earned(account, token);
252:            userRewardsPerTokenPaid[token][account] = rewardsPerTokensStored[
253:                token
254:            ];
255:        }
256:        _;
257:    }
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..59d652a 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -245,13 +245,12 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {

     // Rewards set here
     modifier updateReward(address account, address token) {
-        rewardsPerTokensStored[token] = rewardPerToken(token);
+        uint256 _rewardsPerTokensStored = rewardPerToken(token);
+        rewardsPerTokensStored[token] = _rewardsPerTokensStored;
         lastUpdateTime[token] = lastTimeRewardApplicable(token);
         if (account != address(0)) {
             tokenRewards[token][account] = earned(account, token);
-            userRewardsPerTokenPaid[token][account] = rewardsPerTokensStored[
-                token
-            ];
+            userRewardsPerTokenPaid[token][account] = _rewardsPerTokensStored;
         }
         _;
     }
```

## Avoid emitting storage values
Caching of a state variable replaces each `Gwarmaccess (100 gas)` with a much cheaper stack read. We can avoid unecessary SLOADs by caching storage values that were previously accessed and emitting those cached values.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L232-L242

### Emit `_rewardsDuration` instead of reading from storage
```solidity
File: contracts/periphery/BathBuddy.sol
232:    function setRewardsDuration(
233:        uint256 _rewardsDuration,
234:        address token
235:    ) external onlyOwner {
236:        require(
237:            block.timestamp > periodFinish[token],
238:            "Previous rewards period must be complete before changing the duration for the new period"
239:        );
240:        rewardsDuration[token] = _rewardsDuration;
241:        emit RewardsDurationUpdated(rewardsDuration[token]); // @audit: unecessary SLOAD, emit _rewardsDuration
242:    }
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..171d14b 100644
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

## Structs can be packed to use fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if values combined are <= 32 bytes). If the variables packed together are retrieved together in functions (more likely with structs) we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)`.

Total Instances: `2`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L33-L40

### Pack `quote`, `blockNum`, and `isActive` into one storage slot & `borrowedAmount` and `bathTokenAmount` into one storage slot to save 3 SLOTS (~6000 gas)

*`blockNum` can have type `uint80` without fear of overflowing. `uint128` should be large enough to hold `borrowedAmount` and `bathTokenAmount`.*

**Note: the `leverage-wrapper.ts` test must be modified for this optimization since the struct layout changes. The changes should be as simple as changing the index for `position[x]` and `positions[x]` so that you grab the correct value from the struct. Since changes to the tests are needed, the [final diffs](https://gist.github.com/0xJCN/69437a989c5a2a2ef4ae45a7ff13cac7) only include the changes to the uint types (no re-ordering) so that gas savings can be seen without any changes to the tests.**
```solidity
File: contracts/utilities/poolsUtility/Position.sol
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
index 7c72d4f..e71120e 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -31,11 +31,11 @@ contract Position is Ownable, DSMath {
     }

     struct Position {
+        uint128 borrowedAmount; // amount of borrowed quote
+        uint128 bathTokenAmount; // amount of bathTokens to which collateral was supplied
         address asset; // supplied as collateral
         address quote; // borrowed token
-        uint256 borrowedAmount; // amount of borrowed quote
-        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
-        uint256 blockNum; // block number on which position was opened
+        uint80 blockNum; // block number on which position was opened
         bool isActive; // false by default, when active - true
     }
     // position id => Position struct
@@ -406,11 +406,11 @@ contract Position is Ownable, DSMath {
     ) internal {
         lastPositionId++;
         Position memory pos = Position(
+            uint128(_borrowedAmount),
+            uint128(_currentBathTokenAmount),
             _asset,
             _quote,
-            _borrowedAmount,
-            _currentBathTokenAmount,
-            block.number,
+            uint80(block.number),
             true
         );
         positions[lastPositionId] = pos;
@@ -428,7 +428,7 @@ contract Position is Ownable, DSMath {
         uint256 _bathTokenAmount,
         uint256 _marginAmount
     ) internal {
-        positions[_positionId].bathTokenAmount += _bathTokenAmount;
+        positions[_positionId].bathTokenAmount += uint128(_bathTokenAmount);

         emit MarginIncreased(_positionId, _marginAmount);
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L686-L690

### Pack `next`, `prev` and `delb` into a single storage slot to save 2 SLOTS (~2000 gas)

*`uint80` should be large enough to hold offer ids and timestamps without fear of overflowing*
```solidity
File: contracts/RubiconMarket.sol
686:    struct sortInfo {
687:        uint256 next; //points to id of next higher offer
688:        uint256 prev; //points to id of previous lower offer
689:        uint256 delb; //the blocknumber where this entry was marked for delete
690:    }
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..c662d52 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -684,9 +684,9 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
     address public AqueductAddress;

     struct sortInfo {
-        uint256 next; //points to id of next higher offer
-        uint256 prev; //points to id of previous lower offer
-        uint256 delb; //the blocknumber where this entry was marked for delete
+        uint80 next; //points to id of next higher offer
+        uint80 prev; //points to id of previous lower offer
+        uint80 delb; //the blocknumber where this entry was marked for delete
     }
     mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
     mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
@@ -1381,8 +1381,8 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             //requirement below is satisfied by statements above
             //require(_isPricedLtOrEq(id, pos));
             prev_id = _rank[pos].prev;
-            _rank[pos].prev = id;
-            _rank[id].next = pos;
+            _rank[pos].prev = uint80(id);
+            _rank[id].next = uint80(pos);
         } else {
             //offers[id] is the highest offer
             prev_id = _best[address(pay_gem)][address(buy_gem)];
@@ -1393,8 +1393,8 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             //if lower offer does exist
             //requirement below is satisfied by statements above
             //require(!_isPricedLtOrEq(id, prev_id));
-            _rank[prev_id].next = id;
-            _rank[id].prev = prev_id;
+            _rank[prev_id].next = uint80(id);
+            _rank[id].prev = uint80(prev_id);
         }

         _span[address(pay_gem)][address(buy_gem)]++;
@@ -1430,7 +1430,7 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
         }

         _span[pay_gem][buy_gem]--;
-        _rank[id].delb = block.number; //mark _rank[id] for deletion
+        _rank[id].delb = uint80(block.number); //mark _rank[id] for deletion
         return true;
     }
```

## State variables can be packed to use fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if the values combined are <= 32 bytes). If the variables packed together are retrieved together in functions we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L227-L230

### Pack `feeBPS` and `feeTo` into one storage slot to save 1 SLOT (~2000 gas)

*`feeBPS` is able to be of type `uint96` without fear of overflowing*

*Both state variables are accessed in RubiconMarket.buy*
```solidity
File: contracts/RubiconMarket.sol
227:    uint256 internal feeBPS;
228:
229:    /// @dev This parameter provides the address to which fees are sent
230:    address internal feeTo;
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..9f284d1 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -223,12 +223,12 @@ contract SimpleMarket is EventfulMarket, DSMath {

     bool locked;

-    /// @dev This parameter is in basis points
-    uint256 internal feeBPS;
-
     /// @dev This parameter provides the address to which fees are sent
     address internal feeTo;

+    /// @dev This parameter is in basis points
+    uint96 internal feeBPS;
+
     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

     struct OfferInfo {
@@ -1464,7 +1464,7 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
     }

     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
-        feeBPS = _newFeeBPS;
+        feeBPS = uint96(_newFeeBPS);
         return true;
     }
```

## Rearrange storage variables to pack values more efficiently
`friendshipStarted` and `myBathTokenBuddy` would benefit more from being stored in the same storage slot because they are both retrieved in `rewardPerToken` and `earned`. `frienshipStarted` and `bathHouse` are both retrieved only in the `getReward` function.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L46-L49

*Gas Savings for `notifyRewardAmount`, obtained via protocol's tests: Avg 2000 gas* 

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  113455 |    1    |
| After  |  -  |  -  |  111455 |    1    |

```solidity
File: contracts/periphery/BathBuddy.sol
46:    address public owner;
47:    address public myBathTokenBuddy;
48:    address public bathHouse;
49:    bool public friendshipStarted;
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..a05bb11 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -44,8 +44,8 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
     // WILL BE BATH HOUSE IS OWNER
     /// BATH TOKEN ONLY ENTRYPOINTs
     address public owner;
-    address public myBathTokenBuddy;
     address public bathHouse;
+    address public myBathTokenBuddy;
     bool public friendshipStarted;
```

## Return values from external calls can be cached to avoid unecessary call
External calls are expensive as they use the `CALL` opcode (~100 gas). If you are calling the same external function more than once you should cache the return value to avoid an unecessary `CALL`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L121-L135

### Cache `IERC20(myBathTokenBuddy).totalSupply()`

*Gas Savings for `BathBuddy.claimRewards`, obtained via protocol's tests: Avg 1798 gas* 

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  265601 |    1    |
| After  |  -  |  -  |  263803 |    1    |

```solidity
File: contracts/periphery/BathBuddy.sol
121:    function rewardPerToken(address token) public view returns (uint256) {
122:        require(friendshipStarted, "I have not started a bathToken friendship");
123:
124:        if (IERC20(myBathTokenBuddy).totalSupply() == 0) { // @audit: 1st external call
125:            return rewardsPerTokensStored[token];
126:        }
127:        return
128:            rewardsPerTokensStored[token].add(
129:                lastTimeRewardApplicable(token)
130:                    .sub(lastUpdateTime[token])
131:                    .mul(rewardRates[token])
132:                    .mul(1e18)
133:                    .div(IERC20(myBathTokenBuddy).totalSupply()) // @audit: 2nd external call
134:            );
135:    }
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..b5a9de7 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -120,8 +120,9 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {

     function rewardPerToken(address token) public view returns (uint256) {
         require(friendshipStarted, "I have not started a bathToken friendship");
-
-        if (IERC20(myBathTokenBuddy).totalSupply() == 0) {
+
+        uint256 _totalSupply = IERC20(myBathTokenBuddy).totalSupply();
+        if (_totalSupply == 0) {
             return rewardsPerTokensStored[token];
         }
         return
@@ -130,7 +131,7 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
                     .sub(lastUpdateTime[token])
                     .mul(rewardRates[token])
                     .mul(1e18)
-                    .div(IERC20(myBathTokenBuddy).totalSupply())
+                    .div(_totalSupply)
             );
     }
```

## Multiple accesses of a mapping/array should use a storage pointer
Caching a mapping's value in a storage pointer when the value is accessed multiple times saves ~40 gas per access due to not having to perform the same offset calculation every time. Help the Optimizer by saving a storage variable's reference instead of repeatedly fetching it.

To achieve this, declare a storage pointer for the variable and use it instead of repeatedly fetching the reference in a map or an array. As an example, instead of repeatedly calling `offers[id]`, save its reference via a storage pointer: `OfferInfo storage _offer = offers[id]` and use the pointer instead.

Total Instances: `43`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L318-L436

### Cache storage pointer for `offers[id]`

*Gas Savings for `RubiconMarket.buy`, obtained via protocol's tests: Avg 218 gas*

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  127629  |  160126  |  142006 |    3     |
| After  |  127411  |  159908  |  141788 |    3     |

```solidity
File: contracts/RubiconMarket.sol
318:        OfferInfo memory _offer = offers[id];
...
334:        offers[id].pay_amt = sub(_offer.pay_amt, quantity);
335:        offers[id].buy_amt = sub(_offer.buy_amt, spend);
...
436:        if (offers[id].pay_amt == 0) {
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..2c093ed 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -315,7 +315,8 @@ contract SimpleMarket is EventfulMarket, DSMath {
         uint256 id,
         uint256 quantity
     ) public virtual can_buy(id) synchronized returns (bool) {
-        OfferInfo memory _offer = offers[id];
+        OfferInfo storage _offerStore = offers[id];
+        OfferInfo memory _offer = _offerStore;
         uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;

         require(uint128(spend) == spend, "spend is not an int");
@@ -331,8 +332,8 @@ contract SimpleMarket is EventfulMarket, DSMath {
             return false;
         }

-        offers[id].pay_amt = sub(_offer.pay_amt, quantity);
-        offers[id].buy_amt = sub(_offer.buy_amt, spend);
+        _offerStore.pay_amt = sub(_offer.pay_amt, quantity);
+        _offerStore.buy_amt = sub(_offer.buy_amt, spend);

         /// @dev Fee logic added on taker trades
         uint256 fee = mul(spend, feeBPS) / 100_000;
@@ -433,7 +434,7 @@ contract SimpleMarket is EventfulMarket, DSMath {
         ///     address(_offer.buy_gem)
         /// );

-        if (offers[id].pay_amt == 0) {
+        if (_offerStore.pay_amt == 0) {
             delete offers[id];
             /// emit OfferDeleted(bytes32(id));
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1043-L1059

### Cache storage pointer for `offers[offerId]`

*Gas Savings for `RubiconMarket.sellAllAmount`, obtained via protocol's tests: Avg 140 gas*

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  145248 |    1     |
| After  |  -  |  -  |  145108 |    1     |

```solidity
File: contracts/RubiconMarket.sol
1043:            if (
1044:                mul(pay_amt, 1 ether) <
1045:                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
1046:            ) {
1047:                break; //We consider that all amount is sold
1048:            }
1049:            if (pay_amt >= offers[offerId].buy_amt) {
1050:                //If amount to sell is higher or equal than current offer amount to buy
1051:                fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator
1052:                pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell
1053:                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
1054:            } else {
1055:                // if lower
1056:                uint256 baux = rmul(
1057:                    mul(pay_amt, 10 ** 9),
1058:                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
1059:                ) / 10 ** 9;
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..2f9bd66 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1040,22 +1040,23 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             require(offerId != 0, "0 offerId"); //Fails if there are not more offers

             // There is a chance that pay_amt is smaller than 1 wei of the other token
+            OfferInfo storage _offer = offers[offerId];
             if (
                 mul(pay_amt, 1 ether) <
-                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
+                wdiv(_offer.buy_amt, _offer.pay_amt)
             ) {
                 break; //We consider that all amount is sold
             }
-            if (pay_amt >= offers[offerId].buy_amt) {
+            if (pay_amt >= _offer.buy_amt) {
                 //If amount to sell is higher or equal than current offer amount to buy
-                fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator
-                pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell
-                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
+                fill_amt = add(fill_amt, _offer.pay_amt); //Add amount bought to acumulator
+                pay_amt = sub(pay_amt, _offer.buy_amt); //Decrease amount to sell
+                take(bytes32(offerId), uint128(_offer.pay_amt)); //We take the whole offer
             } else {
                 // if lower
                 uint256 baux = rmul(
                     mul(pay_amt, 10 ** 9),
-                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
+                    rdiv(_offer.pay_amt, _offer.buy_amt)
                 ) / 10 ** 9;
                 fill_amt = add(fill_amt, baux); //Add amount bought to acumulator
                 take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1083-L1101

### Cache storage pointer for `offers[offerId]`

*Gas Savings for `RubiconMarket.buyAllAmountForETH`, obtained via protocol's tests: Avg 105 gas*

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  217184 |    1     |
| After  |  -  |  -  |  217079 |    1     |

```solidity
File: contracts/RubiconMarket.sol
1083:            if (
1084:                mul(buy_amt, 1 ether) <
1085:                wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
1086:            ) {
1087:                break; //We consider that all amount is sold
1088:            }
1089:            if (buy_amt >= offers[offerId].pay_amt) {
1090:                //If amount to buy is higher or equal than current offer amount to sell
1091:                fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator
1092:                buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy
1093:                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
1094:            } else {
1095:                //if lower
1096:                fill_amt = add(
1097:                    fill_amt,
1098:                    rmul(
1099:                        mul(buy_amt, 10 ** 9),
1100:                        rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
1101:                    ) / 10 ** 9
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..be86e8c 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1080,24 +1080,25 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             require(offerId != 0, "offerId == 0");

             // There is a chance that buy_amt is smaller than 1 wei of the other token
+            OfferInfo storage _offer = offers[offerId];
             if (
                 mul(buy_amt, 1 ether) <
-                wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
+                wdiv(_offer.pay_amt, _offer.buy_amt)
             ) {
                 break; //We consider that all amount is sold
             }
-            if (buy_amt >= offers[offerId].pay_amt) {
+            if (buy_amt >= _offer.pay_amt) {
                 //If amount to buy is higher or equal than current offer amount to sell
-                fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator
-                buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy
-                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
+                fill_amt = add(fill_amt, _offer.buy_amt); //Add amount sold to acumulator
+                buy_amt = sub(buy_amt, _offer.pay_amt); //Decrease amount to buy
+                take(bytes32(offerId), uint128(_offer.pay_amt)); //We take the whole offer
             } else {
                 //if lower
                 fill_amt = add(
                     fill_amt,
                     rmul(
                         mul(buy_amt, 10 ** 9),
-                        rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
+                        rdiv(_offer.buy_amt, _offer.pay_amt)
                     ) / 10 ** 9
                 ); //Add amount sold to acumulator
                 take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1130-L1144

### Cache storage pointer for `offers[offerId]`

```solidity
File: contracts/RubiconMarket.sol
1130:        while (pay_amt > offers[offerId].buy_amt) {
1131:            fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount to buy accumulator
1132:            pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to pay
1133:            if (pay_amt > 0) {
1134:                //If we still need more offers
1135:                offerId = getWorseOffer(offerId); //We look for the next best offer
1136:                require(offerId != 0); //Fails if there are not enough offers to complete
1137:            }
1138:        }
1139:        fill_amt = add(
1140:            fill_amt,
1141:            rmul(
1142:                mul(pay_amt, 10 ** 9),
1143:                rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
1144:           ) / 10 ** 9
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..fba762d 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1127,9 +1127,10 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
         uint256 pay_amt
     ) public view returns (uint256 fill_amt) {
         uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair
-        while (pay_amt > offers[offerId].buy_amt) {
-            fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount to buy accumulator
-            pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to pay
+        OfferInfo storage _offer = offers[offerId];
+        while (pay_amt > _offer.buy_amt) {
+            fill_amt = add(fill_amt, _offer.pay_amt); //Add amount to buy accumulator
+            pay_amt = sub(pay_amt, _offer.buy_amt); //Decrease amount to pay
             if (pay_amt > 0) {
                 //If we still need more offers
                 offerId = getWorseOffer(offerId); //We look for the next best offer
@@ -1140,7 +1141,7 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             fill_amt,
             rmul(
                 mul(pay_amt, 10 ** 9),
-                rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
+                rdiv(_offer.pay_amt, _offer.buy_amt)
             ) / 10 ** 9
         ); //Add proportional amount of last offer to buy accumulator
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1163-L1177

### Cache storage pointer for `offers[offerId]`

```solidity
File: contracts/RubiconMarket.sol
1163:        while (buy_amt > offers[offerId].pay_amt) {
1164:            fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount to pay accumulator
1165:            buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy
1167:            if (buy_amt > 0) {
1168:                //If we still need more offers
1169:                offerId = getWorseOffer(offerId); //We look for the next best offer
1170:                require(offerId != 0); //Fails if there are not enough offers to complete
1171:            }
1172:        }
1173:        fill_amt = add(
1174:            fill_amt,
1175:            rmul(
1176:                mul(buy_amt, 10 ** 9),
1177:                rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
1178:            ) / 10 ** 9
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..9263388 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1160,9 +1160,10 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
         uint256 buy_amt
     ) public view returns (uint256 fill_amt) {
         uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair
-        while (buy_amt > offers[offerId].pay_amt) {
-            fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount to pay accumulator
-            buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy
+        OfferInfo storage _offer = offers[offerId];
+        while (buy_amt > _offer.pay_amt) {
+            fill_amt = add(fill_amt, _offer.buy_amt); //Add amount to pay accumulator
+            buy_amt = sub(buy_amt, _offer.pay_amt); //Decrease amount to buy
             if (buy_amt > 0) {
                 //If we still need more offers
                 offerId = getWorseOffer(offerId); //We look for the next best offer
@@ -1173,7 +1174,7 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             fill_amt,
             rmul(
                 mul(buy_amt, 10 ** 9),
-                rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
+                rdiv(_offer.buy_amt, _offer.pay_amt)
             ) / 10 ** 9
         ); //Add proportional amount of last offer to pay accumulator
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1183-L1203

### Cache storage pointer for `offers[id]`

*Gas Savings for `RubiconMarket.buy`, obtained via protocol's tests: Avg 85 gas*

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  127629  |  160126  |  142006 |    3     |
| After  |  127544  |  160041  |  141921 |    3     |

```solidity
File: contracts/RubiconMarket.sol
1183:    function _buys(uint256 id, uint256 amount) internal returns (bool) {
1184:        require(buyEnabled);
1185:        if (amount == offers[id].pay_amt) {
1186:            if (isOfferSorted(id)) {
1187:                //offers[id] must be removed from sorted list because all of it is bought
1188:                _unsort(id);
1189:            } else {
1190:                _hide(id);
1191:            }
1192:        }
1193:
1194:        require(super.buy(id, amount));
1195:
1196:        // If offer has become dust during buy, we cancel it
1197:        if (
1198:            isActive(id) &&
1199:            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]
1200:        ) {
1201:            dustId = id; //enable current msg.sender to call cancel(id)
1202:            cancel(id);
1203:        }
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..8f5431d 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1182,7 +1182,8 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {

     function _buys(uint256 id, uint256 amount) internal returns (bool) {
         require(buyEnabled);
-        if (amount == offers[id].pay_amt) {
+        OfferInfo storage _offer = offers[id];
+        if (amount == _offer.pay_amt) {
             if (isOfferSorted(id)) {
                 //offers[id] must be removed from sorted list because all of it is bought
                 _unsort(id);
@@ -1196,7 +1197,7 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
         // If offer has become dust during buy, we cancel it
         if (
             isActive(id) &&
-            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]
+            _offer.pay_amt < _dust[address(_offer.pay_gem)]
         ) {
             dustId = id; //enable current msg.sender to call cancel(id)
             cancel(id);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1208-L1212

### Cache storage pointer for `offers[id]`

*Gas Savings for `RubiconMarket.offer`, obtained via protocol's tests: Avg 9 gas*

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  112423  |  292961  |  259582 |    28    |
| After  |  112423  |  292951  |  259573 |    28    |

```solidity
File: contracts/RubiconMarket.sol
1208:    function _find(uint256 id) internal view returns (uint256) {
1209:        require(id > 0);
1210:
1211:        address buy_gem = address(offers[id].buy_gem);
1212:        address pay_gem = address(offers[id].pay_gem);
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..5dbeb25 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1207,9 +1207,10 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
     //find the id of the next higher offer after offers[id]
     function _find(uint256 id) internal view returns (uint256) {
         require(id > 0);
-
-        address buy_gem = address(offers[id].buy_gem);
-        address pay_gem = address(offers[id].pay_gem);
+
+        OfferInfo storage _offer = offers[id];
+        address buy_gem = address(_offer.buy_gem);
+        address pay_gem = address(_offer.pay_gem);
         uint256 top = _best[pay_gem][buy_gem];
         uint256 old_top = 0;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1261-L1268

### Cache storage pointers for `offer[low]` and `offers[high]`

*Gas Savings for `RubiconMarket.offer`, obtained via protocol's tests: Avg 173 gas*

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  112423  |  292961  |  259582 |    28    |
| After  |  112423  |  292396  |  259409 |    28    |

```solidity
File: contracts/RubiconMarket.sol
1261:    function _isPricedLtOrEq(
1262:        uint256 low, //lower priced offer's id
1263:        uint256 high //higher priced offer's id
1264:    ) internal view returns (bool) {
1265:        return
1266:            mul(offers[low].buy_amt, offers[high].pay_amt) >=
1267:            mul(offers[high].buy_amt, offers[low].pay_amt);
1268:    }
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..cc1f056 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1262,9 +1262,11 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
         uint256 low, //lower priced offer's id
         uint256 high //higher priced offer's id
     ) internal view returns (bool) {
+        OfferInfo storage _offerLow = offers[low];
+        OfferInfo storage _offerHigh = offers[high];
         return
-            mul(offers[low].buy_amt, offers[high].pay_amt) >=
-            mul(offers[high].buy_amt, offers[low].pay_amt);
+            mul(_offerLow.buy_amt, _offerHigh.pay_amt) >=
+            mul(_offerHigh.buy_amt, _offerLow.pay_amt);
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1020-L1026

### Cache storage pointer for `_rank[id]`

*Gas Savings for `RubiconMarket.cancel`, obtained via protocol's tests: Avg 98 gas*

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  118396 |    1    |
| After  |  -  |  -  |  118298 |    1    |

```solidity
File: contracts/RubiconMarket.sol
1020:    function isOfferSorted(uint256 id) public view returns (bool) {
1021:        return
1022:            _rank[id].next != 0 ||
1023:            _rank[id].prev != 0 ||
1024:            _best[address(offers[id].pay_gem)][address(offers[id].buy_gem)] ==
1025:            id;
1026:    }
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..71d3067 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1018,9 +1018,10 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
     }

     function isOfferSorted(uint256 id) public view returns (bool) {
+        sortInfo storage _sort = _rank[id];
         return
-            _rank[id].next != 0 ||
-            _rank[id].prev != 0 ||
+            _sort.next != 0 ||
+            _sort.prev != 0 ||
             _best[address(offers[id].pay_gem)][address(offers[id].buy_gem)] ==
             id;
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1412-L1433

### Cache storage pointers for `_rank[id]`, `_rank[_rank[id].next]`, and `_rank[_rank[id].prev]`

*Gas Savings for `RubiconMarket.cancel`, obtained via protocol's tests: Avg 155 gas*

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  118396 |    1    |
| After  |  -  |  -  |  118241 |    1    |

```solidity
1412:        require(
1413:            _rank[id].delb == 0 && //assert id is in the sorted list
1414:                isOfferSorted(id)
1415:        );
1416:
1417:        if (id != _best[pay_gem][buy_gem]) {
1418:            // offers[id] is not the highest offer
1419:            require(_rank[_rank[id].next].prev == id);
1420:            _rank[_rank[id].next].prev = _rank[id].prev;
1421:        } else {
1422:            //offers[id] is the highest offer
1423:            _best[pay_gem][buy_gem] = _rank[id].prev;
1424:        }
1425:
1426:        if (_rank[id].prev != 0) {
1427:            //offers[id] is not the lowest offer
1428:            require(_rank[_rank[id].prev].next == id);
1429:            _rank[_rank[id].prev].next = _rank[id].next;
1430:        }
1431:
1432:        _span[pay_gem][buy_gem]--;
1433:        _rank[id].delb = block.number; //mark _rank[id] for deletion
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..30d14b1 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1408,29 +1408,32 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
         address buy_gem = address(offers[id].buy_gem);
         address pay_gem = address(offers[id].pay_gem);
         require(_span[pay_gem][buy_gem] > 0);
-
+
+        sortInfo storage _sort = _rank[id];
         require(
-            _rank[id].delb == 0 && //assert id is in the sorted list
+            _sort.delb == 0 && //assert id is in the sorted list
                 isOfferSorted(id)
         );

         if (id != _best[pay_gem][buy_gem]) {
             // offers[id] is not the highest offer
-            require(_rank[_rank[id].next].prev == id);
-            _rank[_rank[id].next].prev = _rank[id].prev;
+            sortInfo storage _sortNext = _rank[_sort.next];
+            require(_sortNext.prev == id);
+            _sortNext.prev = _sort.prev;
         } else {
             //offers[id] is the highest offer
-            _best[pay_gem][buy_gem] = _rank[id].prev;
+            _best[pay_gem][buy_gem] = _sort.prev;
         }

-        if (_rank[id].prev != 0) {
+        if (_sort.prev != 0) {
             //offers[id] is not the lowest offer
-            require(_rank[_rank[id].prev].next == id);
-            _rank[_rank[id].prev].next = _rank[id].next;
+            sortInfo storage _sortPrev = _rank[_sort.prev];
+            require(_sortPrev.next == id);
+            _sortPrev.next = _sort.next;
         }

         _span[pay_gem][buy_gem]--;
-        _rank[id].delb = block.number; //mark _rank[id] for deletion
+        _sort.delb = block.number; //mark _rank[id] for deletion
         return true;
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1417-L1424

### Cache storage pointer for `_best[pay_gem]`

*Gas Savings for `RubiconMarket.cancel`, obtained via protocol's tests: Avg 52 gas*

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  118396 |    1    |
| After  |  -  |  -  |  118344 |    1    |

```solidity
File: contracts/RubiconMarket.sol
1417:        if (id != _best[pay_gem][buy_gem]) {
1418:            // offers[id] is not the highest offer
1419:            require(_rank[_rank[id].next].prev == id);
1420:            _rank[_rank[id].next].prev = _rank[id].prev;
1421:        } else {
1422:            //offers[id] is the highest offer
1423:            _best[pay_gem][buy_gem] = _rank[id].prev;
1424:        }
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..3bc7038 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1413,14 +1413,15 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             _rank[id].delb == 0 && //assert id is in the sorted list
                 isOfferSorted(id)
         );
-
-        if (id != _best[pay_gem][buy_gem]) {
+
+        mapping(address => uint256) storage _bestOffer = _best[pay_gem];
+        if (id != _bestOffer[buy_gem]) {
             // offers[id] is not the highest offer
             require(_rank[_rank[id].next].prev == id);
             _rank[_rank[id].next].prev = _rank[id].prev;
         } else {
             //offers[id] is the highest offer
-            _best[pay_gem][buy_gem] = _rank[id].prev;
+            _bestOffer[buy_gem] = _rank[id].prev;
         }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1410-L1432

### Cache storage pointer for `_span[pay_gem]`

*Gas Savings for `RubiconMarket.cancel`, obtained via protocol's tests: Avg 52 gas*

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  118396 |    1    |
| After  |  -  |  -  |  118344 |    1    |

```solidity
File: contracts/RubiconMarket.sol
1410:        require(_span[pay_gem][buy_gem] > 0);
1411:
1412:        require(
1413:            _rank[id].delb == 0 && //assert id is in the sorted list
1414:                isOfferSorted(id)
1415:        );
1416:
1417:        if (id != _best[pay_gem][buy_gem]) {
1418:            // offers[id] is not the highest offer
1419:            require(_rank[_rank[id].next].prev == id);
1420:            _rank[_rank[id].next].prev = _rank[id].prev;
1421:        } else {
1422:            //offers[id] is the highest offer
1423:            _best[pay_gem][buy_gem] = _rank[id].prev;
1424:        }
1425:
1426:        if (_rank[id].prev != 0) {
1427:            //offers[id] is not the lowest offer
1428:            require(_rank[_rank[id].prev].next == id);
1429:            _rank[_rank[id].prev].next = _rank[id].next;
1430:        }
1431:
1432:        _span[pay_gem][buy_gem]--;
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..13310c8 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -1407,7 +1407,8 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
     ) internal returns (bool) {
         address buy_gem = address(offers[id].buy_gem);
         address pay_gem = address(offers[id].pay_gem);
-        require(_span[pay_gem][buy_gem] > 0);
+        mapping(address => uint256) storage _spanOffer = _span[pay_gem];
+        require(_spanOffer[buy_gem] > 0);

         require(
             _rank[id].delb == 0 && //assert id is in the sorted list
@@ -1429,7 +1430,7 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
             _rank[_rank[id].prev].next = _rank[id].next;
         }

-        _span[pay_gem][buy_gem]--;
+        _spanOffer[buy_gem]--;
         _rank[id].delb = block.number; //mark _rank[id] for deletion
         return true;
     }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L168-L185

### Cache storage pointer for `tokenRewards[address(rewardsToken)]`

*Gas Savings for `BathHouseV2.claimRewards`, obtained via protocol's tests: Avg 73 gas*

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  265601 |    1     |
| After  |  -  |  -  |  265528 |    1     |

```solidity
File: contracts/periphery/BathBuddy.sol
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
179:        uint256 reward = tokenRewards[address(rewardsToken)][holderRecipient];
180:        if (reward > 0) {
181:            tokenRewards[address(rewardsToken)][holderRecipient] = 0;
182:            rewardsToken.safeTransfer(holderRecipient, reward);
183:            emit RewardPaid(holderRecipient, reward);
184:        }
185:    }
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..ab0d8d7 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -175,10 +175,11 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         whenNotPaused
         updateReward(holderRecipient, address(rewardsToken))
         onlyBathHouse
-    {
-        uint256 reward = tokenRewards[address(rewardsToken)][holderRecipient];
+    {
+        mapping(address => uint256) storage _tokenRewards = tokenRewards[address(rewardsToken)];
+        uint256 reward = _tokenRewards[holderRecipient];
         if (reward > 0) {
-            tokenRewards[address(rewardsToken)][holderRecipient] = 0;
+            _tokenRewards[holderRecipient] = 0;
             rewardsToken.safeTransfer(holderRecipient, reward);
             emit RewardPaid(holderRecipient, reward);
         }
```

## Use assembly for value transfer and success check
If we are not using the return data from a low level `.call` we can avoid creating extra stack variables and storing return data into memory by using assembly to access the `CALL` opcode and specify `0` for both the return data offset and the return data size.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L118-L119

```solidity
File: contracts/utilities/FeeWrapper.sol
118:        (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
119:		require(OK, "ETH transfer failed");
```
```diff
diff --git a/contracts/utilities/FeeWrapper.sol b/contracts/utilities/FeeWrapper.sol
index 88d6cfe..173e2ee 100644
--- a/contracts/utilities/FeeWrapper.sol
+++ b/contracts/utilities/FeeWrapper.sol
@@ -115,8 +115,13 @@ contract FeeWrapper {
         require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");

         // transfer fee to the 3rd party protocol
-        (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
-               require(OK, "ETH transfer failed");
+        assembly {
+            let OK := call(gas(), _feeTo, _feeAmount, 0, 0, 0, 0)
+            if iszero(OK) {
+                revert(0, 0)
+            }
+        }
         _msgValue = msg.value - _feeAmount;
     }
 }
```

## `GasReport` output, with all optimizations applied

```js
·----------------------------------------------------------------------------|---------------------------|-----------|-----------------------------·
|                            Solc version: 0.8.17                            ·  Optimizer enabled: true  ·  Runs: 5  ·  Block limit: 30000000 gas  │
·············································································|···························|···········|······························
|  Methods                                                                                                                                         │
···············································|·····························|·············|·············|···········|···············|··············
|  Contract                                    ·  Method                     ·  Min        ·  Max        ·  Avg      ·  # calls      ·  usd (avg)  │
···············································|·····························|·············|·············|···········|···············|··············
|  BathBuddy                                   ·  notifyRewardAmount         ·          -  ·          -  ·    68064  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  BathBuddy                                   ·  setRewardsDuration         ·          -  ·          -  ·    47792  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  BathHouseV2                                 ·  claimRewards               ·          -  ·          -  ·   240252  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  BathHouseV2                                 ·  createBathToken            ·    2847576  ·    2852863  ·  2848916  ·            4  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  BathHouseV2                                 ·  initialize                 ·          -  ·          -  ·    88830  ·            3  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  BathTokenV1                                 ·  approve                    ·          -  ·          -  ·    46243  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  BathTokenV1                                 ·  deposit                    ·          -  ·          -  ·   150158  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  BathTokenV1                                 ·  initialize                 ·          -  ·          -  ·   261169  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  CErc20Delegator                             ·  approve                    ·          -  ·          -  ·    52284  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  CErc20Delegator                             ·  mint                       ·     206751  ·     208256  ·   207151  ·            4  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  CErc20Delegator                             ·  redeem                     ·          -  ·          -  ·   120788  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Comptroller                                 ·  _become                    ·          -  ·          -  ·    53491  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Comptroller                                 ·  _setCollateralFactor       ·      59507  ·      64767  ·    60822  ·            4  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Comptroller                                 ·  _setCompSpeeds             ·          -  ·          -  ·    95742  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Comptroller                                 ·  _setPriceOracle            ·      48079  ·      53333  ·    49830  ·            3  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Comptroller                                 ·  _supportMarket             ·     132639  ·     152437  ·   143200  ·            9  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Comptroller                                 ·  setCompAddress             ·          -  ·          -  ·    47048  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  contracts/RubiconMarket.sol:ExpiringMarket  ·  cancel                     ·          -  ·          -  ·    86489  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  DummyPriceOracle                            ·  addCtoken                  ·      44139  ·      44187  ·    44154  ·            4  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  MarketAidFactory                            ·  createMarketAidInstance    ·          -  ·          -  ·  3736491  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  MarketAidFactory                            ·  initialize                 ·          -  ·          -  ·    90691  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  PoolsUtility                                ·  createPosition             ·          -  ·          -  ·  2559470  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  PoolsUtility                                ·  initialize                 ·          -  ·          -  ·   111506  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Position                                    ·  buyAllAmountWithLeverage   ·     638441  ·    1261701  ·   957229  ·           10  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Position                                    ·  closePosition              ·     461338  ·     505136  ·   490680  ·            6  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Position                                    ·  increaseMargin             ·     195272  ·     195308  ·   195290  ·            2  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Position                                    ·  sellAllAmountWithLeverage  ·     638641  ·    1261870  ·   887977  ·            7  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconMarket                               ·  buy                        ·     125314  ·     157811  ·   139691  ·            3  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconMarket                               ·  initialize                 ·      73300  ·      73516  ·    73440  ·            3  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconMarket                               ·  offer                      ·     255135  ·     267221  ·   261178  ·            2  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconMarket                               ·  offer                      ·     109760  ·     279301  ·   251681  ·           28  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconMarket                               ·  offer                      ·     189025  ·     193837  ·   191431  ·            2  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconMarket                               ·  sellAllAmount              ·          -  ·          -  ·   142506  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconMarket                               ·  setFeeBPS                  ·          -  ·          -  ·    29196  ·            2  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconMarket                               ·  setMakerFee                ·          -  ·          -  ·    46800  ·            9  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconMarketV1                             ·  initialize                 ·          -  ·          -  ·    98458  ·            3  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconRouter                               ·  buyAllAmountForETH         ·          -  ·          -  ·   201348  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconRouter                               ·  buyAllAmountWithETH        ·          -  ·          -  ·   220047  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconRouter                               ·  cancelForETH               ·          -  ·          -  ·   152143  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconRouter                               ·  depositWithETH             ·          -  ·          -  ·   316622  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconRouter                               ·  offerForETH                ·          -  ·          -  ·   331577  ·            2  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconRouter                               ·  offerWithETH               ·          -  ·          -  ·   350246  ·            2  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconRouter                               ·  startErUp                  ·      67022  ·      67034  ·    67028  ·            2  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconRouter                               ·  swapForETH                 ·          -  ·          -  ·   193783  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconRouter                               ·  swapWithETH                ·          -  ·          -  ·   203703  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  RubiconRouter                               ·  withdrawForETH             ·          -  ·          -  ·   218995  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Test3rdPartyProtocol                        ·  executeDepositWithETH      ·          -  ·          -  ·   383362  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Test3rdPartyProtocol                        ·  executeOfferWithETH        ·          -  ·          -  ·   399228  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Test3rdPartyProtocol                        ·  executeSwap                ·          -  ·          -  ·   289584  ·            2  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Test3rdPartyProtocol                        ·  executeSwapWithETH         ·          -  ·          -  ·   295521  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  TokenWithFaucet                             ·  approve                    ·      29161  ·      46261  ·    44107  ·           24  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  TokenWithFaucet                             ·  faucet                     ·      58647  ·      75747  ·    63533  ·           14  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  TokenWithFaucet                             ·  transfer                   ·      46881  ·      51633  ·    49257  ·            2  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  TransparentUpgradeableProxy                 ·  upgradeTo                  ·          -  ·          -  ·    33078  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  Unitroller                                  ·  _setPendingImplementation  ·          -  ·          -  ·    47596  ·            1  ·          -  │
···············································|·····························|·············|·············|···········|···············|··············
|  V2Migrator                                  ·  migrate                    ·          -  ·          -  ·   374696  ·            1  ·          -  │
```