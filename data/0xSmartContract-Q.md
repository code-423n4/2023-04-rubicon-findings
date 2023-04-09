## Summary
### Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
|[L-01]|Danger "while" loop | 1 |
|[L-02]|Prevent division by `0`  | 1 |
|[L-03]|Function Calls in Loop Could Lead to Denial of Service due to Array length not being checked | 3 |
|[L-04]|Use ```safeTransferOwnership``` instead of ```transferOwnership``` function|1|
|[L-05]|Missing Event for  initialize| 1 |
|[L-06]|Use Fuzzing Test for math code bases  | All Contracts |
|[L-07]|Omissions in Events| 6 |
|[L-08]|Set an upper limit on the determination of fee | 1 |
|[L-09]|Use the latest updated version of OpenZeppelin dependencies| 1 |
|[L-10]|Project Upgrade and Stop Scenario should be| All Contracts |
|[L-11] |Insufficient coverage|All Contracts|
|[L-12] |Add a timelock to critical functions| 1|
|[L-13] |` RewardPaid ` event is missing parameters| 1|
|[L-14] |Keccak Constant values should used to immutable rather than constant|1|
|[N-15] |Constants on the left are better| 27 |
|[N-16] |NatSpec comments should be increased in contracts| All Contracts |
|[N-17] |`Function writing` that does not comply with the `Solidity Style Guide`| All Contracts |
|[N-18] |Include return parameters in NatSpec comments | All Contracts |
|[N-19] |Tokens accidentally sent to the contract cannot be recovered| 1 |
|[N-20] |Assembly Codes Specific – Should Have Comments | 2|
|[N-21] |For modern and more readable code; update import usages| 26 |
|[N-22] |Use a more recent version of Solidity | 2 |
|[N-23] |For functions, follow Solidity standard naming conventions (internal function style rule)| 16 |
|[N-24] |Floating pragma| 2 |
|[N-25] |Use SMTChecker| All Contract |
|[N-26] |Lines are too long| 3 |
|[N-27] |Use `uint256` instead `uint`| 4 |
|[N-28] |Avoid _shadowing_ `inherited state variables` | 1 |
|[N-29] |Remove the codes used in the test area in the actual codes | 1 |
|[N-30] |Include proxy contracts to Audit| 2 |
|[N-31] |Remove NatSpec comments during testing and design| 4 |
|[N-32] |In 0.8>= pragma versions, the use of SafeMath is unnecessary|1 |
|[N-33] |Use the `delete` keyword instead of assigning a value of `0`| 1 |

Total 33 issues



## [L-01] Danger "while" loop

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L541

## Impact
` Position.sol#L541` has a while loop on this line .
When using `while` loops, the Solidity compiler is aware that a condition needs to be checked first, before executing the code inside the loop.

Firstly, the conditional part is evaluated

a) if the condition evaluates to true , the instructions inside the loop (defined inside the brackets { ... }) get executed.

b) once the code reaches the end of the loop body (= the closing curly brace } , it will jump back up to the conditional part).

c) the code inside the while loop body will keep executing over and over again until the conditional part turns false.

For loops are more associated with iterations. While loop instead are more associated with repetitive tasks. Such tasks could potentially be infinite if the while loop body never affects the condition initially checked.
Therefore, they should be used with care.


At very small intervals, a large number of loops can be accessed by someone attempting a DOS attack.


## Tools Used
Manual code review

## Recommended Mitigation Steps
Determine a reasonable minimum range `_assetAmount <= _desiredAmount`

### [L-02] Prevent division by `0`

On several locations in the code precautions are not being taken for not dividing by 0, this will revert the code.
These functions can be calledd with 0 value in the input, this value is not checked for being bigger than 0, that means in some scenarios this can potentially trigger a division by zero.

`totalWeight` value should be check for 0 value

