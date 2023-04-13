## Summary

### Gas Optimizations
| |Issue|Instances| |
|-|:-|:-:|:-:|
| [G&#x2011;01] | Avoid using state variable in emit | 2 |
| [G&#x2011;02] | Using Private for constant will save gas | 3 |
| [G&#x2011;03] | DSMath some functions should not be used, will result in saving some gas | 1 |
| [G&#x2011;04] | Cheaper input valdiations should come before expensive operations | 4 |
| [G&#x2011;05] | Functions guaranteed to revert when called by normal users can be marked payable | 13 |
| [G&#x2011;06] | Splitting require() statements that use && saves gas | 4 |
| [G&#x2011;07] | Avoid compound assignment operator in state variables | 5 |
| [G&#x2011;08] | Use nested if and, avoid multiple check combinations | 5 |
| [G&#x2011;09] | Save gas with the use of the import statement | 25 |
| [G&#x2011;10] | Setting the constructor to payable | 2 |
| [G&#x2011;11] | Upgrade Solidity’s optimizer | 1 |
| [G&#x2011;12] | A modifier used only once and not being inherited should be inlined to save gas | 2 |
| [G&#x2011;13] | Using immutable on variables that are only set in the constructor and never after (2.1k gas per var) | 4 |
| [G&#x2011;14] | struct Order: can be more tighly packed | 3 |
| [G&#x2011;15] | State variables can be packed into fewer storage slots | 1 |


### [G&#x2011;01]  Avoid using state variable in emit 
While emitting events, Passing parameter variables will cost less as compared to passing the storage values.
There are 2 instances of this issues.

```solidity
File: contracts/RubiconMarket.sol

25    function setOwner(address owner_) external auth {
26        owner = owner_;
27        emit LogSetOwner(owner);
28    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28)

### Recommended Mitigation steps
By incorporating below recommendation, It can be saved 130 gas approximately checked as per Remix IDE.

```solidity
File: contracts/RubiconMarket.sol

    function setOwner(address owner_) external auth {
+       emit LogSetOwner(owner_);
        owner = owner_;
    }
```

```solidity
File: contracts/periphery/BathBuddy.sol

232    function setRewardsDuration(
233        uint256 _rewardsDuration,
234        address token
235    ) external onlyOwner {
236        require(
237            block.timestamp > periodFinish[token],
238            "Previous rewards period must be complete before changing the duration for the new period"
239        );
240        rewardsDuration[token] = _rewardsDuration;
241        emit RewardsDurationUpdated(rewardsDuration[token]);
242    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L232-L242)

### Recommended Mitigation steps
By incorporating below recommendation, It can be saved 130 gas approximately checked as per Remix IDE.

```solidity
File: contracts/RubiconMarket.sol

    function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner {
        require(
            block.timestamp > periodFinish[token],
            "Previous rewards period must be complete before changing the duration for the new period"
        );
+       emit RewardsDurationUpdated(_rewardsDuration);
        rewardsDuration[token] = _rewardsDuration;
-        emit RewardsDurationUpdated(rewardsDuration[token]);
    }
```

### [G&#x2011;02]  Using Private for constant will save gas
The default visibility of state variables is internal. In this case the constant variables have internal default visibilty, however by using Private visibility some gas can be saved for constant variables. If needed, the value can be read from the verified contract source code.
There are 3 instances of this issue.

```solidity
File: contracts/RubiconMarket.sol

74    uint256 constant WAD = 10 ** 18;
75    uint256 constant RAY = 10 ** 27;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74-L75)

```solidity
File: contracts/RubiconMarket.sol

232    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L232)

### Recommended Mitigation steps
```solidity
File: contracts/RubiconMarket.sol

+    uint256 private constant WAD = 10 ** 18;
+    uint256 private constant RAY = 10 ** 27;
```

```solidity
File: contracts/RubiconMarket.sol

+    bytes32 private constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```

### [G&#x2011;03]  DSMath some functions should not be used, will result in saving some gas
The DSMath contract library has functions like sum(), sub(), mul(), div(), etc have been extensively used in arithmatic operations. Since the contract solidity compiler version is ^0.8.9, Solidity 0.8 includes checked arithmetic operations by default, and this renders these SafeMath functions unnecessary. It's also true that using these SafeMath function calls will be more expensive than just built in arithmetic. 

### Proof of concept

[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L45-L1317)

### Recommended Mitigation steps
It's better to avoid using DSMath contract above listed SafeMath functions for uint256 for solidity >0.8.0.  Solidity ^0.8.0 has already taken care of underflow and overflow conditions, which is inbuilt in solidity ^0.8.0.
"
[Solidity changelog Reference](https://docs.soliditylang.org/en/v0.8.19/080-breaking-changes.html)
[openzeppelin Reference](https://github.com/OpenZeppelin/openzeppelin-contracts/issues/2465)

### [G&#x2011;04]  Cheaper input valdiations should come before expensive operations
input parameter validations should be come at top instead of between of functions. This will validate the input on top itself and will ultimately save the gas to end users.
There are 2 instances of this issue.

### Proof of concept
```solidity
File: contracts/RubiconMarket.sol

511    function offer(
512        uint256 pay_amt,
513        ERC20 pay_gem,
514        uint256 buy_amt,
515        ERC20 buy_gem,
516        address owner,
517        address recipient
518    ) public virtual can_offer synchronized returns (uint256 id) {
519        require(uint128(pay_amt) == pay_amt);
520        require(uint128(buy_amt) == buy_amt);
521        require(pay_amt > 0);
522        require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) 
       which compiles in 0.7.6
523        require(buy_amt > 0);
524        require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) 
       which compiles in 0.7.6
525        require(pay_gem != buy_gem);
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L511-L525)

### Recommended Mitigation steps
```solidity
File: contracts/RubiconMarket.sol

    function offer(
        uint256 pay_amt,
        ERC20 pay_gem,
        uint256 buy_amt,
        ERC20 buy_gem,
        address owner,
       address recipient
    ) public virtual can_offer synchronized returns (uint256 id) {
+          require(pay_amt > 0);
+          require(buy_amt > 0);
           require(uint128(pay_amt) == pay_amt);
           require(uint128(buy_amt) == buy_amt);
-          require(pay_amt > 0);
           require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) 
       which compiles in 0.7.6
-          require(buy_amt > 0);
           require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) 
       which compiles in 0.7.6
           require(pay_gem != buy_gem);
```

### [G&#x2011;05]  Functions guaranteed to revert when called by normal users can be marked payable
If a function modifier such as onlyOwner/auth is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

There are 13 instances of this issue.

```solidity
File: contracts/RubiconMarket.sol

25    function setOwner(address owner_) external auth {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25)

```solidity
File: contracts/RubiconMarket.sol

628    function stop() external auth {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L628)

```solidity
File: contracts/RubiconMarket.sol

955    function setMinSell(
956        ERC20 pay_gem, //token to assign minimum sell amount to
957        uint256 dust //maker (ask) minimum sell amount
958    ) external auth note returns (bool) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L955-L958)

```solidity
File: contracts/RubiconMarket.sol

1466    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
---

1471    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
---

1476    function setFeeTo(address newFeeTo) external auth returns (bool) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1476)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

98    ) external onlyOwner {
---

112    ) external onlyOwner {
---

210    function closePosition(uint256 posId) external onlyOwner {
---

226    function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {
---

242    function withdraw(address token, uint256 amount) external onlyOwner {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L98-L242)

```solidity
File: contracts/periphery/BathBuddy.sol

194    ) external onlyOwner updateReward(address(0), address(rewardsToken)) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L194)

```solidity
File: contracts/periphery/BathBuddy.sol

235    ) external onlyOwner {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L235)

### [G&#x2011;06]  Splitting require() statements that use && saves gas
Instead of using the && operator in a single require statement to check multiple conditions, I suggest using multiple require statements with 1 condition per require statement (saving 3 gas per "&").
Check  [this reference](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper.
There are 4 instances of this issue:

```solidity
File: contracts/RubiconMarket.sol

893        require(
894            payAmts.length == payGems.length &&
895                payAmts.length == buyAmts.length &&
896                payAmts.length == buyGems.length,
897            "Array lengths do not match"
898        );
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L893-L898)

```solidity
File: contracts/RubiconMarket.sol

940        require(
941            !isActive(id) &&
942                _rank[id].delb != 0 &&
943                _rank[id].delb < block.number - 10
944        );
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L940-L944)

```solidity
File: contracts/RubiconMarket.sol

1412        require(
1413            _rank[id].delb == 0 && //assert id is in the sorted list
1414                isOfferSorted(id)
1415        );
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1412-L1415)

```solidity
File: contracts/periphery/BathBuddy.sol

95        require(
96            msg.sender == myBathTokenBuddy &&
97                msg.sender != address(0) &&
98                friendshipStarted,
99            "You are not my buddy!"
100        );
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L95-L100)

### [G&#x2011;07]  Avoid compound assignment operator in state variables
Using compound assignment operators for state variables (like State += X or State -= X …) it’s more expensive than using operator assignment (like State = State + X or State = State - X …).
There are 5 instance of this issue.

```solidity
File: contracts/RubiconMarket.sol

583        _amount -= mul(amount, feeBPS) / 100_000;
---

586        _amount -= mul(amount, makerFee()) / 100_000;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L583-L586)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

184            vars.currentBathTokenAmount += _borrowLoop(
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L184)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

431        positions[_positionId].bathTokenAmount += _bathTokenAmount;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L431)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

560            _assetAmount += _loopBorrowed;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L560)

### [G&#x2011;08]  Use nested if and, avoid multiple check combinations
Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.
There are 5 instances of this issue.

```solidity
File: contracts/RubiconMarket.sol

349            if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L349)

```solidity
File: contracts/RubiconMarket.sol

1197        if (
1198            isActive(id) &&
1199            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]
1200        ) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1197-L1200)

```solidity
File: contracts/RubiconMarket.sol

1324        if (
1325            t_buy_amt > 0 &&
1326            t_pay_amt > 0 &&
1327            t_pay_amt >= _dust[address(t_pay_gem)]
1328        ) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1324-L1328)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

176            if (i.add(1) == vars.limit && vars.lastBorrow != 0) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L176)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

391        if (
392            IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
393            borrowBalance(_bathTokenAsset) == 0
394        ) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L391-L394)

### Recommended Mitigation steps
Recommending below code as an example that can also be followed for other instances.

```solidity
File: contracts/utilities/poolsUtility/Position.sol

-        if (
-            IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
-            borrowBalance(_bathTokenAsset) == 0
-        ) {

+        if (IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 ){
+            if (borrowBalance(_bathTokenAsset) == 0 ) {
+         }
+         }
+         { ...}
```
### [G&#x2011;09]  Save gas with the use of the import statement
With the import statement, it saves gas to specifically import only the parts of the contracts, not the complete ones.
There are 25 instances of this issue, including all smart contracts in scope. 

### Description:
Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct polluted the source code with an unnecessary object we were not using because we did not need it.

This was breaking the rule of modularity and modular programming: only import what you need Specific imports with curly braces allow us to apply this rule better.

Prefer import declarations that specify the symbol(s) using the form import {SYMBOL} from "SomeContract.sol" rather than importing the whole file.

### Recommendation:
For example, 
```solidity
import {contract1 , contract2} from "filename.sol";

A good example from the ArtGobblers project;

import {Owned} from “solmate/auth/Owned.sol”;
import {ERC721} from “solmate/tokens/ERC721.sol”;
import {LibString} from “solmate/utils/LibString.sol”;
import {MerkleProofLib} from “solmate/utils/MerkleProofLib.sol”;
import {FixedPointMathLib} from “solmate/utils/FixedPointMathLib.sol”;
import {ERC1155, ERC1155TokenReceiver} from “solmate/tokens/ERC1155.sol”;
import {toWadUnsafe, toDaysWadUnsafe} from “solmate/utils/SignedWadMath.sol”;
```

### [G&#x2011;10] Setting the constructor to payable
You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. The following opcodes are cut out:
CALLVALUE, DUP1, ISZERO, PUSH2, JUMPI, PUSH1, DUP1, REVERT, JUMPDEST, POP
Making the constructor payable eliminates the need for an initial check of msg.value == 0 and saves 13 gas on deployment with no security risks.
There are 2 instances of this issue.

```solidity
File: contracts/V2Migrator.sol

30    constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

54    constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L54)

### [G&#x2011;11] Upgrade Solidity’s optimizer
Make sure Solidity’s optimizer is enabled. It reduces gas costs. If you want to gas optimize for contract deployment (costs less to deploy a contract) then set the Solidity optimizer at a low number. If you want to optimize for run-time gas costs (when functions are called on a contract) then set the optimizer to a high number.

```solidity
File: hardhat.config.ts

10 const config: HardhatUserConfig = {
11   solidity: {
12    compilers: [
13      {
14        version: "0.8.17",
15        settings: {
16          optimizer: {
17            enabled: true,
18            runs: 5,
19          },
20        },
21      },
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/hardhat.config.ts#L10-L21)

### [G&#x2011;12] A modifier used only once and not being inherited should be inlined to save gas
There are 2 instances of this issue.

In BathBuddy.sol, onlyBathHouse modifier is used only once and it can be directly used in function to save some gas.

```solidity
File: contracts/periphery/BathBuddy.sol

104    modifier onlyBathHouse() {
105        require(msg.sender == bathHouse, "You are not my beloved bath house!");
106        _;
107    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L104-L107)

The above modifer is only used once in the following:

```solidity
File: contracts/periphery/BathBuddy.sol

168    function getReward(
169        IERC20 rewardsToken,
170        address holderRecipient
171    )
172        external
173        override
174        nonReentrant
175        whenNotPaused
176        updateReward(holderRecipient, address(rewardsToken))
177        onlyBathHouse
178    {
179        uint256 reward = tokenRewards[address(rewardsToken)][holderRecipient];
180        if (reward > 0) {
181            tokenRewards[address(rewardsToken)][holderRecipient] = 0;
182            rewardsToken.safeTransfer(holderRecipient, reward);
183            emit RewardPaid(holderRecipient, reward);
184        }
185    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L168-L185)

### Recommended Mitigation steps
```solidity
diff --git a/BathBuddy.sol.orig b/BathBuddy.sol
index 4aac344..b664a4e 100644
--- a/BathBuddy.sol.orig
+++ b/BathBuddy.sol
@@ -101,11 +101,6 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         _;
     }

-    modifier onlyBathHouse() {
-        require(msg.sender == bathHouse, "You are not my beloved bath house!");
-        _;
-    }
-
     /* ========== VIEWS ========== */
     // BATH TOKEN DOES ALL SHARE ACCOUNTING! CAREFUL!

@@ -174,8 +169,8 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         nonReentrant
         whenNotPaused
         updateReward(holderRecipient, address(rewardsToken))
-        onlyBathHouse
     {
+        require(msg.sender == bathHouse, "You are not my beloved bath house!");
         uint256 reward = tokenRewards[address(rewardsToken)][holderRecipient];
         if (reward > 0) {
             tokenRewards[address(rewardsToken)][holderRecipient] = 0;
```

In BathBuddy.sol, onlyBathHouse modifier is used only once and it can be directly used in function to save some gas.

```solidity
File: contracts/periphery/BathBuddy.sol

26    modifier onlyAdmin() {
27        require(msg.sender == admin, "onlyAdmin: !admin");
28        _;
29    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L26-L29)

The above modifer is only used once in the following:

```solidity
File: contracts/periphery/BathBuddy.sol

60    function createBathToken(
61        address underlying,
62        InterestRateModel interestRateModel,
63        uint256 initialExchangeRateMantissa,
64        address implementation,
65        bytes memory becomeImplementationData
66    ) external onlyAdmin {
67        // underlying can be used only for one bathToken
68        require(
69            tokenToBathToken[underlying] == address(0),
70            "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
71        );
             ----
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L60-L71)

### Recommended Mitigation steps

```solidity
diff --git a/BathHouseV2.sol.orig b/BathHouseV2.sol
index 5ff6d4c..c30c127 100644
--- a/BathHouseV2.sol.orig
+++ b/BathHouseV2.sol
@@ -23,11 +23,6 @@ contract BathHouseV2 {
     event BathTokenCreated(address bathToken, address underlying);
     event BuddySpawned(address bathToken, address bathBuddy);

-    modifier onlyAdmin() {
-        require(msg.sender == admin, "onlyAdmin: !admin");
-        _;
-    }
-
     // proxy-constructor
     function initialize(address _comptroller, address _pAdmin) external {
         require(!initialized, "BathHouseV2 already initialized!");
@@ -63,7 +58,8 @@ contract BathHouseV2 {
         uint256 initialExchangeRateMantissa,
         address implementation,
         bytes memory becomeImplementationData
-    ) external onlyAdmin {
+    ) external {
+        require(msg.sender == admin, "onlyAdmin: !admin");
         // underlying can be used only for one bathToken
         require(
             tokenToBathToken[underlying] == address(0),
```

### [G&#x2011;13] Using immutable on variables that are only set in the constructor and never after (2.1k gas per var)
Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving SLOAD.
Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around 20 000 gas per variable) and replace the expensive storage-reading operations (around 2100 gas per reading) to a less expensive value reading (3 gas).
There are 4 instances of this issue.

Total instances: 4 gas savings 4 * 2.1k = 8.4k gas

```solidity
File: contracts/utilities/poolsUtility/Position.sol

44    Comptroller public comptroller;
45    PriceOracle public oracle;
46    RubiconMarket public rubiconMarket;
47    BathHouseV2 public bathHouseV2;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L44-L47)

### [G&#x2011;14] struct Order: can be more tighly packed
Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings.
There are 3 instances of this issue.

In FeeWrapper.sol, the struct variables can be tightly packed to save some slots

```solidity
File: contracts/utilities/FeeWrapper.sol

12    struct CallParams {
13        bytes4 selector; // function selector
14        bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
15        address target; // target contract to call
16        FeeParams feeParams;
17   }
18
19    struct FeeParams {
20        address feeToken; // token in the form of which fee paid
21        uint256 totalAmount; // amount without deducted fee
22        uint256 feeAmount; // amount of feeToken from which fee should be charged
23        address feeTo; // receiver of the fee
24    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L12-L24)

### Recommended Mitigation steps
```solidity
    struct CallParams {
-       bytes4 selector; // function selector
        bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
+       bytes4 selector; // function selector
        address target; // target contract to call
        FeeParams feeParams;
    }

    struct FeeParams {
-       address feeToken; // token in the form of which fee paid
        uint256 totalAmount; // amount without deducted fee
        uint256 feeAmount; // amount of feeToken from which fee should be charged
+       address feeToken; // token in the form of which fee paid
        address feeTo; // receiver of the fee
    }
```

In Position.sol, the struct variables can be tightly packed to save some slots

```solidity
File: contracts/utilities/poolsUtility/Position.sol

33    struct Position {
34        address asset; // supplied as collateral
35        address quote; // borrowed token
36        uint256 borrowedAmount; // amount of borrowed quote
37        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
38        uint256 blockNum; // block number on which position was opened
39        bool isActive; // false by default, when active - true
40    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L33-L40)

### Recommended Mitigation steps

```solidity
File: contracts/utilities/poolsUtility/Position.sol

    struct Position {
        address asset; // supplied as collateral
        address quote; // borrowed token
+       bool isActive; // false by default, when active - true
        uint256 borrowedAmount; // amount of borrowed quote
        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
        uint256 blockNum; // block number on which position was opened
-       bool isActive; // false by default, when active - true
    }
```

### [G&#x2011;15] State variables can be packed into fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if the values combined are <= 32 bytes).
If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables can also be cheaper.

There is 1 instance of this issue:

```solidity
File: contracts/RubiconMarket.sol

219    uint256 public last_offer_id;
220
221    /// @dev The mapping that makes up the core orderbook of the exchange
222    mapping(uint256 => OfferInfo) public offers;
223
224    bool locked;
225
226    /// @dev This parameter is in basis points
227    uint256 internal feeBPS;
228
229    /// @dev This parameter provides the address to which fees are sent
230    address internal feeTo;
231
232    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L219-L232)

### Recommended Mitigation steps

```solidity
diff --git a/RubiconMarket.sol.orig b/RubiconMarket.sol
index 219e915..50c1e02 100644
--- a/RubiconMarket.sol.orig
+++ b/RubiconMarket.sol
@@ -221,14 +221,14 @@ contract SimpleMarket is EventfulMarket, DSMath {
     /// @dev The mapping that makes up the core orderbook of the exchange
     mapping(uint256 => OfferInfo) public offers;

-    bool locked;

     /// @dev This parameter is in basis points
     uint256 internal feeBPS;

     /// @dev This parameter provides the address to which fees are sent
     address internal feeTo;

+    bool locked;

     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```