```solidity

1 result - 1 file

contracts/RubiconMarket.sol:
  1272      //match offers with taker offer, and execute token transactions
  1273:     function _matcho(
  1274:         uint256 t_pay_amt, //taker sell how much
  1275:         ERC20 t_pay_gem, //taker sell which token
  1276:         uint256 t_buy_amt, //taker buy how much
  1277:         ERC20 t_buy_gem, //taker buy which token
  1278:         uint256 pos, //position id
  1279:         bool rounding, //match "close enough" orders?
  1280:         address owner,
  1281:         address recipient
  1282:     ) internal returns (uint256 id) {
  1283:         uint256 best_maker_id; //highest maker id
  1284:         uint256 t_buy_amt_old; //taker buy how much saved
  1285:         uint256 m_buy_amt; //maker offer wants to buy this much token
  1286:         uint256 m_pay_amt; //maker offer wants to sell this much token
  1287: 
  1288:         // there is at least one offer stored for token pair
  1289:         while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {
  1290:             best_maker_id = _best[address(t_buy_gem)][address(t_pay_gem)];
  1291:             m_buy_amt = offers[best_maker_id].buy_amt;
  1292:             m_pay_amt = offers[best_maker_id].pay_amt;
  1293: 
  1294:             // Ugly hack to work around rounding errors. Based on the idea that
  1295:             // the furthest the amounts can stray from their "true" values is 1.
  1296:             // Ergo the worst case has t_pay_amt and m_pay_amt at +1 away from
  1297:             // their "correct" values and m_buy_amt and t_buy_amt at -1.
  1298:             // Since (c - 1) * (d - 1) > (a + 1) * (b + 1) is equivalent to
  1299:             // c * d > a * b + a + b + c + d, we write...
  1300:             if (
  1301:                 mul(m_buy_amt, t_buy_amt) >
  1302:                 mul(t_pay_amt, m_pay_amt) +
  1303:                     (
  1304:                         rounding
  1305:                             ? m_buy_amt + t_buy_amt + t_pay_amt + m_pay_amt
  1306:                             : 0
  1307:                     )
  1308:             ) {
  1309:                 break;
  1310:             }
  1311:             // ^ The `rounding` parameter is a compromise borne of a couple days
  1312:             // of discussion.
  1313:             buy(best_maker_id, min(m_pay_amt, t_buy_amt));
  1314:             emit LogMatch(id, min(m_pay_amt, t_buy_amt));
  1315:             t_buy_amt_old = t_buy_amt;
  1316:             t_buy_amt = sub(t_buy_amt, min(m_pay_amt, t_buy_amt));
  1317:             t_pay_amt = mul(t_buy_amt, t_pay_amt) / t_buy_amt_old; // @audit  `t_buy_amt_old` can be division by `0`

  1318: 
  1319:             if (t_pay_amt == 0 || t_buy_amt == 0) {
  1320:                 break;
  1321:             }
  1322:         }
  1323: 
  1324:         if (
  1325:             t_buy_amt > 0 &&
  1326:             t_pay_amt > 0 &&
  1327:             t_pay_amt >= _dust[address(t_pay_gem)]
  1328:         ) {
  1329:             //new offer should be created
  1330:             id = super.offer(
  1331:                 t_pay_amt,
  1332:                 t_pay_gem,
  1333:                 t_buy_amt,
  1334:                 t_buy_gem,
  1335:                 owner,
  1336:                 recipient
  1337:             );
  1338:             //insert offer into the sorted list
  1339:             _sort(id, pos);
  1340:         }
  1341:     }
```

### [L-03] Function Calls in Loop Could Lead to Denial of Service due to Array length not being checked


```diff

contracts/RubiconMarket.sol:
  886      /// @notice Batch offer functionality - multuple offers in a single transaction
  887:     function batchOffer(
  888:         uint[] calldata payAmts,
  889:         address[] calldata payGems,
  890:         uint[] calldata buyAmts,
  891:         address[] calldata buyGems
  892:     ) external {
  893:         require(
  894:             payAmts.length == payGems.length &&
  895:                 payAmts.length == buyAmts.length &&
  896:                 payAmts.length == buyGems.length,
  897:             "Array lengths do not match"
  898:         );
+      if payAmts.length > maxArrayLength) {
+          revert maxArrayLengt();
+       }
  899:         for (uint i = 0; i < payAmts.length; i++) {
  900:             this.offer(
  901:                 payAmts[i],
  902:                 ERC20(payGems[i]),
  903:                 buyAmts[i],
  904:                 ERC20(buyGems[i])
  905:             );
  906:         }
  907:     }



```

**Recommendation:**
Function calls made in unbounded loop are error-prone with potential resource exhaustion as it can trap the contract due to gas limitations or failed transactions. Consider bounding the loop length if the array is expected to be growing and/or handling a huge list of elements to avoid unnecessary gas wastage and denial of service.

### [L-04] Use ```safeTransferOwnership``` instead of ```transferOwnership``` function

**Context:**
contracts/utilities/poolsUtility/Position.sol:
  7: import "@openzeppelin/contracts/access/Ownable.sol";

**Description:**
```transferOwnership``` function is used to change Ownership from ```Ownable.sol```.

Use a 2 structure transferOwnership which is safer. 
```safeTransferOwnership```,  use it is more secure due to 2-stage ownership transfer.

**Recommendation:**
Use ``Ownable2Step.sol``
[Ownable2Step.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol)

### [L-05] Missing Event for  initialize

**Context:**
```solidity

contracts/BathHouseV2.sol:
  30  
  31:     // proxy-constructor
  32:     function initialize(address _comptroller, address _pAdmin) external {
  33:         require(!initialized, "BathHouseV2 already initialized!");
  34:         comptroller = Comptroller(_comptroller);
  35:         admin = msg.sender;
  36:         proxyAdmin = _pAdmin;
  37: 
  38:         initialized = true;
  39:     }



```

**Description:**
Events help non-contract tools to track changes, and events prevent users from being surprised by changes
Issuing event-emit during initialization is a detail that many projects skip

**Recommendation:**
Add Event-Emit

### L-06]  Use Fuzzing Test for math code bases 


I recommend fuzzing testing in math code structures,

**Recommendation:**

Use should fuzzing test like Echidna.


Fuzzing is not easy, the tools are rough, and the math is hard, but it is worth it. Fuzzing gives me a level of confidence in my smart contracts that I didn’t have before. Relying just on unit testing anymore and poking around in a testnet seems reckless now.



https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05

### [L-07] Omissions in Events

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters

The events should include the new value and old value where possible:

```solidity

6 results - 2 files

contracts/RubiconMarket.sol:
    24  
    25:     function setOwner(address owner_) external auth {
    26          owner = owner_;

   954      //    of tokens received.
   955:     function setMinSell(
   956          ERC20 pay_gem, //token to assign minimum sell amount to

  1465  
  1466:     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
  1467          feeBPS = _newFeeBPS;

  1470  
  1471:     function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
  1472          StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;

  1475  
  1476:     function setFeeTo(address newFeeTo) external auth returns (bool) {
  1477          require(newFeeTo != address(0));

contracts/periphery/BathBuddy.sol:
  231      // Must be used before? notifyRewardAmount to set the new period
  232:     function setRewardsDuration(
  233          uint256 _rewardsDuration,


```

### [L-08] Set an upper limit on the determination of fee.

Determining the fees is an important assignment. It is an issue that concerns users very closely, so adding an upper limit in the determination of fees both prevents false identifications and increases the confidence of users.

```solidity
contracts/RubiconMarket.sol:
  1465  
  1466:     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
  1467          feeBPS = _newFeeBPS;
```

### [L-09] Use the latest updated version of OpenZeppelin dependencies

```js

package.json:
  48:     "@openzeppelin/contracts": "^4.8.0",
```

### Proof Of Concept

https://github.com/OpenZeppelin/openzeppelin-contracts/releases/


### Recommended Mitigation Steps
Upgrade `OpenZeppelin` to version 4.8.2


### [L-10] Project Upgrade and Stop Scenario should be

At the start of the project, the system may need to be stopped or upgraded, I suggest you have a script beforehand and add it to the documentation.
This can also be called an " EMERGENCY STOP (CIRCUIT BREAKER) PATTERN ".

https://github.com/maxwoe/solidity_patterns/blob/master/security/EmergencyStop.sol


### [L-11] Insufficient coverage

**Description:**
The test coverage rate of the project is ~60%. Testing all functions is best practice in terms of security criteria.
Due to its capacity, test coverage is expected to be 100%.


```js
README.md:
  173: - What is the overall line coverage percentage provided by your tests?:  60%
```


Moreover ,  i can't check to coverage percentage when i use `forge coverage` ;

```js
forge coverage


| File                                                       | % Lines        | % Statements   | % Branches     | % Funcs       |
|------------------------------------------------------------|----------------|----------------|----------------|---------------|
| contracts/BathHouseV2.sol                                  | 0.00% (0/26)   | 0.00% (0/31)   | 0.00% (0/10)   | 0.00% (0/7)   |
| contracts/RubiconMarket.sol                                | 1.01% (3/298)  | 0.95% (3/315)  | 0.00% (0/162)  | 4.17% (3/72)  |
| contracts/V2Migrator.sol                                   | 0.00% (0/11)   | 0.00% (0/14)   | 0.00% (0/6)    | 0.00% (0/1)   |
| contracts/periphery/BathBuddy.sol                          | 0.00% (0/31)   | 0.00% (0/34)   | 0.00% (0/16)   | 0.00% (0/8)   |
| contracts/utilities/FeeWrapper.sol                         | 0.00% (0/30)   | 0.00% (0/36)   | 0.00% (0/10)   | 0.00% (0/6)   |
| contracts/utilities/poolsUtility/PoolsUtility.sol          | 0.00% (0/12)   | 0.00% (0/13)   | 0.00% (0/4)    | 0.00% (0/3)   |
| contracts/utilities/poolsUtility/Position.sol              | 0.00% (0/134)  | 0.00% (0/168)  | 0.00% (0/52)   | 0.00% (0/26)  |
| Total                                                      | 0.11% (3/2643) | 0.10% (3/3103) | 0.00% (0/1108) | 0.51% (3/590) |


```

### [L-12] Add a timelock to critical functions

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user).
Consider adding a timelock to:

```solidity

contracts/RubiconMarket.sol:
  1465  
  1466:     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
  1467:         feeBPS = _newFeeBPS;
  1468:         return true;
  1469:     }
```

### [L-13] ` RewardPaid ` event is missing parameters


The ` BathBuddy.sol` contract has very important function; ` getReward()` 


However, only amounts are published in emits, whereas msg.sender must be specified in every transaction, a contract or web page listening to events cannot react to users, emit does not serve the purpose.
Basically, this event cannot be used


```solidity
contracts/periphery/BathBuddy.sol:
  167      /// @param holderRecipient allows the BathToken to pass through correct reward amounts to callers of the function @ BATH TOKEN LEVEL
  168:     function getReward(
  169:         IERC20 rewardsToken,
  170:         address holderRecipient
  171:     )
  172:         external
  173:         override
  174:         nonReentrant
  175:         whenNotPaused
  176:         updateReward(holderRecipient, address(rewardsToken))
  177:         onlyBathHouse
  178:     {
  179:         uint256 reward = tokenRewards[address(rewardsToken)][holderRecipient];
  180:         if (reward > 0) {
  181:             tokenRewards[address(rewardsToken)][holderRecipient] = 0;
  182:             rewardsToken.safeTransfer(holderRecipient, reward);
  183:             emit RewardPaid(holderRecipient, reward);
  184:         }
  185:     }

```



### Recommended Mitigation Steps
add `msg.sender` parameter in event-emit

### [L-14] Keccak Constant values should used to immutable rather than constant

There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts.

While it doesn't save any gas because the compiler knows that developers often make this mistake, it's still best to use the right tool for the task at hand.



```solidity
1 result - 1 file

contracts/RubiconMarket.sol:
  232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```

### [N-15] Constants on the left are better

If you use the constant first you support structures that veil programming errors. And one should only produce code either to add functionality, fix an programming error or trying to support structures to avoid programming errors (like design patterns). 

https://www.moserware.com/2008/01/constants-on-left-are-better-but-this.html

```solidity
27 results - 5 files

contracts/RubiconMarket.sol:
    54      function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
    55:         require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");
    56      }

   325          if (
   326:             quantity == 0 ||
   327:             spend == 0 ||
   328              quantity > _offer.pay_amt ||

   435  
   436:         if (offers[id].pay_amt == 0) {
   437              delete offers[id];

  1079              offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair
  1080:             require(offerId != 0, "offerId == 0");
  1081  

  1232  
  1233:         if (pos == 0) {

  1319:             if (t_pay_amt == 0 || t_buy_amt == 0) {

  1371  
  1372:         pos = pos == 0 ||
  1373              offers[pos].pay_gem != pay_gem ||

  1412          require(
  1413:             _rank[id].delb == 0 && //assert id is in the sorted list
  1414                  isOfferSorted(id)

contracts/V2Migrator.sol:
  54          require(
  55:             CErc20Interface(bathTokenV2).mint(amountWithdrawn) == 0,
  56              "migrate: MINT FAILED"

  63          require(
  64:             IERC20(bathTokenV2).balanceOf(address(this)) == 0,
  65              "migrate: BATH TOKENS V2 STUCK IN THE CONTRACT"

contracts/periphery/BathBuddy.sol:
  123  
  124:         if (IERC20(myBathTokenBuddy).totalSupply() == 0) {

contracts/utilities/FeeWrapper.sol:
  50      ) external payable returns (bytes memory) {
  51:         if (msg.value == 0) {

contracts/utilities/poolsUtility/Position.sol:
  159:             if (vars.initAssetBalance == 0) {

  269          require(
  270:             CErc20Interface(_cToken).borrow(_amount) == 0,

  294          require(
  295:             CErc20Interface(_bathTokenQuote).repayBorrow(_amountToRepay) == 0,

  308:         require(_err == 0, "_maxBorrow: ERROR");
  309:         require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");
  310:         require(_shortfall == 0, "_maxBorrow: SHORTFALL != 0");
  311  

  335          uint256[] memory _errs = comptroller.enterMarkets(_bathTokens);
  336:         require(_errs[0] == 0);
  337      }

  339      function _exitMarket(address _bathToken) internal {
  340:         require(comptroller.exitMarket(_bathToken) == 0, "_exitMarket: ERROR");

  355          require(
  356:             CErc20Interface(_bathToken).mint(_amount) == 0,

  381          require(
  382:             CErc20Interface(_bathTokenAsset).redeem(_bathTokenAmount) == 0,

  387          if (
  388:             IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
  389:             borrowBalance(_bathTokenAsset) == 0

  537          while (_assetAmount <= _desiredAmount) {
  538:             if (_limit == 0) {

```


### [N-16] NatSpec comments should be increased in contracts

**Context:**
All Contracts

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.
In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.
https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

**Recommendation:**
NatSpec comments should be increased in contracts

### [N-17] `Function writing` that does not comply with the `Solidity Style Guide`

**Context:**
All Contracts

**Description:**
Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.
But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:

 constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private
within a grouping, place the view and pure functions last


### [N-18] Include return parameters in NatSpec comments

**Context:**
All Contracts

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

**Recommendation:**
Include return parameters in NatSpec comments

_Recommendation  Code Style: (from Uniswap3)_
```js
    /// @notice Adds liquidity for the given recipient/tickLower/tickUpper position
    /// @dev The caller of this method receives a callback in the form of IUniswapV3MintCallback#uniswapV3MintCallback
    /// in which they must pay any token0 or token1 owed for the liquidity. The amount of token0/token1 due depends
    /// on tickLower, tickUpper, the amount of liquidity, and the current price.
    /// @param recipient The address for which the liquidity will be created
    /// @param tickLower The lower tick of the position in which to add liquidity
    /// @param tickUpper The upper tick of the position in which to add liquidity
    /// @param amount The amount of liquidity to mint
    /// @param data Any data that should be passed through to the callback
    /// @return amount0 The amount of token0 that was paid to mint the given amount of liquidity. Matches the value in the callback
    /// @return amount1 The amount of token1 that was paid to mint the given amount of liquidity. Matches the value in the callback
    function mint(
        address recipient,
        int24 tickLower,
        int24 tickUpper,
        uint128 amount,
        bytes calldata data
    ) external returns (uint256 amount0, uint256 amount1);
```
### [N-19] Tokens accidentally sent to the contract cannot be recovered

contracts/RubiconMarket.sol

It can't be recovered if the tokens accidentally arrive at the contract address, which has happened to many popular projects, so I recommend adding a recovery code to your critical contracts.

### Recommended Mitigation Steps

Add this code:

```solidity
contracts/RubiconMarket.sol

 /**
  * @notice Sends ERC20 tokens trapped in contract to external address
  * @dev Onlyowner is allowed to make this function call
  * @param account is the receiving address
  * @param externalToken is the token being sent
  * @param amount is the quantity being sent
  * @return boolean value indicating whether the operation succeeded.
  *
 */
  function rescueERC20(address account, address externalToken, uint256 amount) public onlyOwner returns (bool) {
    IERC20(externalToken).transfer(account, amount);
    return true;
  }
}

```

### [N-20] Assembly Codes Specific – Should Have Comments

Since this is a low level language that is more difficult to parse by readers, include extensive documentation, comments on the rationale behind its use, clearly explaining what each assembly instruction does


This will make it easier for users to trust the code, for reviewers to validate the code, and for developers to build on or update the code.

Note that using Aseembly removes several important security features of Solidity, which can make the code more insecure and more error-prone.

```solidity
contracts/RubiconMarket.sol:
  648:         assembly {
  649:             foo := calldataload(4)
  650:             bar := calldataload(36)
  651:             wad := callvalue()
  652:         }

contracts/utilities/poolsUtility/Position.sol:
  366  
  367:         assembly {
  368:             switch _initBathTokenAmount
  369:             case 0 {
  370:                 _bathTokenAmount := _currentBathTokenAmount
  371:             }
  372:             default {
  373:                 _bathTokenAmount := sub(
  374:                     _currentBathTokenAmount,
  375:                     _initBathTokenAmount
  376:                 )
  377:             }
  378:         }

```

### [N-21] For modern and more readable code; update import usages

**Context:**

```solidity

26 results - 6 files

contracts/BathHouseV2.sol:
  3  
  4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
  5: import "./compound-v2-fork/InterestRateModel.sol";
  6: import "./compound-v2-fork/CErc20Delegator.sol";
  7: import "./compound-v2-fork/Comptroller.sol";
  8: import "./compound-v2-fork/Unitroller.sol";
  9: import "./periphery/BathBuddy.sol";
  10  

contracts/RubiconMarket.sol:
   4  // Uncomment this line to use console.log
   5: // import "hardhat/console.sol";
   6  

  10  
  11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
  12: import "@openzeppelin/contracts/utils/StorageSlot.sol";
  13  

contracts/V2Migrator.sol:
  3  
  4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
  5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
  6: import "./compound-v2-fork/CTokenInterfaces.sol";
  7  

contracts/periphery/BathBuddy.sol:
  3  
  4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
  5: import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
  6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";
  7: import "@openzeppelin/contracts/security/Pausable.sol";
  8  

contracts/utilities/FeeWrapper.sol:
  3  
  4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
  5: import "./RubiconRouter.sol";
  6  

contracts/utilities/poolsUtility/Position.sol:
   3  
   4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
   5: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
   6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";
   7: import "@openzeppelin/contracts/access/Ownable.sol";
   8: import "../../compound-v2-fork/Comptroller.sol";
   9: import "../../compound-v2-fork/PriceOracle.sol";
  10: import "../../BathHouseV2.sol";
  11: import "../../RubiconMarket.sol";
```


**Description:**
Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct `polluted the source code` with an unnecessary object we were not using because we did not need it. 
This was breaking the rule of modularity and modular programming: `only import what you need` Specific imports with curly braces allow us to apply this rule better.

**Recommendation:**
`import {contract1 , contract2} from "filename.sol";`

A good example from the ArtGobblers project;
```js
import {Owned} from "solmate/auth/Owned.sol";
import {ERC721} from "solmate/tokens/ERC721.sol";
import {LibString} from "solmate/utils/LibString.sol";
import {MerkleProofLib} from "solmate/utils/MerkleProofLib.sol";
import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
import {ERC1155, ERC1155TokenReceiver} from "solmate/tokens/ERC1155.sol";
import {toWadUnsafe, toDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol";
```
### [N-22] Use a more recent version of Solidity

**Context:**
```solidity
2 results - 2 files

contracts/RubiconMarket.sol:
  1  /// SPDX-License-Identifier: AGPL-3.0
  2: pragma solidity ^0.8.9;
  3  

contracts/periphery/BathBuddy.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity ^0.8.0;

```

**Description:**
For security, it is best practice to use the latest Solidity version.
For the security fix list in the versions;
https://github.com/ethereum/solidity/blob/develop/Changelog.md


**Recommendation:**
Old version of Solidity is used `(0.8.9 – 0.8.0)`, newer version can be used `(0.8.17)` 

### [N-23] For functions, follow Solidity standard naming conventions (internal function style rule)

**Context:**
```solidity
16 results - 2 files

contracts/RubiconMarket.sol:
   35:     function isAuthorized(address src) internal view returns (bool) {
   46:     function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
   50:     function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {
   54:     function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
   58:     function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
   62:     function max(uint256 x, uint256 y) internal pure returns (uint256 z) {
   66:     function imin(int256 x, int256 y) internal pure returns (int256 z) {
   70:     function imax(int256 x, int256 y) internal pure returns (int256 z) {
   77:     function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
   81:     function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
   85:     function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {
   89:     function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {
  227:     uint256 internal feeBPS;
  230:     address internal feeTo;
  232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

contracts/utilities/poolsUtility/Position.sol:
  125:     function openPosition( address asset, address quote, uint256 initMargin,uint256 leverage) internal returns (bool OK) {


```


**Description:**
The above codes don't follow Solidity's standard naming convention,

internal and private functions : the mixedCase format starting with an underscore (_mixedCase starting with an underscore)


### [N-24] Floating pragma

**Description:**
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
https://swcregistry.io/docs/SWC-103

Floating Pragma List: 
```solidity

2 results - 2 files

contracts/RubiconMarket.sol:
  1  /// SPDX-License-Identifier: AGPL-3.0
  2: pragma solidity ^0.8.9;
  3  

contracts/periphery/BathBuddy.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity ^0.8.0;

```


**Recommendation:**
Lock the pragma version and also consider known bugs (https://github.com/ethereum/solidity/releases) for the compiler version that is chosen.

### [N-25] Use SMTChecker

The *highest* tier of smart contract behavior assurance is formal mathematical verification. All assertions that are made are guaranteed to be true across all inputs → The quality of your asserts is the quality of your verification.

https://twitter.com/0xOwenThurm/status/1614359896350425088?t=dbG9gHFigBX85Rv29lOjIQ&s=19

### [N-26] Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length

There are many examples, some of which are listed below;

[RubiconMarket.sol#L9](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L9)

[RubiconMarket.sol#L250](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L250)

[RubiconMarket.sol#L312](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L312)

### [N-27] Use `uint256` instead `uint`

```solidity
4 results - 1 file

contracts/RubiconMarket.sol:
  784          ERC20 buy_gem, //maker (ask) buy which token
  785:         uint pos, //position to insert offer, 0 should be used if unknown

  899:         for (uint i = 0; i < payAmts.length; i++) {
  900              this.offer(

  910      function batchCancel(uint[] calldata ids) external {
  911:         for (uint i = 0; i < ids.length; i++) {

  923      ) external {
  924:         for (uint i = 0; i < ids.length; i++) {


```
Project use uint and uint256
 
Number of uses:
uint  = 4 results
uint256 = 315 results

Some developers prefer to use `uint256` because it is consistent with other uint data types, which also specify their size, and also because making the size of the data explicit reminds the developer and the reader how much data they've got to play with, which may help prevent or detect bugs.

For example if doing ```bytes4(keccak('transfer(address, uint)’))```, you'll get a different method sig ID than ```bytes4(keccak('transfer(address, uint256)’))``` and smart contracts will only understand the latter when comparing method sig IDs


## [N-28] Avoid _shadowing_ `inherited state variables`

**Context:**
```solidity

node_modules/@openzeppelin/contracts/access/Ownable.sol:
  43:     function owner() public view virtual returns (address) {


contracts/RubiconMarket.sol:
  823  
  824:     function offer(
  825:         uint256 pay_amt, //maker (ask) sell how much
  826:         ERC20 pay_gem, //maker (ask) sell which token
  827:         uint256 buy_amt, //maker (ask) buy how much
  828:         ERC20 buy_gem, //maker (ask) buy which token
  829:         uint256 pos, //position to insert offer, 0 should be used if unknown
  830:         bool matching, //match "close enough" orders?
  831:         address owner, // owner of the offer
  832:         address recipient // recipient of the offer's fill
  833:     ) public can_offer returns (uint256) {
  834:         require(!locked, "Reentrancy attempt");
  835:         require(_dust[address(pay_gem)] <= pay_amt);
  836: 
  837:         /// @dev currently matching is perma-enabled
  838:         // if (matchingEnabled) {
  839:         return
  840:             _matcho(
  841:                 pay_amt,
  842:                 pay_gem,
  843:                 buy_amt,
  844:                 buy_gem,
  845:                 pos,
  846:                 matching,
  847:                 owner, // @audit  shadow
  848:                 recipient
  849:             );



contracts/RubiconMarket.sol:
  801      // Make a new offer. Takes funds from the caller into market escrow.
  802:     function offer(
  803:         uint256 pay_amt, //maker (ask) sell how much
  804:         ERC20 pay_gem, //maker (ask) sell which token
  805:         uint256 buy_amt, //maker (ask) buy how much
  806:         ERC20 buy_gem, //maker (ask) buy which token
  807:         uint256 pos, //position to insert offer, 0 should be used if unknown
  808:         address owner,
  809:         address recipient
  810:     ) external can_offer returns (uint256) {
  811:         return
  812:             offer(
  813:                 pay_amt,
  814:                 pay_gem,
  815:                 buy_amt,
  816:                 buy_gem,
  817:                 pos,
  818:                 true,
  819:                 owner, // @audit  shadow
  820:                 recipient
  821:             );
  822:     }


```


`owner` is shadowed, 

The local variable name `owner` in the `RubiconMarket.sol` hase the same name and create shadowing

**Recommendation:**
Avoid using variables with the same name, including inherited in the same contract, if used, it must be specified in the NatSpec comments.

### [N-29] Remove the codes used in the test area in the actual codes


```solidity
contracts/RubiconMarket.sol:
  5: // import "hardhat/console.sol";

```
## [N-30] Include proxy contracts to Audit

The Proxy contract could not be seen in the checklist because it is an upgradable contract, only the implementation contracts are visible, I recommend including the Proxy contract in the audit for integrity in the audit.


## [N-31] Remove NatSpec comments during testing and design

These clutter up the code and make it difficult to control - read the code

```solidity
contracts/RubiconMarket.sol:
  162  
  163:     /// event LogKill(
  164:     ///     bytes32 indexed id,
  165:     ///     bytes32 indexed pair,
  166:     ///     address indexed maker,
  167:     ///     ERC20 pay_gem,
  168:     ///     ERC20 buy_gem,
  169:     ///     uint128 pay_amt,
  170:     ///     uint128 buy_amt,
  171:     ///     uint64 timestamp
  172:     /// );

  184:     /// TODO: double check it is sound logic to kill this event
  185:     /// event LogInt(string lol, uint256 input);
  186: 
  187:     /// event FeeTake(
  188:     ///     bytes32 indexed id,
  189:     ///     bytes32 indexed pair,
  190:     ///     ERC20 asset,
  191:     ///     address indexed taker,
  192:     ///     address feeTo,
  193:     ///     uint256 feeAmt,
  194:     ///     uint64 timestamp
  195:     /// );

  384          emit LogItemUpdate(id);
  385: 
  386:         /// emit LogTake(
  387:         ///     bytes32(id),
  388:         ///     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  389:         ///     _offer.owner,
  390:         ///     _offer.pay_gem,
  391:         ///     _offer.buy_gem,
  392:         ///     msg.sender,
  393:         ///     uint128(quantity),
  394:         ///     uint128(spend),
  395:         ///     uint64(block.timestamp)
  396:         /// );


  409:         /// emit FeeTake(
  410:         ///     bytes32(id),
  411:         ///     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  412:         ///     _offer.buy_gem,
  413:         ///     msg.sender,
  414:         ///     feeTo,
  415:         ///     fee,
  416:         ///     uint64(block.timestamp)
  417:         /// );


  428:         /// TODO: double check it is sound logic to kill this event
  429:         /// emit LogTrade(
  430:         ///     quantity,
  431:         ///     address(_offer.pay_gem),
  432:         ///     spend,
  433:         ///     address(_offer.buy_gem)
  434:         /// );

  542:         /// emit LogMake(
  543:         ///     bytes32(id),
  544:         ///     keccak256(abi.encodePacked(pay_gem, buy_gem)),
  545:         ///     msg.sender,
  546:         ///     pay_gem,
  547:         ///     buy_gem,
  548:         ///     uint128(pay_amt),
  549:         ///     uint128(buy_amt),
  550:         ///     uint64(block.timestamp)
  551:         /// );


```
## [N-32] In 0.8>= pragma versions, the use of SafeMath is unnecessary

Although pragma version 0.8 and above is used in the project, SafeMath library is used, it is not necessary, but if there is a use due to interaction with other contracts, it should be stated in the NatSpec comments and documents.

```solidity

contracts/RubiconMarket.sol:
  43  
  44: /// @notice DSMath library for safe math without integer overflow/underflow
  45: contract DSMath {
  46      function add(uint256 x, uint256 y) internal pure returns (uint256 z) {

```

### [N-33] Use the `delete` keyword instead of assigning a value of `0`


Using the 'delete' keyword instead of assigning a '0' value is a detailed optimization that increases code readability and audit quality, and clearly indicates the intent.

Other hand, if use `delete` instead `0` value assign , it will be gas saved

```diff
contracts/periphery/BathBuddy.sol:
  179          uint256 reward = tokenRewards[address(rewardsToken)][holderRecipient];
  180:         if (reward > 0) {
- 181:             tokenRewards[address(rewardsToken)][holderRecipient] = 0;
+ 	     delete tokenRewards[address(rewardsToken)][holderRecipient];
  182:             rewardsToken.safeTransfer(holderRecipient, reward);
  183:             emit RewardPaid(holderRecipient, reward);
  184:         }


```


